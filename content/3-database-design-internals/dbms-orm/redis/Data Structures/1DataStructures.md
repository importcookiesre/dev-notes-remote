


### **Comparison Table**

| Data Structure | Key Features                         | Use Cases                                   |
| -------------- | ------------------------------------ | ------------------------------------------- |
| Strings        | Simple key-value storage             | Caching, counters, configuration values     |
| Lists          | Ordered collection                   | Queues, logs, feeds                         |
| Sets           | Unordered collection of unique items | Tags, unique users, set operations          |
| Sorted Sets    | Sets with scores for ordering        | Leaderboards, rankings, task prioritization |
| Hashes         | Field-value mapping                  | User profiles, metadata, structured data    |
| Bitmaps        | Bit-level operations on strings      | Boolean tracking, attendance                |
| HyperLogLog    | Probabilistic cardinality estimation | Unique visitors, large-scale tracking       |
| Streams        | Real-time data management            | Messaging, event logging, real-time feeds   |

---


#### Strings
- A simple key-value pair where the value is a string (can also store numbers, JSON, or binary data).
- Supports numeric manipulation.


| **Command**               | **Description**                                   | **Example**                  |
| ------------------------- | ------------------------------------------------- | ---------------------------- |
| `SET key value`           | Set a value for the given key.                    | `SET user:1 "Alice"`         |
| `GET key`                 | Get the value of the given key.                   | `GET user:1`                 |
| `INCR key`                | Increment the numeric value stored at the key.    | `INCR counter`               |
| `DECR key`                | Decrement the numeric value stored at the key.    | `DECR counter`               |
| `APPEND key value`        | Append value to the existing value.               | `APPEND user:1 "Smith"`      |
| `STRLEN key`              | Get the length of the value.                      | `STRLEN user:1`              |
| `SETEX key seconds value` | Set a value with an expiration time (in seconds). | `SETEX session:1 300 "data"` |

---


#### List:
- Ordered collections of strings, similar to a linked list. 
- Support push and pop operations from both ends.

|**Command**|**Description**|**Example**|
|---|---|---|
|`LPUSH key value`|Add a value to the head (left).|`LPUSH tasks "task1"`|
|`RPUSH key value`|Add a value to the tail (right).|`RPUSH tasks "task2"`|
|`LPOP key`|Remove and return the first element.|`LPOP tasks`|
|`RPOP key`|Remove and return the last element.|`RPOP tasks`|
|`LRANGE key start stop`|Get a range of elements (inclusive).|`LRANGE tasks 0 -1`|
|`LLEN key`|Get the length of the list.|`LLEN tasks`|
|`LINDEX key index`|Get the element at the specified index.|`LINDEX tasks 1`|



#### Sets:
Sets are unordered collections of unique strings. They are optimized for membership tests and set operations like union, intersection, and difference.

##### **Key Features**:

- Unordered and ensures uniqueness of elements.
- Supports fast set operations.
- Memory-efficient for storing large collections of unique items.


| **Command**           | **Description**                              | **Example**               |
| --------------------- | -------------------------------------------- | ------------------------- |
| `SADD key value`      | Add a value to the set. (or multiple values) | `SADD colors "red"`       |
| `SREM key value`      | Remove a value from the set.                 | `SREM colors "red"`       |
| `SMEMBERS key`        | Get all elements of the set.                 | `SMEMBERS colors`         |
| `SISMEMBER key value` | Check if a value exists in the set.          | `SISMEMBER colors "blue"` |
| `SCARD key`           | Get the number of elements in the set.       | `SCARD colors`            |
| `SINTER key1 key2`    | Get the intersection of two sets.            | `SINTER colors1 colors2`  |
| `SUNION key1 key2`    | Get the union of two sets.                   | `SUNION colors1 colors2`  |


---

#### Sorted sets
Sorted Sets are like sets but with an associated score for each member, which determines the order.

##### **Key Features**:

- Members are unique, but scores can repeat.
- Supports efficient range queries based on scores.
- Used for ranking systems and priority queues.
##### **Use Cases**:

- Tracking unique visitors or users.
- Tagging systems and group management.
- Performing set operations like intersections (e.g., common interests).

##### **Use Cases**:

- Implementing leaderboards.
- Ranking search results.
- Scheduling tasks by priority.

| **Command**                          | **Description**                              | **Example**                    |
| ------------------------------------ | -------------------------------------------- | ------------------------------ |
| `ZADD key score member`              | Add a member with a score to the sorted set. | `ZADD leaderboard 100 "Alice"` |
| `ZREM key member`                    | Remove a member from the sorted set.         | `ZREM leaderboard "Alice"`     |
| `ZRANGE key start stop [WITHSCORES]` | Get members within a range.                  | `ZRANGE leaderboard 0 -1`      |
| `ZRANK key member`                   | Get the rank of a member.                    | `ZRANK leaderboard "Alice"`    |
| `ZCOUNT key min max`                 | Count members with scores in a range.        | `ZCOUNT leaderboard 50 100`    |
| `ZSCORE key member`                  | Get the score of a member.                   | `ZSCORE leaderboard "Alice"`   |
| `ZREVRANGE key start stop`           | Get members in reverse order of their rank.  | `ZREVRANGE leaderboard 0 -1`   |


#### **5. Hashes**

Hashes are maps of field-value pairs, where each field is unique within the hash. They are optimised for storing objects and allow efficient access to individual fields.

##### **Key Features**:

- Lightweight storage for multiple key-value pairs.
- Fast access to individual fields.
#### **Use Cases**:

- Storing user profiles (e.g., `user:1 -> name, age, email`).
- Caching objects with multiple properties.
- Managing configurations with multiple parameters.

| **Command**                             | **Description**                        | **Example**                     |
| --------------------------------------- | -------------------------------------- | ------------------------------- |
| `HSET key field value`                  | Set a field to a value in the hash.    | `HSET user:1 name "Alice"`      |
| `HGET key field`                        | Get the value of a specific field.     | `HGET user:1 name`              |
| `HGETALL key`                           | Get all fields and values in the hash. | `HGETALL user:1`                |
| `HDEL key field`                        | Delete a field from the hash.          | `HDEL user:1 name`              |
| `HEXISTS key field`                     | Check if a field exists in the hash.   | `HEXISTS user:1 email`          |
| `HLEN key`                              | Get the number of fields in the hash.  | `HLEN user:1`                   |
| `HMSET key field1 value1 field2 value2` | Set multiple fields and values.        | `HMSET user:1 age 25 city "NY"` |


#### Bitmaps:
Bitmaps allow you to manipulate individual bits of a string value stored at a key. They are useful for binary operations and compact storage of large datasets.

##### **Key Features**:

- Store data as a sequence of bits.
- Support for bit-level operations.
- Extremely memory-efficient for large datasets.

##### **Use Cases**:

- Tracking daily user activity (e.g., which days a user was active).
- Representing presence/absence data compactly.
- Efficiently storing and analysing large boolean datasets.


|**Command**|**Description**|**Example**|
|---|---|---|
|`SETBIT key offset value`|Set the bit at the specified offset.|`SETBIT attendance 5 1`|
|`GETBIT key offset`|Get the bit at the specified offset.|`GETBIT attendance 5`|
|`BITCOUNT key`|Count the number of bits set to 1.|`BITCOUNT attendance`|
|`BITPOS key bit [start end]`|Find the position of the first bit set to the given value.|`BITPOS attendance 1`|
|`BITOP operation destkey key1 [key2 ...]`|Perform a bitwise operation.|`BITOP AND result attendance1 attendance2`|

### **7. HyperLogLog**

#### **Explanation**:

HyperLogLog is a probabilistic data structure used for estimating the cardinality (number of unique elements) of a dataset with minimal memory.

#### **Key Features**:

- Highly memory-efficient (uses only 12 KB).
- Provides approximate cardinality estimates.
- Ideal for large datasets where exact counts are unnecessary.

#### **Use Cases**:

- Counting unique visitors to a website.
- Tracking unique events or actions in a stream.
- Estimating distinct items in a massive dataset.

#### **Commands Table**:

|**Command**|**Description**|**Example**|
|---|---|---|
|`PFADD key element`|Add an element to the HyperLogLog.|`PFADD visits "user1"`|
|`PFCOUNT key`|Estimate the cardinality of the elements.|`PFCOUNT visits`|
|`PFMERGE destkey key1 key2`|Merge multiple HyperLogLogs into one.|`PFMERGE total visits1 visits2`|

#### **Example**:

redis

CopyEdit

`PFADD visits "user1" PFADD visits "user2" PFCOUNT visits`

---

### **8. Streams**

#### **Explanation**:

Streams are an append-only data structure used to store a sequence of events. Each entry in a stream has a unique ID and associated data fields.

#### **Key Features**:

- Ordered sequence of events.
- Allows range queries and consumer groups for processing.
- Supports persistence for reliable event processing.

#### **Use Cases**:

- Message queues for real-time data pipelines.
- Event sourcing for microservices.
- Logging and auditing systems.

#### **Commands Table**:

|**Command**|**Description**|**Example**|
|---|---|---|
|`XADD key ID field value`|Add an entry to the stream.|`XADD mystream * user Alice`|
|`XRANGE key start end`|Get entries within a range of IDs.|`XRANGE mystream - +`|
|`XREAD COUNT count STREAMS key ID`|Read entries starting from a specific ID.|`XREAD COUNT 2 STREAMS mystream 0`|
|`XDEL key ID`|Delete an entry by ID.|`XDEL mystream 1234567890-0`|
|`XLEN key`|Get the length of the stream.|`XLEN mystream`|
|`XGROUP CREATE groupname key id`|Create a consumer group.|`XGROUP CREATE group1 mystream $`|

#### **Example**: