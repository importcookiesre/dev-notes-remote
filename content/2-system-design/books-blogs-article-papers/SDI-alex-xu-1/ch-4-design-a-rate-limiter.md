

# Design a rate limiter : 



## benefits : 

1. Prevent resource starvation caused by Denial of Service (DoS) attack.
2. Reduces cost. Supressing excess requests means fewer computation and now we can allocated resources to high priority services.
3. Prevent server from being overloaded. Protects from excessive request from bots and users.


## Design :


### Step 1. Understanding the problem and establish design scope :


Rate limiting can be implemented using different algorithms, each with its pros and cons. The interactions between an interviewer and a candidate help to clarify the type of rate limiters we are trying to build.

**Candidate :** What kind of rate limiter are we going to design? Is it a client-side rate limiter or server-side API rate limiter?
**Interviewer :** Great question. We focus on the server-side API rate limiter.

**Candidate :** Does the rate limiter throttle API requests based on IP, the user ID, or other properties?
**Interviewer :** The rate limiter should be flexible enough to support different sets of throttle rules.

**Candidate :** What is the scale of the system? Is it built for a startup or a big company with a large user base?
**Interviewer :** The system must be able to handle a large number of requests.

**Candidate :** Will the system work in a distributed environment?
**Interviewer :** yes

**Candidate :** Is the rate limiter a separate service or should it be implemented in application code?
**Interviewer :** It is a design decision up to you.

**Candidate :** Do we need to inform users who are throttled?
**Interviewer :** Yes


With this interaction, here's the requirement summary that we can gather :
- Accurately limit excessive request.
- Low latency. The rate limiter should not slow down the HTTP request.
- Distributed rate limiting. It will be used accross multiple services or processes.
- Exception handling. Show clear response that the request is throttled.
- High fault tolerance. If there is any problem with the rate limiter, it should not affect the entire system.



### Step 2: Propose high-level design and get buy-in

`buy-in` : gain acceptance, active participation, support


1. Where to put the rate limiter ? 
	1. Api server
	2. Middleware
	3. API gateway - 
		1. Api gateway is a fully managed service that support rate limiting, SSL termination, caching, IP whitelisting, serving static content etc.
- There is no definite answer where to put a rate limiter, it depend on the system architecture, tech stack, goal etc.		   

- You shoudl know Why client side rate limiter is not reliable ?

#### Algorithms for rate limiting :

- There is no best algo, they depends on the use cases. But obviosuly for every use case there will atleast one fit in the service.

- Dive into each of them based on the goal, explain their pros and cons,: 
##### 1. Token Bucket algorithm : 

##### 2. Leaking Bucket algorithm :

##### 3. Fixed window counter algorithm : 

##### 4. Sliding window log algo :

##### 5. Sliding window counter algo : 



#### High level architecture : 

The basic idea of rate limiting algorithms is simple. At the high-level, we need a counter to keep track of how many requests are sent from the same user, IP address, etc. If the counter is larger than the limit, the request is disallowed.

**Now the question is where should we keep the counter ?**
Using the database is not a good idea due to slowness of disk access. In-memory cache is chosen because it is fast and supports time-based expiration strategy. For instance, Redis is a popular option to implement rate limiting. It is an inmemory store that offers two commands: INCR and EXPIRE.

![[Pasted image 20251229175930.png]]


• The client sends a request to rate limiting middleware. 
• Rate limiting middleware fetches the counter from the corresponding bucket in Redis and check if the limit is reached or not :
- If the limit is reached, the request is dropped
- Else the request is served and the rate limiter counter is updated.


#### Step. 3 : Design deep dive : 

The high level design does not answers the following questions : 
- How rate limiter rules are created ? Where the rules are stored ?
- How to handle the rejected requests ?

First we will try to answer the rate limiting rules  then we will move over the strategies to handle rate limiting requests.

- Talk about the rate limiting rules, like where they are stores whether in a json/yaml file. Discuss about some of the rules, with examples.
- Them move to rate limiting strategies : how rejected requests are treated ;
	- We can send response of a request with 429 status code with suitable headers
	- Or we can queue it and process it. For example : we dont want to loose payment, order requests etc.

![[Pasted image 20251229181544.png]]
- Rules are stored on the disk. Workers frequently pull rules from the disk and store them in the cache.
- When a client sends a request to the server, the request is sent to the rate limiter middleware first.
- Rate limiter middleware loads rules from the cache. It fetches counters and last request timestamp from Redis cache. Based on the response, the rate limiter decides:
- if the request is not rate limited, it is forwarded to API servers.
- if the request is rate limited, the rate limiter returns 429 too many requests error to the client. In the meantime, the request is either dropped or forwarded to the queue


##### scaling it to support distributed systems :

Building a rate limiter that works in a single server environment is not difficult. However, scaling the system to support multiple servers and concurrent threads is a different story. 
There are two challenges:
- Race condition :
	- One value is updated by multiple threads at the same time leading to calculation error.
	- Solution : Use locks
- Synchronization issue :
	- In a distributed rate limiter, we dont want multiple rate limiter to have its own storage. Otherwise, if i have 2 requests and both goes throught different rate limiter then both will have two different states of a single user and will violate intigrity.
solution is :
- sticky session : it allows a client to send request to same rate limiter
- centralized storage : redis
![[Pasted image 20251229182806.png]]


##### Performance optimizaton : 

- multiple data center setup : 
	- traffic is routed to the closest server.
	- synchronize data with eventual consistency model.


##### Monitoring :

After the rate limiter is put in place, it is important to gether analytics to check whether it is working properly or not :

we can check if:
- The rate limiter algo. is effective
- The rules are effective

For example, if rate limiting rules are too strict, many valid requests are dropped. In this case, we want to relax the rules a little bit. In another example, we notice our rate limiter becomes ineffective when there is a sudden increase in traffic like flash sales. In this scenario, we may replace the algorithm to support burst traffic.



### Wrap up :

- Here you can  :
	- give a summary / revice.
	- ask if the interviwer want to dig deeper into any component.
 