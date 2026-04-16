


## High level thinking : First Principle

- Break the problem to the fundamental and solve them from the ground up. (divide and conquer)
- Write the goal, constaints and tradeoffs.
- Iterate: aim for the simplest thing that meets the requirements, then evolve.


## How to approach System Design ?

These are very similar yet different way of approaching a Designing a System : 
Given a problem statement :
1. Pick `1. Build in Layers` if you know the solution very well, you are pretty confident.
2. Otherwise Go with `2. Incremetal MVP approach`, build the minimal working model then scale as per requirements.

### 1. Built in Layers :

You break down the system into layer and then build one by one.
When to use this ?
- When the problem statement is simple.
- You are experience enough and pretty confident on the decision you are making.
- 
Eg : 
1. Build the database layer
2. Add bunch of APIs for communication.
3. Add payment gateway
4. Add queue for async operations.

![[Pasted image 20260309155253.png]]

### 2. Incremental MVP approach : 

You start with a simple and basic `Day 0` architecture that works. Over the time you scale each component based on requirements.
Eg : A Sql db, api server and a user.
- Now as you get more user, you add a few servers with a load balancer.
- You get even more users, add few Read replica to handle the load.
- Then you add a message broker to handle asynchronous operations.

![[Pasted image 20260309160022.png]]

## Key Points : 

1. Every system is infinitely buildable : Fence the system
	1. Scope the design to ship it within the deadline.
2. Seek clarification from senior or interviewer.
	- Is this the only requirement.
	- What would happen with this changes.
	- Are we okey to tak 5 min downtime
3. Ask critical question that challenge the design decisions :
	- Like why protocol A not protocol B
	- Why we are using REST, why not gRPC
	- I think we should not use web sockets,
	- why this thing cannot be asynchronous.


## Step by step process : 


### 1. understand the problem  :

- summarize the problem statement in 1 paragraph.


### 2. Collect requirements :

[[2-requirements]]


### 3. High Level Design


### 4. Design APIs and Data Models

