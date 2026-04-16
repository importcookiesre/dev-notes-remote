

# Phase 0: Mental Model Setup

Goal: Install the Laws of Distributed Reality

---

## 1. What Is a Distributed System

Understand:

- Multiple independent nodes
    
- Communicate over network
    
- No shared memory
    
- Partial failure is normal
    

Key shift:  
You are programming a system where components can disagree about reality.

### From DDIA:

Read:

- Chapter 1 introduction
    
- Chapter 2 overview sections
    

Focus on:  
Reliability, scalability, maintainability.

---

## 2. Reliability Foundations

Before distribution, understand what reliability means.

Learn:

- Fault vs failure
    
- Availability
    
- Durability
    
- SLA vs SLO
    
- Mean time between failures
    
- Mean time to recovery
    

Why this matters:  
Distributed systems are about surviving failures, not preventing them.

### From DDIA:

Chapter 1 fully. Do not skim.

---

## 3. Latency and Throughput

You must deeply understand:

- Latency vs throughput
- Tail latency (p95, p99)
- Queueing effects
- Coordinated omission problem (concept level)
- Little’s Law (intuition only)

Mental model:  
As load increases, latency grows non linearly.

External resource:  
Search for:  
“Jeff Dean latency numbers” and understand the magnitude difference between memory, SSD, network.

---

## 4. The Fallacies of Distributed Computing

Internalize these as constraints:

- Network is unreliable
    
- Latency is not zero
    
- Bandwidth is limited
    
- Messages can duplicate
    
- Nodes can restart anytime
    

This is not theory. This is default state.

External resource:  
Search for “Fallacies of distributed computing original list”.

---

## 5. Failure Models

You must understand types of failure:

- Crash failure
    
- Omission failure
    
- Byzantine failure (conceptual only)
    
- Network partition
    
- Slow node failure
    

Mental shift:  
Slow is often worse than dead.

From DDIA:  
Read:  
Chapter 8 introduction sections on distributed systems problems.

---

## 6. Delivery Semantics

Understand clearly:

- At most once
    
- At least once
    
- Exactly once (practically achieved via idempotency)
    

You must be able to reason about retries.

Questions you should answer:  
If client times out but server processed request, what happens?

From DDIA:  
Chapter 11 sections on stream processing semantics.

---

## 7. Idempotency & Determinism

You must understand:

- Idempotent operations
    
- Commutative operations
    
- Deterministic state transitions
    
- Why retries require idempotency keys
    

This will appear in every system you build.

External resource:  
Look up:  
“Idempotent REST API design”.

---

## 8. Consistency Models

Now we move to core distributed theory.

Understand these properly:

- Strong consistency
- Eventual consistency
- Read after write consistency
- Monotonic reads
- Causal consistency
- Linearizability (intuition only)

Do not memorize definitions.  
Understand behavior differences.

From DDIA:  
Chapter 9 entirely.  
This is critical.

Take notes.

---

## 9. CAP Theorem (Practical View)

Understand:

- What partition tolerance means
- Why it is non negotiable
- What you sacrifice under partition
- Why CAP is about tradeoffs during partition only

Avoid oversimplified internet explanations.

From DDIA:  
Chapter 9 sections discussing CAP.

---

## 10. Replication

Before consensus, understand replication.

Learn:

- Leader follower replication
    
- Multi leader replication
    
- Log based replication
    
- Quorum reads and writes
    
- Write ahead log
    
- Replication lag
    

From DDIA:  
Chapter 5 entirely.  
Read slowly.

This chapter is foundational for your future builds.

---

## 11. Partitioning (Sharding)

Learn:

- Horizontal partitioning
    
- Range partitioning
    
- Hash partitioning
    
- Rebalancing
    
- Hot partitions
    

From DDIA:  
Chapter 6 fully.

This connects to scaling later.

---

## 12. Time and Ordering

Critical topic.

Understand:

- Clock skew
    
- NTP limitations
    
- Logical clocks
    
- Lamport clocks
    
- Vector clocks (conceptual)
    
- Why ordering is hard
    

Never assume wall clock time gives ordering.

From DDIA:  
Chapter 8 sections on time and clocks.

---

## 13. Consensus Fundamentals

You do not implement yet.  
Just understand problem.

Learn:

- What is consensus
    
- Leader election
    
- Quorum
    
- Safety vs liveness
    
- Split brain
    
- Why two phase commit is fragile
    

From DDIA:  
Chapter 9 and 10 sections on consensus.  
Especially Raft explanation.

---

## 14. Transactions in Distributed Systems

Understand:

- Two phase commit
    
- Distributed transactions
    
- Why they block
    
- Sagas
    
- Compensating transactions
    

From DDIA:  
Chapter 7 entirely.

This will matter when you build event driven systems later.

---

## 15. Backpressure & Flow Control

Very important, often ignored.

Understand:

- Producer faster than consumer problem
    
- Bounded queues
    
- Load shedding
    
- Rate limiting
    
- Circuit breakers
    

External resource:  
Search for:  
“Reactive manifesto backpressure explanation”.

---

## 16. Observability Basics

Before building systems, know how you will debug them.

Understand:

- Logs vs metrics vs traces
    
- RED method
    
- Golden signals
    
- Correlation IDs
    

External resource:  
Search:  
“Google SRE golden signals”.

You do not need full SRE book now.  
Just fundamentals.