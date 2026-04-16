

#### What is NoSQL database ?

A **NoSQL database** is a type of database that stores and manages data in a more flexible way than traditional relational databases. Instead of organising data into rows and columns like in a spreadsheet, NoSQL databases can store data in different formats, such as:

- **Documents** (like JSON files)
- **Key-value pairs** (like a dictionary)
- **Graphs** (nodes and relationships)
- **Wide columns** (grouped data in tables)

They are great for handling large amounts of unstructured or semi-structured data, are highly callable, and are often used in real-time applications like social media, e-commerce, and big data analytics.


# Key Characteristics of NoSQL database
## 1. **Schema-Less or Flexible Schema**

- **What it means**: Unlike relational databases that require a fixed schema (a predefined structure for tables and columns), NoSQL databases allow you to store data without a strict structure.
- **Why it matters**:
    - Makes it easy to handle **unstructured or semi-structured data**, such as JSON documents, logs, or IoT sensor data.
    - Allows you to quickly adjust your data model if the application requirements change (e.g., adding new fields to a document without impacting existing data).
- **Example**: In MongoDB, you can store documents with different fields in the same collection, making it highly adaptable.

---

##### 2. **Horizontal Scalability**

- **What it means**: NoSQL databases are designed to scale out by adding more servers (or nodes) instead of upgrading existing hardware (scaling up).
- **Why it matters**:
    - Distributes the data across multiple servers, enabling the system to handle large workloads and maintain performance as the data volume grows.
    - Reduces costs, as scaling horizontally can be done with commodity hardware.
- **Example**: In Cassandra, the data is distributed across a cluster of servers, and new nodes can be added seamlessly without downtime.

---

##### 3. **High Performance**

- **What it means**: NoSQL databases are optimised for fast data access and high throughput.
- **Why it matters**:
    - By simplifying the storage structure and reducing overhead (like complex joins), NoSQL databases are able to handle large amounts of data at high speeds.
    - Perfect for real-time applications like gaming leaderboards or stock price tracking.
- **Example**: Redis, a key-value NoSQL database, is known for its blazing-fast response times, making it ideal for caching.

---

##### 4. **Distributed and Fault-Tolerant Architecture**

- **What it means**: NoSQL databases often operate on a distributed model where data is replicated across multiple servers or locations.
- **Why it matters**:
    - **High availability**: Even if one server fails, other servers can take over, ensuring the system keeps running.
    - **Data locality**: Users from different regions can access data from a nearby server, reducing latency.
    - **Scalability and redundancy**: Data replication ensures that no single point of failure disrupts the application.
- **Example**: Couchbase uses a distributed architecture with automatic data replication to ensure fault tolerance.

---

##### 5. **Support for Diverse Data Models**

NoSQL databases support various data storage models, tailored for specific types of applications:

- **Document Model**:
    - Data is stored as documents, typically in formats like JSON or BSON.
    - **Use Case**: E-commerce product catalogs, where each product has different fields.
    - **Example**: MongoDB, CouchDB.
- **Key-Value Model**:
    - Data is stored as simple key-value pairs.
    - **Use Case**: Caching, session management.
    - **Example**: Redis, DynamoDB.
- **Column-Family Model**:
    - Data is stored in tables with rows and columns, but columns are grouped into families, providing more flexibility.
    - **Use Case**: Analytics on time-series data.
    - **Example**: Cassandra, HBase.
- **Graph Model**:
    - Data is stored as nodes and edges, representing entities and their relationships.
    - **Use Case**: Social networks, recommendation engines.
    - **Example**: Neo4j, ArangoDB.

---

##### 6. **Eventual Consistency**

- **What it means**: NoSQL databases prioritize high availability and partition tolerance over immediate consistency (as per the CAP theorem).
- **Why it matters**:
    - Allows the system to keep functioning even when parts of it are unavailable.
    - Data changes might take some time to propagate across the system but will eventually become consistent.
- **Example**: Amazon DynamoDB uses eventual consistency to ensure fast write operations while maintaining reliability.

---

##### 7. **Optimised for Large Data Volumes (Big Data)**

- **What it means**: NoSQL databases are designed to handle large-scale data and high-speed writes.
- **Why it matters**:
    - Suitable for modern applications generating vast amounts of data, such as IoT devices, social media platforms, or logs.
    - Efficiently handles scenarios where data grows exponentially.
- **Example**: HBase (based on Hadoop) is optimized for storing massive datasets in a distributed manner.

---

##### 8. **High Availability and Partition Tolerance**

- **What it means**: NoSQL databases are built to stay operational even if parts of the system are unavailable or experiencing issues.
- **Why it matters**:
    - Ensures uninterrupted service to users.
    - Especially important for applications requiring 24/7 uptime, like banking systems or global e-commerce platforms.
- **Example**: Couchbase uses built-in replication and auto-failover mechanisms to maintain availability.