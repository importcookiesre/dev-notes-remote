
In monolithic, the system is either running or broken, 
but in case of distributed system there could be many reasons for failure.

To build resilient systems, you have to understand exactly _how_ a component can fail so you can build the right defense mechanism.

### Why Slow is often worse than Dead.

A fast failure is a good failure. A slow response is a system killer.

If a Docker container running your API runs out of memory and instantly crashes (a clean death), your load balancer immediately detects the closed TCP socket and routes the next user's request to a healthy container. The user experiences a quick error or a seamless retry.

But, if that container gets stuck in a deadlock or is waiting on a slow PostgreSQL query, it doesn't die. It stays "up." It accepts incoming HTTP requests but takes 45 seconds to process them.

- Upstream services wait for it, tying up their own threads.
    
- Connection pools to the database get fully exhausted because the slow node is hoarding them.
    
- A single slow node creates a **Cascading Failure** that acts like a traffic jam, backing up the entire network until every other healthy service in your cluster starves to death waiting for a response.
    

This is why we aggressively use **timeouts** and **circuit breakers**. If a service can't answer in 200ms, it is better to brutally sever the connection and treat the node as dead than to wait for it.


### 1. crash failure : fail stop

- **What it is:** A node simply halts operations permanently or until restarted. It stops sending and receiving messages.
    
- **The Reality:** A Go process hits a `panic` that wasn't recovered, or the Linux Out-Of-Memory (OOM) killer terminates the container.
    
- **Why it's "easy":** Other nodes quickly realize it's gone because TCP connections drop or heartbeat pings stop answering. You just spin up a new instance.


### 2. Omission Failure

- **What it is:** A node is running, but it fails to send or receive messages.
    
- **The Reality:** The server is perfectly healthy, but the network switch's buffer is full, so it silently drops incoming packets. Or, an internal Go channel is full and dropping messages before they can be processed.
    
- **The Defense:** Acknowledgment protocols (ACKs). If you don't get an ACK, you assume an omission failure and retry.
    

### 3. Timing Failure (Slow Node / Performance Failure)

- **What it is:** A node responds with the correct data, but outside the required time window.
    
- **The Reality:** A massive Garbage Collection (GC) pause in the runtime freezes the application for 2 seconds. Or, a missing index in Postgres turns a 5ms lookup into a 5000ms full-table scan.
    
- **The Danger:** As mentioned above, this causes cascading failures and violates the "Slow is worse than dead" rule.
    

### 4. Network Partition

- **What it is:** The network splits into two or more isolated sub-networks. Nodes within a sub-network can talk to each other, but they cannot communicate across the partition.
    
- **The Reality:** A router misconfiguration severs communication between your `us-east` and `us-west` data centers. Both sides are perfectly healthy, but they think the other side is dead.
    
- **The Danger:** This triggers the "Split-Brain" scenario. If both sides of the partition elect a new primary database and start accepting writes, you will have massive data corruption when the network heals.
    

### 5. Crash-Recovery Failure (The Missing Link)

- **What it is:** A node crashes, but then restarts and tries to rejoin the cluster—except it has amnesia or corrupted state.
    
- **The Reality:** A server crashes mid-write. When Docker restarts it, its local disk contains a half-written, corrupted file, but the node broadcast to the cluster that it is healthy and ready to serve data.
    
- **The Defense:** Write-Ahead Logs (WAL), which Postgres uses heavily, ensuring that if a crash happens, the system can replay the log upon reboot to restore exact state before accepting traffic.
    

### 6. Byzantine Failure (Conceptual)

- **What it is:** The absolute worst-case scenario. A node doesn't just crash or slow down; it acts arbitrarily, maliciously, or sends conflicting information to different parts of the system.
    
- **The Reality:** A cosmic ray flips a bit in a server's RAM, causing it to calculate math incorrectly but report the answers as valid. Or, a hacker compromises a node and intentionally sends fake transaction data to the rest of the cluster.
    
- **The Defense:** Extremely complex, math-heavy consensus algorithms (like Practical Byzantine Fault Tolerance). In standard enterprise backend engineering, we generally ignore Byzantine failures. We assume our internal nodes might crash or be slow, but we trust that they aren't actively trying to sabotage us. Byzantine fault tolerance is mostly the domain of aerospace engineering (where radiation causes bit-flips) and blockchains (where you can't trust the other nodes).