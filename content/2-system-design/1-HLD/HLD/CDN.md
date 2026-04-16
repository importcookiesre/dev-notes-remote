
- Learn about Dynamic Content Caching

- If the CDN server does not have the file in the cache, the CDN server request the file from the origin, which can be our web server or some object storage (S3).
	- The origin returns the file which also contains optional TTL in the headers which describes how long it will live in the CDN.



## 1. Consideration using CDN.

### 1. CDN fallback : 

We should consider how your website/application copes with CDN failure. If there is a temporary CDN outage, clients should be able to detect the problem and request resources from the origin.
