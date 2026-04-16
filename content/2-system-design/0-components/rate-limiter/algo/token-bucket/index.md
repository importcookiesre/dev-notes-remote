Build a rate limiter
	Requirements: 
		1. Limit requests per user (e.g., 100 requests/min). 
		2. Limit requests globally (e.g., 1M requests/sec across all users).



The Token Bucket Algorithm works by imagining a bucket that fills up with tokens at a constant rate.

- The token bucket algorithm allocates tokens at a fixed rate into a "bucket."
- Each request consumes a cetain number of token from the bucket, and requests are only allowed if there are sufficient tokens available.
- Unused tokens are stored in the bucket, up to a maximum capacity.
- It protects from a bursts request.

- Advantages : 
	- Memory efficient : One advantage of the token bucket algorithm is that it is memory efficient, as it only requires a fixed number of tokens to be stored in memory. This can be important in systems with limited memory resources.
- Disadvantages : 
	- the token bucket algorithm is susceptible to race conditions, which can occur when multiple threads or processes attempt to access the same resource simultaneously.
