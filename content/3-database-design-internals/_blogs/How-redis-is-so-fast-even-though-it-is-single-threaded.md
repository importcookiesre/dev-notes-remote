
 - resource : 
	 - [medium blog](https://medium.com/javarevisited/interview-why-is-redis-so-fast-even-though-it-is-single-threaded-bfe663dabe08)

The Reasons are : 
- In memory operations : - Redis is a RAM-based database. RAM access is at least 1000 times faster than random disk access.
- Optimized data structure and algorithms.:
	- It is designed around highly optimized, simple data structures like strings, hashes, lists, sets etc.
- Single Threaded Architecture : The single theaded model eliminates the overhead which are generally associated with multhreaded system like locks, context switching, and synchronization issues.
- Event Loop and Non Blocking I/O : It uses event loop and IO multiplexing to handle multiple client connections asynchronously, will alows to serve multiple requests without blocking the thread.
- File read and write : While operations are in-memory, Redis provides persistence options (AOF and RDB snapshots) that are typically handled in the background by separate processes to avoid blocking the main server thread.



## intro :
While scrolling through X, I came across a question asking why Redis is so fast, even though it runs on a single-thread. That question made me pause. I went down the rabbit hole reading blogs, articles, and discussions, and this post is the result of that curiosity and digging into how Redis actually works under the hood.

Redis is a great example of a personal project that grew into a leading technology used by companies like MAANG. But what made it so special?

Redis is known for its exceptional performance, primarily due to its in-memory data storage. Redis is not just a cache. It's versatile enough to be used as a primary database, a caching layer, a message broker, and a queue.


![[Pasted image 20260208234839.png]]
# Why is Redis so popular ? 

Redis is a great example of a personal project that grew into a leading technology used by companies like MAANG. But what made it so special?

Redis is not just a cache. It's versatile enough to be used as a primary database, a caching layer, a message broker, and a queue. This flexibility has enabled it to fit into various architectural needs, making it a popular choice among developers working on complex applications.

Redis is known for its exceptional performance, primarily due to its in-memory data storage. By storing data directly in RAM, Redis can read and write at a speed much higher than database that rely on disk storage. This allows it to handle million of requests per second with minimum latency.

Unlike many other key-value stores, Redis supports a variety of data structures such as strings, lists, sets, hashes, sorted sets, bitmaps, and geospatial indexes. This variety allows developers to use Redis for a wide range of use cases beyond simple caching, including message brokering, real-time analytics, and session management.

Being open source has allowed Redis to benefit from contributions from a global developer community, which has helped in enhancing its features and capabilities over time.




# Why is Redis so fast ?


![[Pasted image 20260209003124.png]]
### 1. In memory storage : 
![[Pasted image 20260209003129.png]]
One of the main reasons why Redis is so fast is that it stores all of its data in memory, rather than relying on disk storage. This drastically reduces the time it takes to access data, as memory is significantly faster than traditional hard drives.

On an average, one Memory opeartion takes 100ns while one disk operation takes around 100 000ns which is 1000x more than a typical memory operation.


### 2. Single Threaded Event Loop : 

Redis uses a single-threaded event loop to handle all client requests. This design simplifies the processing model and avoids the overhead associated with multithreading like context switching and locking. 
Since all commands are processed sequentially, there is never more than one command being processed at any time, which eliminates race conditions and locking delays.

Redis can perfrom 1.5 million RPC even on an entry level config linux machine.


### 3. I/O Multiplexing : 

 Redis uses a technique called I/O multiplexing, which allows it to handle thousands of simultaneous connections with a single thread. This technique lets Redis efficiently juggle multiple requests without the performance hit that often comes with managing multiple threads.

### 4. Pipelining : 

Redis supports pipelining, which allows to send multiple commands at once, reducing the latency costs associated with round trip times.


### 5. Built in replication and clustering : 

For scalability, redis offers built in replication and support for clustering. This allows redis to handle more data and more operations by distributing the load across multiple nodes, and each of them can be optimized for performance.


### 6. Persistence Options : 


Redis provides different options for data persistence, allowing it to balance between performance and durability requirements. For example, the Append Only File (AOF) can be configured to append each operation to a log, which can be synchronized with the disk at different intervals according to the desired durability level.

### 7. Rich data structure
Redis supports several data structures like strings, hashes, lists, sets, and sorted sets, each optimized for efficient access and manipulation. For instance, adding an element to a Redis list is an O (1) operation, meaning it executes in constant time regardless of the list size.
![[Pasted image 20260209003148.png]]


## conclusion :

Understanding these internals also makes one thing clear:  It is a well engineered system that stays focused on doing a few things extremely well.

**Finally, if the article was helpful, please upvote 👏and follow, thank you!**