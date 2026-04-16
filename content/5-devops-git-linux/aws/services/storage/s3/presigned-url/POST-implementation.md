
## 1. Create a user using IAM  with access to S3.

## 2. configure the bucket. 








Inject the credentials : 
```go
export AWS_ACCESS_KEY_ID=...
export AWS_SECRET_ACCESS_KEY=...
export AWS_SESSION_TOKEN=...   # optional
export AWS_REGION=ap-south-1
```



```go
package main

import (
	"bytes"
	"context"
	"crypto/hmac"
	"crypto/sha256"
	"encoding/base64"
	"encoding/hex"
	"encoding/json"
	"errors"
	"fmt"
	"log"
	"net/http"
	"path/filepath"
	"strings"
	"time"

	"github.com/aws/aws-sdk-go-v2/aws"
	"github.com/aws/aws-sdk-go-v2/config"
)

type PresignedPostResponse struct {
	URL    string            `json:"url"`
	Fields map[string]string `json:"fields"`
}

type PresignPostInput struct {
	Bucket      string
	Region      string
	Key         string
	ExpiresIn   time.Duration
	MinSize     int64
	MaxSize     int64
	ContentType string            // exact match; leave empty if you do not want to enforce it
	ACL         string            // optional, e.g. "private" or "public-read"
	Metadata    map[string]string // keys like "x-amz-meta-user-id"
}

type postPolicy struct {
	Expiration string            `json:"expiration"`
	Conditions []json.RawMessage `json:"conditions"`
}

func main() {
	ctx := context.Background()

	cfg, err := config.LoadDefaultConfig(ctx)
	if err != nil {
		log.Fatal(err)
	}

	http.HandleFunc("/upload/presign", func(w http.ResponseWriter, r *http.Request) {
		if r.Method != http.MethodPost {
			http.Error(w, "method not allowed", http.StatusMethodNotAllowed)
			return
		}

		// Example request body:
		// {
		//   "filename":"avatar.png",
		//   "content_type":"image/png",
		//   "size":123456
		// }
		var req struct {
			Filename    string `json:"filename"`
			ContentType string `json:"content_type"`
			Size        int64  `json:"size"`
		}

		if err := json.NewDecoder(r.Body).Decode(&req); err != nil {
			http.Error(w, "invalid json", http.StatusBadRequest)
			return
		}

		if req.Filename == "" || req.Size <= 0 {
			http.Error(w, "filename and size are required", http.StatusBadRequest)
			return
		}

		// Keep the object key server-controlled. A UUIDv7 prefix works well in production.
		safeName := filepath.Base(req.Filename)
		objectKey := fmt.Sprintf("uploads/%s", safeName)

		post, err := GeneratePresignedPost(ctx, cfg, PresignPostInput{
			Bucket:      "your-bucket-name",
			Region:      "ap-south-1",
			Key:         objectKey,
			ExpiresIn:   15 * time.Minute,
			MinSize:     1,
			MaxSize:     10 * 1024 * 1024, // 10 MiB
			ContentType: req.ContentType,
			ACL:         "private",
			Metadata: map[string]string{
				"x-amz-meta-uploaded-by": "backend",
			},
		})
		if err != nil {
			http.Error(w, err.Error(), http.StatusInternalServerError)
			return
		}

		w.Header().Set("Content-Type", "application/json")
		_ = json.NewEncoder(w).Encode(post)
	})

	log.Println("listening on :8080")
	log.Fatal(http.ListenAndServe(":8080", nil))
}

func GeneratePresignedPost(ctx context.Context, cfg aws.Config, in PresignPostInput) (*PresignedPostResponse, error) {
	if in.Bucket == "" || in.Region == "" || in.Key == "" {
		return nil, errors.New("bucket, region, and key are required")
	}
	if in.ExpiresIn <= 0 {
		in.ExpiresIn = 15 * time.Minute
	}
	if in.MaxSize <= 0 || in.MaxSize < in.MinSize {
		return nil, errors.New("invalid size range")
	}

	creds, err := cfg.Credentials.Retrieve(ctx)
	if err != nil {
		return nil, fmt.Errorf("retrieve credentials: %w", err)
	}
	if creds.AccessKeyID == "" || creds.SecretAccessKey == "" {
		return nil, errors.New("missing AWS credentials")
	}

	now := time.Now().UTC()
	dateStamp := now.Format("20060102")
	amzDate := now.Format("20060102T150405Z")
	expiration := now.Add(in.ExpiresIn).UTC().Format("2006-01-02T15:04:05.000Z")

	scope := fmt.Sprintf("%s/%s/s3/aws4_request", dateStamp, in.Region)
	credential := fmt.Sprintf("%s/%s", creds.AccessKeyID, scope)

	conditions := []json.RawMessage{
		mustJSON(map[string]string{"bucket": in.Bucket}),
		mustJSON(map[string]string{"key": in.Key}),
		mustJSON(map[string]string{"x-amz-algorithm": "AWS4-HMAC-SHA256"}),
		mustJSON(map[string]string{"x-amz-credential": credential}),
		mustJSON(map[string]string{"x-amz-date": amzDate}),
		contentLengthRangeCond(in.MinSize, in.MaxSize),
	}

	if in.ACL != "" {
		conditions = append(conditions, mustJSON(map[string]string{"acl": in.ACL}))
	}
	if in.ContentType != "" {
		conditions = append(conditions, mustJSON(map[string]string{"Content-Type": in.ContentType}))
	}
	if creds.SessionToken != "" {
		conditions = append(conditions, mustJSON(map[string]string{"x-amz-security-token": creds.SessionToken}))
	}
	for k, v := range in.Metadata {
		if !strings.HasPrefix(strings.ToLower(k), "x-amz-meta-") {
			k = "x-amz-meta-" + k
		}
		conditions = append(conditions, mustJSON(map[string]string{k: v}))
	}

	policy := postPolicy{
		Expiration: expiration,
		Conditions: conditions,
	}

	policyJSON, err := json.Marshal(policy)
	if err != nil {
		return nil, fmt.Errorf("marshal policy: %w", err)
	}

	policyB64 := base64.StdEncoding.EncodeToString(policyJSON)
	signingKey := deriveSigningKey(creds.SecretAccessKey, dateStamp, in.Region, "s3")
	signature := hex.EncodeToString(hmacSHA256(signingKey, policyB64))

	fields := map[string]string{
		"key":             in.Key,
		"Policy":          policyB64,
		"X-Amz-Algorithm":  "AWS4-HMAC-SHA256",
		"X-Amz-Credential": credential,
		"X-Amz-Date":      amzDate,
		"X-Amz-Signature":  signature,
	}

	if creds.SessionToken != "" {
		fields["X-Amz-Security-Token"] = creds.SessionToken
	}
	if in.ACL != "" {
		fields["acl"] = in.ACL
	}
	if in.ContentType != "" {
		fields["Content-Type"] = in.ContentType
	}
	for k, v := range in.Metadata {
		if !strings.HasPrefix(strings.ToLower(k), "x-amz-meta-") {
			k = "x-amz-meta-" + k
		}
		fields[k] = v
	}

	// Use the bucket endpoint for the form action.
	url := fmt.Sprintf("https://%s.s3.%s.amazonaws.com/", in.Bucket, in.Region)

	return &PresignedPostResponse{
		URL:    url,
		Fields: fields,
	}, nil
}

func mustJSON(v any) json.RawMessage {
	b, err := json.Marshal(v)
	if err != nil {
		panic(err)
	}
	return b
}

func contentLengthRangeCond(min, max int64) json.RawMessage {
	b, _ := json.Marshal([]any{"content-length-range", min, max})
	return b
}

func deriveSigningKey(secret, date, region, service string) []byte {
	kDate := hmacSHA256([]byte("AWS4"+secret), date)
	kRegion := hmacSHA256(kDate, region)
	kService := hmacSHA256(kRegion, service)
	kSigning := hmacSHA256(kService, "aws4_request")
	return kSigning
}

func hmacSHA256(key []byte, data string) []byte {
	mac := hmac.New(sha256.New, key)
	_, _ = mac.Write([]byte(data))
	return mac.Sum(nil)
}
```




## Response from AWS : 

### success :

- the file location is `https://<bucket>.s3.<region>.amazonaws.com/<key>`
- 

|S.No|Status Code|Response Type|When It Happens|Response Body|
|---|---|---|---|---|
|1|`204 No Content`|Default success|No special field set|Empty|
|2|`200 OK`|Custom success|`success_action_status=200`|Empty|
|3|`201 Created`|Detailed success|`success_action_status=201`|XML with `Bucket`, `Key`, `ETag`, `Location`|
|4|`303 Redirect`|Redirect success|`success_action_redirect` set|Redirect to given URL|


### error : 

| S.No | Status Code | Error Code                            | Cause               | What It Means                  |
| ---- | ----------- | ------------------------------------- | ------------------- | ------------------------------ |
| 1    | `400`       | `InvalidPolicyDocument`               | Wrong policy JSON   | Policy malformed or invalid    |
| 2    | `400`       | `MalformedPOSTRequest`                | Bad form-data       | Missing/invalid fields         |
| 3    | `400`       | `EntityTooLarge`                      | File too big        | Exceeds `content-length-range` |
| 4    | `400`       | `EntityTooSmall`                      | File too small      | Below allowed size             |
| 5    | `400`       | `IncorrectNumberOfFilesInPostRequest` | Multiple/no files   | Exactly 1 file required        |
| 6    | `400`       | `ExpiredToken`                        | Expired credentials | Temporary creds expired        |
| 7    | `403`       | `AccessDenied`                        | Permission issue    | IAM/bucket policy denies       |
| 8    | `403`       | `SignatureDoesNotMatch`               | Signature mismatch  | Fields/policy altered          |
| 9    | `403`       | `RequestTimeTooSkewed`                | Clock mismatch      | Server/client time drift       |
| 10   | `403`       | `InvalidAccessKeyId`                  | Wrong key           | Access key doesn’t exist       |
| 11   | `403`       | `AccessControlListNotSupported`       | ACL disabled        | Bucket owner enforced mode     |
| 12   | `5xx`       | `InternalError`                       | AWS issue           | Rare internal failure          |
