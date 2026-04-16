
read :
[[1-unbuffered-channel]]
[[2-buffered-channel]]



## Bounded Queue : 

- Has a fixed capacity C. Once it holds C items, behavior is determined by the policy you choose: block producer, return an error, or drop items.
- Typical implementation: a ring buffer or a buffered channel.


## Unbounded Queue : 

- No fixed capacity; it grows as producers push items. Producers usually never block. Consumers block only when the queue is empty.
    
- Typical implementation: dynamically growing slice/linked list protected by a mutex and a condition variable, or a goroutine-based buffer.



## Core behavioral differences

- Blocking vs non-blocking on push:
    - Bounded with blocking: producers wait until space becomes available.
    - Bounded with non-blocking offer: producers get a failure/boolean if full.
    - Unbounded: push usually never blocks (until memory exhaustion).
        
- Memory usage:
    - Bounded: predictable memory usage (C * item_size).
    - Unbounded: can grow unbounded and risk out-of-memory.
        
- Backpressure:
    - Bounded gives natural backpressure; system slows producers.
    - Unbounded removes immediate backpressure; build-up can overwhelm consumers later.
        
- Latency:
    - Bounded with congestion creates higher producer latency (waiting). It keeps tail latency bounded by queue capacity.
    - Unbounded can keep latency low for producers until consumers fall behind, then tail latency and GC pauses can explode.
        
- Ordering:
    - Both typically preserve FIFO ordering unless you design otherwise.



## When to use which

Use **bounded** when:
- You want predictable resource usage and simple backpressure.
- You need to enforce flow control and protect downstream services.
- You want to limit latency and tail latency.

Use **unbounded** when:
- Producers must not block (e.g., capture logs, telemetry) and you can tolerate or handle eventual drops/aggregation.
- You will implement external safeguards: circuit breakers, rate limiting, offload to durable storage, or drain to disk.


# Common policies to handle overload

- Block producers until capacity frees up.
    
- Return error / fail fast to the producer so it can retry or back off.
    
- Drop newest (drop-tail) or drop oldest items.
    
- Sample or aggregate items instead of storing them all.
    
- Spill to disk / persistent queue when memory high.
    
- Apply rate limiters or token buckets upstream.
    
- Use high-water/low-water marks and throttling callbacks.
    

# Orchestration and topology patterns

- Pipeline stage: bounded queue between stages to isolate latency and resource usage.
    
- Worker pool: bounded queue of tasks consumed by N workers.
    
- Event streaming: typically unbounded in concept, but consumers have offsets and retention policies; often backed by disk (Kafka).
    
- Actor mailbox: often bounded to protect the actor.