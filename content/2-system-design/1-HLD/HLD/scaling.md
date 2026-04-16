

## stateless and stateful

### stateless web tier : 

For scaling the web tier horizontally, we need to move state out of the web tier. A good practice is to store session data in the persistent storage such as relational database or NoSQL.

### stateful architecture : 

A stateful server and stateless server has some key differences. A stateful server remembers client data (state) from one request to the next. 
![[Pasted image 20251226192030.png]]
- ==The issue is that every request from the same client must be routed to the same server.==
	- User A’s session data and profile image are stored in Server 1. To authenticate User A, HTTP requests must be routed to Server 1. If a request is sent to other servers like Server 2, authentication would fail because Server 2 does not contain User A’s session data. Similarly, all HTTP requests from User B must be routed to Server 2; all requests from User C must be sent to Server 3.
- This can be solved using sticky sessions in most load balancers, however its add overhead.
### stateless architecture : 

![[Pasted image 20251226192338.png]]
In stateless architecture, HTTP request of a client can be send handled by any server, which fetches data from a shared data store.

State data is stored in a shared data store and kept out of web servers.

![[Pasted image 20251226192706.png]]



## scaling across data centers : 

In normal operations, traffic is geoDNS-routed also known as geo-routed, to the closest data center.
geoDNS is a DNS service that allows domain names to be resolved to IP addresses based on the location of a user.

![[Pasted image 20251226193309.png]]


### In case of outage :
In the event of any significant data center outage, we direct all traffic to a healthy data center.

![[Pasted image 20251226193342.png]]

### Challenges must be resolved to achieve multi data center setup : 

#### 1. Traffic redirection : 
Effective tools are needed to direct traffic to the correct data center. GeoDNS can be used to direct traffic to the nearest data center depending on where a user is located.

#### 2. Data Synchronization : 

Users from different regions could use different local databases or caches. In failover cases, traffic might be routed to a data center where data is unavailable. A common strategy is to replicate data across multiple data centers. A previous study shows how Netflix implements asynchronous multi-data center replication.
#### 3. Test and deployment : 





