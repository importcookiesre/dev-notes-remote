

## Data Intensive : 

Many application today are Data Intensive where the bigger challenge are amount of data, complexity of data, and the speed at which these data are changing.

Some of the building blocks used to these problems are : 

1. Database : store data
2. Cache : store and serve expensive operation to speed up reads.
3. Search Index : Searching keywords and filter
4. Stream Processing : handle asychronous data processing
5. Batch Processing : Periodically process large amount of accumulated data.

### 1. choosing a system vs choosing a right system : 

There are systems with different characteristics and specialization, and because different systems has different requirements, we need to choose the right system.

eg. 
- There are different types of databases, different strategy for caching, search indexing and we need to choose the right one based on our need.



## Reliability : 

The system should continue to work correctly (performing the correct function at the desired level of performance) even in the face of adversity (hardware or soft‐ ware faults, and even human error).

Everyone can have thier own defination of reliability :
For some people it can be a simple software that takes an input and returns a correct output, or a software that is performant and works smoothly at expected load and data volume.
If all of these means a "working correctly" then it is a reliable software.

### fault vs failure : 

A fault is a underlying bug within code, design or hardware. 

**Failure** is the observable, operational breakdown that occurs when that fault is executed, causing the system to stop providing its required service to the user.

It is impossible to reduce the probability of a fault to zero.
Therefore it is usually best to design fault-tolerance mechanisms that prevent faults from causing failures.

> Fault tolerance is the ability of a system to continue operating properly, without interruption, even when one or more component fails.
> It ensures high availability, prevent single point of failure and often provides zero downtime recovery via redundancy such as backup, standby to immediately takeover in case of component failure.


#### Mean time between failure vs Mean time To recovery 

 Mean time between failure : 
	 The average amount of time a system, component, or service runs continuously in production before experiencing a failure.
	 MTBF = total uptime / nos of failures
Mean time to recover : 
	The average time it takes to fully restore service after a failure has occurred. It includes time to detect, diagnose
	MTTR = total downtime / nos of failures.

## Scalability : 

Scalability is the ability of a system to handle increased load : such as more users, more data, higher traffic
