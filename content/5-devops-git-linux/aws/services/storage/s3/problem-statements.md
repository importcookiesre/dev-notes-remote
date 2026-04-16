
1. In an S3 presigned POST upload, if the policy enforce Content-Type: `application/pdf` and the object key is `uploads/book.pdf`, but the client uploads a non-PDF ( `malicious.exe`) file with a matching content type i.e `application/pdf`.
	1. will S3 validate the actual file contents or only the request fields? 
	2. What are the security implications, and what is the recommended way to handle this safely? 
	3. How to implement secure file upload pipeline with incoming verified and rejected states.




