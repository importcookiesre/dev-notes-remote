



## 1. What is distributed system ?

A distributed system is a collection of independent computers (nodes) connected via a network that function together as a single, cohesive system to achieve a shared goal.

It appears as a single node to the end user.



## 2. Why we need a distributed system and what problems it solves ?

Instead of relying on one massive, expensive server to do all the work (a monolithic architecture), a distributed system breaks the machine into multiple smaller nodes.

CPU clock speeds are barely increasing, but multi-core processors are standard, and networks are getting faster. This means parallelism is only going to increase the performance and handle massive load.

Managing multiple computer over a network is incredibly complicated.


### 2. A distributed system solves three major problems :

#### 1. Scalability (Horizontal Scaling) : 

When the traffic spikes, we can simply add more nodes (containers/servers) to the pool rather than upgrading a single machine's CPU and RAM.

#### 2. High availability and Fault Tolerance :

It solves the ==Single Point of Failure== problem.

In a monolithic architecture, if the server crashes, then the whole system goes down. 
In a distributed system, if one of the node goes down, the load balancer simply sends request to the other nodes in the pool and the systems keeps serving requests without any problem.


#### 3. Low Latency (Geography): 

Data takes time to travel through fiber optic cables. A distributed system allows you to place servers globally (e.g., in India, the US, and Europe) so users are always hitting a server geographically close to them.





## 3. Problem comes with Distributed System

The fundamental problems that come with distributed systems, often famously summarized by the **Fallacies of Distributed Computing**.


### 1. The Network unreliability and latency.

In a single application, calling a function is virtually instantaneous and guaranteed to reach the code. In a distributed system, a function call becomes a network request.

- **Packets get lost:** A router might drop your request.
- **latency spikes :** A call that usually takes 5ms might suddenly take 5000ms.
- **The "Two General" problem :** If you send a request to a payment service and get no response, you have no idea if the request failed to reach the service, if the service crashed while processing it, or if the response simply got lost on the way back.


### 2. State Consistency vs Availability (The CAP theorem)

Because networks fail, our system will eventually experience a **Partition** (nodes lose the ability to talk to each other). When a partition happens, we have to make a painful choice for our data:

- **Consistency:** Reject read/write requests until the network heals, ensuring no user ever sees outdated data (our system goes down, but data is safe).
    
- **Availability:** Keep serving requests using the data each node currently has, even if it means User A sees a different account balance than User B (your system stays up, but data is temporarily inaccurate). 

You cannot have both during a network failure.

### 3. The "Split-Brain" problem :

Imagine a primary database node and a replica node. If the network cable between them is cut, the replica might think, "The primary is dead, I must become the new primary." Now we have _two_ primary nodes blindly accepting writes. When the network heals, their data is fundamentally conflicted. Solving this requires complex **Consensus Algorithms** (like Raft or Paxos) where a majority of nodes must "vote" on who the true primary is.


### 4. Time and ordering problem : 

We cannot trust timestamps. Because every server has its own physical quartz clock, their times will inevitably drift apart, even with synchronization protocols like NTP.
If Server A writes to PostgreSQL at `10:00:00.001` and Server B writes at `10:00:00.002`, we cannot definitively prove Server A's event happened first in the real world. 
This makes resolving concurrent database updates incredibly difficult. Distributed systems have to rely on **Logical Clocks** (counters that track the sequence of events) rather than wall-clock time.


### 5. Parital Failures : 

In a monolith, if the machine runs out of memory, the whole system crashes. It's a total failure, which is actually easy to understand. 
In a distributed architecture, three out of our ten Docker containers might run out of memory, while the rest keep functioning. The system is now limping. Requests routed to the dead containers will timeout, causing cascading failures as healthy services exhaust their own resources waiting for the dead ones to respond.

### 6. Debugging is a Nightmare :

When a user clicks a button and gets an "Internal Server Error", figuring out why is no longer a matter of reading a single stack trace. 
That request might have touched an API gateway, an auth service, a user service, and a database. We now have to piece together logs from multiple different machines to trace the execution path.




## 4. Data Intensive vs Compute Instensive ?

Data-instensive means that the data is the primary challenge : the quantity of the data, the complexity of the data and speed at which data is changing.

Compute-Intensive mean the primary challenge is the computation.  