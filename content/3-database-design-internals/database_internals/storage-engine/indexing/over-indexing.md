---
title: Problems associated with Over-Indexing in Postgres.
---

Over-indexing is a database anti-pattern where a table contains more indexes than are necessary to optimize read operation.

Over indexing degrates performance in multiple layers : 

## 1. write becomes expensive : 

In PostgreSQL, an index is a distinct physical data structure separate from the main table (the heap). When an `INSERT` or `DELETE` operation occurs, PostgreSQL must write the new tuple to the heap and subsequently update every single index associated with that table.

If a table has ten indexes, a single `INSERT` translates to one heap write and ten separate index tree modifications. This results in massive write amplification, saturating disk I/O, and drastically increasing the volume of data written to the Write-Ahead Log (WAL).

1. For every `INSERT`, Postgres must insert entries into every index on that table.  
2. For every `UPDATE`, it may need to update one or more indexes too, especially if indexed columns changed. Postgres uses `Heap Only Tuple (HOT)` optimization to overcome the overhead. 
3. For every `DELETE`, index entries must eventually be removed or marked dead.


## 2. Inhibition of Heap Only Typles (HOT) Updates : 

PostgreSQL uses Multi-Version Concurrency Control (MVCC). When a row is `UPDATE`d, PostgreSQL does not modify the data in place; it writes a entirely new version of the row (a new tuple) and marks the old one as dead. Normally, this means every index must be updated to point to the new physical location of the tuple.

To mitigate this, PostgreSQL utilizes an optimization called **Heap-Only Tuples (HOT)**. If an `UPDATE` operation does not modify any columns that are part of an index, and there is sufficient free space on the same data page, PostgreSQL skips updating the indexes. The existing index pointer directs to the old tuple, which internally forwards to the new tuple.

Over-indexing severely diminishes the probability of HOT updates. If you place indexes on frequently updated fields (e.g., a `status` or `last_modified` column), every update forces a rewrite of _all_ index entries for that row, completely bypassing the HOT optimization and causing severe database bloat.


## 3. buffer cache contention :

PostgreSQL relies on memory (`shared_buffers`) to cache frequently accessed data. Indexes consume physical memory. An excessive number of indexes means index pages compete with heap data pages for cache space. When the cache fills up, PostgreSQL must evict pages. If large, unused indexes occupy `shared_buffers`, critical table data or heavily utilized indexes are pushed out to disk, resulting in higher cache miss rates and latency spikes across all queries.


## 4. Query planner overhead :

Before executing a query, the PostgreSQL query planner evaluates possible execution paths to find the lowest-cost plan. The planner must evaluate every relevant index. A heavily indexed table exponentially increases the search space for the query optimizer, increasing CPU utilization and the time it takes simply to plan the query before execution even begins.



