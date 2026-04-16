

- The first step of scaling a system is to seperate web/mobile traffic and database traffic servers which allows them to scale independently.



## 1. vertical scaling vs horizontal scaling

- Vertical scaling means `scale up` which means scaling a server by adding more power (CPU, RAM, etc) to the server.
	- When traffic is low, vertical scaling is the best option. The simplicity of it is its greatest advantage.
	- Unfortunality it has some serious limitation : 
		- Vertical scaling has a hard limit. It is imposible to add unlimited CPU and memory to a single server.
		- Vertical scaling does not have failover and redundancy. If the server goes down, the whole service goes shut down.
- Horizontal scaling means `scale out` which means scaling a server by adding more servers into the pool of servers.

- redundancy : the state of being no longer in employment because there is no more work available.
## 2. load balancer : 

![[Pasted image 20251226183312.png]]
For better security, private IPs are used for communication between servers. A private IP is an IP address which is only reachable to the servers which are connection on the same network and unreachable over the internet. The load balancer communicates with the server through the private IPs.






