
AWS supports multipart upload for large files, where parts can be uploaded independently and parallel.


> [!question] What will happen if a user upload files multiple time in the same URl ?
> Multiple uploads on the same presigned url overrides the file in the s3 and so only a single upload is allowed.


> [!question] If a signed upload URL expires during the transfer, the outcome depends on the type of upload being performed: 
> 1. Single PUT Uploads : the upload will **continue to completion**. The storage service (such as Amazon S3) typically validates the URL's expiration when the request is **initiated**.
> 2. Each part of a multipart upload requires its own validation. If the URL for a specific part expires before that specific part's upload begins, that step will **fail with a 403 Forbidden error**.


## How PUT works ? 

