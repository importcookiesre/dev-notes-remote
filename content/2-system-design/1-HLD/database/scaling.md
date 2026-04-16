

## Sharding and Database replication : 

### 1. Database Replication (Copying)

Replication involves creating **exact copies** of your database on multiple servers.
- **How it works:** You have one "Primary" (Master) node and multiple "Replica" (Slave) nodes. All writes go to the Primary, and data is copied (synced) to the Replicas.
- **Primary Goal:**
    - **High Availability (Safety):** If the Primary crashes, a Replica can take over immediately.
    - **Read Scaling:** You can distribute read queries ( `SELECT` ) across multiple replicas to handle more traffic.
- **Trade-off:** It does **not** help with write capacity (since all writes still go to one node) or storage limits (every node stores 100% of the data).
### 2. Database Sharding (Splitting)

Sharding involves **splitting** your data across multiple servers. It is strictly "Horizontal Partitioning."
- **How it works:** Instead of one big database, you split the data into smaller chunks called "shards." For example, Users A-M go to Server 1, and Users N-Z go to Server 2.
- **Primary Goal:**
    - **Write Scaling:** You can process parallel writes because different servers handle different users.
    - **Storage Capacity:** You can store more data than fits on a single hard drive.
- **Trade-off:** It is significantly more complex. You cannot easily perform `JOINs` across shards, and if you pick a bad "Shard Key" (the logic used to split data), one server might get overloaded while others sit idle.