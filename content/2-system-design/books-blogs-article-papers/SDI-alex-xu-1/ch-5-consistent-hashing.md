

Consistent hashing is a commonly used technique to distribute request or load effectively in a distributed system / horizontal scaling.

Now why we need consistent hashing ? :
- It is used to Minimize key redistribution on addition or removal of nodes/servers.
- It is easy to scale horizontally as the data are more eventually distributed.
- Mitigate hotspot key problem. Excessive access to a specific shard could cause server overload. Imagine data for Katy Perry, Justin Bieber, and Lady Gaga all are present on the same shard. Consistent hashing helps to mitigate the problem by distributing the data more evenly.

## Problem : 

If you have `n` cache servers, a common way to balance the load is to use the following hash method: 
- So, we first hash the key and the we mod (%) the hash by size of pool.
`serverIndex = hash(key) % N`, where N is the number of cache servers in the pool.
- here the key could be an IP address, userId etc.

For example, if we have 4 serves and on every request we first find out where to send the request by using the prescibed method. 
The load will be eventually distributed.

![[Pasted image 20251230170614.png]]

This works well when the size of the pool is constant and load is also eventually distributed.

However, the **problem** arrives when the nodes are added or removed. For example the server 1 goes offline, now the pool size becomes 3. The hash value of the key remains same, but the server index to those hashes changes.

![[Pasted image 20251230171024.png]]

Most keys are redistributed. This means that when server 1 goes offline, most cache clients will connect to the wrong servers to fetch data.
![[Pasted image 20251230171151.png]]

Consistent hashing is an effective technique to mitigate this problem.


## Consistent hashing : 

- Only `K/N` keys has to remapped on average, where `K`  is the number of keys and `n` is the number of slots. While in case of traditional hash table almost all keys has to remapped.



- hash function generate values which are in range like SHA-1 : 0 - 2^160 -1
	- This mean that it can be 0 also

The basic steps are: 
- Map servers and keys on to the ring using a uniformly distributed hash function.
- To find out which server a key is mapped to, go clockwise from the key position until the first server on the ring is found.


- on adding or removal of Node, all the keys between that node and its previous nodes are effected .
![[Pasted image 20251230173125.png]]

### Issues with the basic approach of the consistent hashing :

1. inconsistency in the size of the partition (size of the arc between two nodes). Some partition may be very small or very large. One server will get more request while other may sit ideal.
2. Non uniform key distribution. 
![[Pasted image 20251230173404.png]]


### solution : virtual nodes / replica

Many virtual nodes are created for each server or simply each server is represented by multiple virtual nodes on the ring.
so instead of `s0` representing server 0, we have `s0_0, s0_1, s0_2, ...`
![[Pasted image 20251230173600.png]]

As the number of virtual nodes increases, the distribution becomes more balanced.