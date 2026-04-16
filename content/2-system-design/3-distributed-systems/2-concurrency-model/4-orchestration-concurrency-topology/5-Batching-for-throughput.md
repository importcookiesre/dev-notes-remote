
Batching means **collecting multiple items and processing them together as a group instead of individually.**

Instead of :
```go
process(item1)
process(item2)
process(item3)
process(item4)
```

You do:
```go
process([item1, item2, item3, item4])
```


## Throughput vs Latency Tradeoff

- Throughput: increases (fewer overheads per item)
- Latency: increases (items wait until a batch is filled or timer fires)
- Memory: increases (you hold more items before processing)


## use cases : 

#### 1. Batching is used when:
- operations are expensive
- many small tasks exist
- latency tolerance exists

#### 2. Common examples:
- database writes
- log ingestion
- analytics pipelines
- Kafka producers
- network packet sending
- background job systems
- email sending systems
- event processing

#### 3. Kafka Producer
Kafka batches messages before sending to brokers to reduce network overhead.

#### 4. Logging Systems
Many logging systems batch logs before writing to disk.

#### 5. Email Systems

Instead of sending emails one-by-one:
```go
send_email(user1)  
send_email(user2)
```
Systems batch them.


---
## Batching strategies : 

 1. Size based batching :
	Process when batch reaches size **N**.
	maximizes throughtput
	If traffic is low then item may wait too long.

2. Time based batching :
	1. Flush batch every T second.
	2. Predictable latency
	3. smaller batches → less throughput gain

3. Hybrid Batching : 

flush when
```go
batch_size >= N
OR
time >= T
```
This gives:
- good throughput
- bounded latency
This is how systems like **Kafka producers and log aggregators work.**


## Failure Handling :

- Retry whole-batch: simple but duplicates may occur; require idempotency.
- Per-item retry: unpack failed items and retry individually or in smaller batches.
- Dead-letter queue (DLQ): write permanently failing items to DLQ for offline inspection.
- Backoff + jitter: exponential backoff with jitter for retries.
- Circuit-breaker: stop batching to the sink if it fails repeatedly; spill to disk/queue.



## Ordering and idempotency

- If ordering matters, attach sequence numbers and either (a) re-sequence at the consumer or (b) only use batching when order is not required.
    
- Make batch operations idempotent where possible (e.g., upsert semantics, idempotency key).

## Backpressure and resource protection

- Use bounded input channel or bounded buffer to avoid unbounded memory growth.
    
- If buffer fills, either block producers (backpressure), return an error (fail fast), or drop items (best-effort telemetry).
    
- Monitor queue lengths, batch size distribution, and processing latency. Set alerts on high queue depth or slow batch processing.