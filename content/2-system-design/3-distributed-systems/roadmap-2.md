
# FOUNDATION LAYER

## Phase 1: Deterministic Systems & Concurrency (Weeks 1–2)

Before distribution, master controlled chaos inside a single machine.

### Concepts You Must Internalize

- Processes vs threads
    
- Race conditions
    
- Deadlocks
    
- Backpressure
    
- Bounded queues
    
- Memory visibility
    
- Context cancellation
    
- Graceful shutdown
    

### Build 1: Production Grade In Memory Task Queue

Features:

- Worker pool
    
- Configurable concurrency
    
- Retry with exponential backoff
    
- Max retry count
    
- Dead letter queue
    
- Priority queue
    
- Graceful shutdown
    
- Metrics endpoint
    
- Context aware jobs
    
- Backpressure when queue full
    

Add:

- Benchmarks
    
- Unit tests
    
- Load test
    

Simulate:

- Worker panic
    
- Slow consumers
    
- Queue overflow
    

Goal:  
Understand throughput, fairness, starvation.

If this phase feels easy, you’re not pushing hard enough.

---

# NETWORK LAYER

## Phase 2: Distributed Communication Basics (Weeks 3–4)

Now we introduce unreliable networks.

### Concepts

- RPC vs REST
    
- Serialization cost
    
- Timeouts
    
- Retries
    
- Idempotency
    
- Circuit breakers
    
- Connection pooling
    

### Build 2: Dual Service Architecture

Service A:

- Accepts tasks
    
- Stores in DB
    
- Publishes event
    

Service B:

- Polls
    
- Processes
    
- Updates state
    

Use:

- Redis or Postgres queue
    

Features:

- Idempotency key
    
- Retry policy
    
- Visibility timeout
    
- Dead letter queue
    
- Request tracing
    

Simulate:

- Network delay
    
- Service crash mid request
    
- Duplicate message
    
- DB restart
    

Goal:  
Understand why exactly once delivery does not exist in real life.

---

# DATA LAYER

## Phase 3: Replication & Consistency (Weeks 5–7)

Now we step into serious territory.

### Concepts

- CAP theorem
    
- Strong vs eventual consistency
    
- Quorum reads and writes
    
- Leader follower replication
    
- Log based replication
    
- Write ahead log
    
- Snapshotting
    

### Build 3: Replicated Key Value Store

3 nodes.

Features:

- Leader node
    
- Followers
    
- Write replication
    
- Quorum acknowledgement
    
- Basic persistence
    
- Heartbeats
    
- Failover logic
    

Simulate:

- Leader crash
    
- Follower lag
    
- Network partition
    
- Split brain
    

Goal:  
Feel the cost of consistency.

This phase changes your mental model permanently.

---

# CONSENSUS LAYER

## Phase 4: Consensus Algorithm (Weeks 8–10)

Now we implement simplified Raft.

### Concepts

- Term
    
- Election timeout
    
- Heartbeat
    
- Log matching property
    
- Commit index
    
- Safety vs liveness
    

### Build 4: Minimal Raft Implementation

Start with:

- In memory only
    

Then:

- Persistent logs
    
- Crash recovery
    

Then:

- Add client write API
    

Simulate:

- Node restart
    
- Leader loss
    
- Partition majority vs minority
    

Goal:  
Understand why consensus is expensive and subtle.

Most engineers never reach here.

---

# EVENT DRIVEN SYSTEMS

## Phase 5: Streaming & Messaging (Weeks 11–13)

Now shift to architecture patterns.

### Concepts

- Event sourcing
    
- Outbox pattern
    
- Idempotent consumers
    
- Consumer groups
    
- Offset management
    
- Replay
    
- Ordering guarantees
    
- At least once delivery
    

### Build 5: Event Driven Order System

Services:

- Order
    
- Payment
    
- Inventory
    
- Notification
    

Use:

- Kafka or NATS
    

Features:

- Event publishing
    
- Retry + DLQ
    
- Outbox pattern
    
- Saga pattern
    
- Event replay
    

Simulate:

- Duplicate events
    
- Out of order events
    
- Service crash during saga
    

Goal:  
Understand distributed transactions are painful.

---

# RESILIENCE LAYER

## Phase 6: Fault Tolerance Patterns (Weeks 14–15)

### Concepts

- Circuit breaker
    
- Bulkhead
    
- Rate limiting
    
- Exponential backoff
    
- Hedged requests
    
- Leader election via lease
    
- Gossip protocol basics
    

### Build 6: Resilient API Gateway

Add:

- Circuit breaker middleware
    
- Distributed rate limiter
    
- Health checks
    
- Service discovery simulation
    
- Graceful degradation
    

Simulate:

- Slow downstream
    
- Sudden traffic spike
    
- Partial outage
    

Goal:  
Build systems that survive.

---

# OPERATIONS LAYER

## Phase 7: Observability & Chaos (Weeks 16–17)

### Concepts

- Metrics vs logs vs traces
    
- RED method
    
- Golden signals
    
- Structured logging
    
- Distributed tracing
    
- Chaos engineering
    

### Add To All Projects

- Prometheus metrics
    
- Grafana dashboard
    
- Request tracing
    
- Load testing
    
- Latency injection
    
- Kill node randomly
    

Goal:  
Learn to debug production level issues.


# ADVANCED LAYER

## Phase 8: Real World Systems Deep Dive (Ongoing)

Study internals of:

## Apache Kafka

![https://daxg39y63pxwu.cloudfront.net/images/blog/apache-kafka-architecture-/image_589142173211625734253276.png](https://daxg39y63pxwu.cloudfront.net/images/blog/apache-kafka-architecture-/image_589142173211625734253276.png)

![https://docs.cloudera.com/runtime/7.3.1/kafka-overview/images/kafka-partition-log-segments.png](https://docs.cloudera.com/runtime/7.3.1/kafka-overview/images/kafka-partition-log-segments.png)

![https://images.ctfassets.net/gt6dp23g0g38/4YKWvDvHoX9Y4NzdPQZvo5/f1f3811e4e6098a45ba702fed62b6735/Kafka_Internals_064.png](https://images.ctfassets.net/gt6dp23g0g38/4YKWvDvHoX9Y4NzdPQZvo5/f1f3811e4e6098a45ba702fed62b6735/Kafka_Internals_064.png)

4

Understand:

- Partitioning
    
- Log segments
    
- ISR
    
- Controller node
    

---

## etcd

![https://etcd.io/img/client-architecture-balancer-figure-08.png](https://etcd.io/img/client-architecture-balancer-figure-08.png)

![https://media.licdn.com/dms/image/v2/D5612AQENSE5lt0Zhyg/article-cover_image-shrink_720_1280/B56Zp7rfGjIcAI-/0/1763011591335?e=2147483647&t=skSZE_gzP7mKQfwfZPrJ_kUFhQPMA-oSe3L7YV90XvY&v=beta](https://media.licdn.com/dms/image/v2/D5612AQENSE5lt0Zhyg/article-cover_image-shrink_720_1280/B56Zp7rfGjIcAI-/0/1763011591335?e=2147483647&t=skSZE_gzP7mKQfwfZPrJ_kUFhQPMA-oSe3L7YV90XvY&v=beta)

![https://miro.medium.com/0%2AztviuIaVJy99UCQc.jpg](https://miro.medium.com/0%2AztviuIaVJy99UCQc.jpg)

4

Understand:

- Raft implementation
    
- Watch mechanism
    
- Leases
    

---

## Redis

![https://cdn.sanity.io/images/sy1jschh/production/085a5659f88e6f4fd65b6d58907eeaff12ed3170-1280x720.jpg?auto=format&fit=clip&q=80&w=3840](https://cdn.sanity.io/images/sy1jschh/production/085a5659f88e6f4fd65b6d58907eeaff12ed3170-1280x720.jpg?auto=format&fit=clip&q=80&w=3840)

![https://images.viblo.asia/c7ece062-b701-4b14-a65e-d1e7d5447dd4.png](https://images.viblo.asia/c7ece062-b701-4b14-a65e-d1e7d5447dd4.png)

![https://substackcdn.com/image/fetch/%24s_%21lW9a%21%2Cf_auto%2Cq_auto%3Agood%2Cfl_progressive%3Asteep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F0542714d-99f9-4642-b58e-98ea65f5be7a_2728x1028.png](https://substackcdn.com/image/fetch/%24s_%21lW9a%21%2Cf_auto%2Cq_auto%3Agood%2Cfl_progressive%3Asteep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F0542714d-99f9-4642-b58e-98ea65f5be7a_2728x1028.png)

4

Understand:

- Replication
    
- Persistence
    
- Sentinel
    
- Cluster hash slots
    

---

# Parallel Study Track

Read slowly:

Designing Data-Intensive Applications

Do not rush it.  
Map concepts to your builds.