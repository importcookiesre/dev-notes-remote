
# Priority Inversion

What it is

- Priority inversion occurs when a high-priority task is blocked waiting for a resource held by a low-priority task, while medium-priority tasks run and prevent the low-priority task from making progress, indirectly blocking the high-priority task.
    

Why it matters

- Causes unexpected latency for time-critical tasks. In systems with real thread priorities it is a classic correctness problem (Mars Pathfinder example).
Does it happen in Go?

- Go does not expose thread priorities to user code.


# Load Sheddings :

Core strategies

- Hard reject (fast fail)
    
    - Return 429 or 503 immediately when queues or in-flight work exceed thresholds. Simple and predictable.
        
- Queue-threshold reject
    
    - Keep a bounded input queue. If full, reject new work. Use TrySubmit semantics. Good for backpressure.
        
- Token-bucket / rate limiting
    
    - Limit request rate with a token bucket. Excess requests are rejected or delayed.
        
- Probabilistic early drop (random early discard)
    
    - At high load, drop a percentage of requests (randomized) to reduce spikes and avoid synchronized retries.
        
- Priority-based shedding
    
    - Reserve capacity for high-priority traffic and shed low-priority traffic first (separate queues or worker pools).
        
- Circuit-breaker / fail-fast
    
    - If downstream is failing, stop forwarding requests and fail fast, optionally route to cache or degraded mode.
        
- Degrade feature set (graceful degradation)
    
    - Serve cheaper, lower quality responses (cached, fewer fields) under load.
        
- Spill to durable store
    
    - If immediate processing is impossible, persist events to disk/queue for later ingestion (trade latency for reliability).
        
- Adaptive / learned shedding
    
    - Observe latency/error metrics and adjust shedding thresholds or drop probabilities automatically.
        

When to use which

- If correctness relies on processing every request, avoid hard shedding; prefer durable queues.
    
- For user-facing APIs with SLAs, keep reserved capacity or return explicit errors so clients can retry/back off.
    
- For telemetry/logging, prefer dropping when overloaded.