

PostgreSQL is an open-source database management system.
- **ACID transactions**: Ensures data integrity (Atomicity, Consistency, Isolation, Durability).
- **Extensibility**: Allows custom functions, data types, and extensions (e.g., PostGIS for geospatial data).
- **Concurrency**: Handles multiple users via Multiversion Concurrency Control (MVCC).
- **Data types**: Supports JSON/JSONB for semi-structured data, arrays, and more.
- **Scalability**: Suitable for small to large-scale applications.


## Why Postgres is used in the first place ?


1. Native JSONB support and allows to put indexes in json data
	Postgres stores JSON in a parsed, binary format (`JSONB`). It allows to index the internal keys of that JSON document with incredible speed.

2. Support complex datatypes like array, UUID with native uuidv7 support
3. Concurrency and Write Performance : MVCC
	1. PostgreSQL uses a highly advanced Multiversion Concurrency Control (MVCC) system. When a row is updated, Postgres writes a new physical version of that row to the table heap rather than overwriting the old one.
		- **The Advantage:** Read operations are never blocked by write operations, and vice versa. Under heavy, concurrent write loads—common in distributed, event-driven architectures—Postgres maintains exceptionally high throughput without locking deadlocks.


4. **Extensibility :**
	1. Postgres is highly extensible, and allow the database to modify to specific requirement without modifying the core source code.
		- **pgvector:** Turns Postgres into a vector database for AI embeddings and similarity search.
		- **TimescaleDB:** Transforms it into a high-performance time-series database for metrics and events.
		- **PostGIS:** Adds incredibly advanced geospatial routing and querying.

5. **Native Fit for Distributed Systems** : 
	1. LISTEN / NOTIFY : Postgres has a build in Publish/Subscribe mechanism. A database trigger like row create or update can brodcast a notification and a backend server can listen for that event and perform async job.
	2. Logical Replication : It can stream row-level changes out to message brokers or other data warehouses, making it a foundational piece for eventually consistent systems.


## Problems with Postgres : 

1. The Connection Bottleneck : Process Per Connection
	Postgres spawns a completely new OS process for every single connection. Each process consumes 5mb to 10mb which is alot.
	In highly concurrent environment, postgres will consume the entire RAM, and crash due to Out of Memory.

2. 