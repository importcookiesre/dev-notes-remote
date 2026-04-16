




## latency :

Latency is the time taken for a single request to travel from the client to the server, get processed, and return a response. It is measured in the unit of time.

## Throughput : 

Throughput measures the amount of work a system can handle over a given period of time.
It represents the system's processing capacity and typically measured in request per second (RPC), transaction per second (TPS) or data per second (Mbps).


## Tail latency : 

Tail latency refers to the small percentage of requests that take significantly longer to process than the average.

- **Average Latency** is the mean response time for all requests.

- **P95 (95th Percentile):** The value under which 95% of your requests finish. It means 95% of users experience this speed or better, while the slowest 5% of requests take longer than this.
- **P99 (99th Percentile):** The value under which 99% of your requests finish. It means 99% of users experience this speed or better, while the slowest 1% of requests take longer.

Tail Latency Metric Comparison

| Percentile | Name              | Description                                                              | Use Case                                       |
| ---------- | ----------------- | ------------------------------------------------------------------------ | ---------------------------------------------- |
| **P50**    | Median            | Median latency; 50% of requests are faster, 50% are slower.              | General performance baseline.                  |
| **P95**    | 95th Percentile   | The top 5% slowest requests. Represents common "bad" experience.         | Good starting point for SLOs.                  |
| **P99**    | 99th Percentile   | The top 1% slowest requests. Represents significant, recurring outliers. | Standard metric for user experience.           |
| **P99.9**  | 99.9th Percentile | The top 0.1% slowest requests. Represents extreme outliers.              | Critical for high-volume, low-latency systems. |
| **Max**    | Maximum           | The single slowest request.                                              | Debugging, but often misleadingly volatile.    |

### How are they calculated ?



### significance : 

- Averages hide bottlenecks. If 99 requests take 10ms and one takes 10 seconds, the average (109ms) looks okay, but one user had a terrible experience. P99 shows that 10-second spike.

- **Real User Experience:** P95/P99 tell you how the system _feels_ to real users. They highlight when the system becomes sluggish for a minority of users.
- **Identifying "Broken Tail" Problems:** If P95 is good but P99 is very high, it suggests your system is fragile—a few, but significant, errors like JVM garbage collection, slow DB queries, or network retries are impacting users.
- **SLA and SLOs:** High-level systems (like payments or booking systems) use P99 or higher (P99.9) to guarantee service quality to customers.

## latency numbers : 

| Operation                             | Approx Latency | Compared To              |
| ------------------------------------- | -------------- | ------------------------ |
| L1 cache reference                    | ~0.5 ns        | Basically instant        |
| L2 cache reference                    | ~7 ns          | Still extremely fast     |
| L3 cache reference                    | ~30 ns         | Slightly slower          |
| Main memory (RAM) access              | ~100 ns        | 200x slower than L1      |
| Mutex lock/unlock                     | ~100 ns        | Same order as RAM        |
| SSD read                              | ~100 µs        | 1,000x slower than RAM   |
| Network round trip (same data center) | ~0.5 ms        | 5x slower than SSD       |
| Network round trip (cross region)     | ~50–150 ms     | 100x slower than same DC |
| Disk seek (HDD)                       | ~5–10 ms       | 10x slower than SSD      |
| Intercontinental network              | ~150–300 ms    | Human noticeable delay   |
