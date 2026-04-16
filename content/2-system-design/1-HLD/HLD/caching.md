
- L1 cache is the fastest, then L2, L3



![[Pasted image 20251226185714.png]]


## Consideration for using cache : 

### 1. Decide when to use cache : 


### 2. Expiration Policy


### 3. Consistency

When scaling across multiple regions, maintaining consistency between the data stores and cache is challenging.
This involves keeping the data store and the cache in sync. Inconsistency can happen because data-modifying operations on the data store and cache are not in a single transaction.

### 4. Mitigating failures : 

- A single cache server is vernarable to Single Point Of Failure (SPOF).
	- As a result, multiple cache servers are recommanded to avoid SPOF.


### 5. Eviction Policy : 

Once the cache is full, any requests to add items to the cache might cause existing items to be removed. This is called cache eviction. ==Least-recently-used== (LRU) is the most popular cache eviction policy. Other eviction policies, such as the Least Frequently Used (LFU) or First in First Out (FIFO), can be adopted to satisfy different use cases


## Scaling cache server : 






