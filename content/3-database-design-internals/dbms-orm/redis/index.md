Redis (Remote Dictionary Server) is an **open-source, in-memory, NoSQL database** known for its speed and versatility. It stores data in a **key-value format** and supports various data structures such as strings, hashes, lists, sets, and sorted sets.

- Despite being an in-memory database, Redis supports persistence by periodically saving data to disk or logging every write operation (RDB and AOF).

[Commands](https://quickref.me/redis.html#redis-string-command)


### **Table of Redis Commands

| **Command**            | **Description**                                                             | **Example**                  |
| ---------------------- | --------------------------------------------------------------------------- | ---------------------------- |
| **`SELECT`**           | Select a database by its index (default is `0`).                            | `SELECT 1`                   |
| **`DBSIZE`**           | Get the number of keys in the selected database.                            | `DBSIZE`                     |
| **`FLUSHDB`**          | Remove all keys from the currently selected database.                       | `FLUSHDB`                    |
| **`FLUSHALL`**         | Remove all keys from all databases.                                         | `FLUSHALL`                   |
| **`MOVE`**             | Move a key from the current database to another database.                   | `MOVE mykey 2`               |
| **`SAVE`**             | Synchronously save the dataset to disk (creating a dump.rdb file).          | `SAVE`                       |
| **`BGSAVE`**           | Asynchronously save the dataset to disk (non-blocking).                     | `BGSAVE`                     |
| **`LASTSAVE`**         | Get the Unix timestamp of the last successful `SAVE` or `BGSAVE` operation. | `LASTSAVE`                   |
| **`SHUTDOWN`**         | Shutdown the Redis server.                                                  | `SHUTDOWN SAVE`              |
| **`INFO`**             | Get information and statistics about the server.                            | `INFO`                       |
| **`CONFIG GET`**       | Get the value of a configuration parameter.                                 | `CONFIG GET *`               |
| **`CONFIG SET`**       | Set a Redis server configuration parameter.                                 | `CONFIG SET maxmemory 100mb` |
| **`CONFIG RESETSTAT`** | Reset all Redis statistics.                                                 | `CONFIG RESETSTAT`           |
| **`CLIENT LIST`**      | Get a list of client connections.                                           | `CLIENT LIST`                |
| **`CLIENT KILL`**      | Kill a client connection by its address or ID.                              | `CLIENT KILL 127.0.0.1:6379` |
| **`CLIENT GETNAME`**   | Get the name of the current client connection.                              | `CLIENT GETNAME`             |
| **`CLIENT SETNAME`**   | Set a name for the current client connection.                               | `CLIENT SETNAME "myClient"`  |
| **`MONITOR`**          | Listen to all commands processed by the Redis server.                       | `MONITOR`                    |
| **`SLAVEOF`**          | Make the server a master or a slave of another server.                      | `SLAVEOF 127.0.0.1 6379`     |
| **`SLOWLOG`**          | Get and manage the Redis slow log.                                          | `SLOWLOG GET`                |
| **`LATENCY`**          | Measure latency-related statistics.                                         | `LATENCY DOCTOR`             |
| **`TIME`**             | Get the current server time.                                                | `TIME`                       |
| **`WATCH`**            | Mark one or more keys to be watched for conditional operations.             | `WATCH mykey`                |
| **`UNWATCH`**          | Discard all watched keys.                                                   | `UNWATCH`                    |
| **`DISCARD`**          | Discard the current transaction (after `MULTI`).                            | `DISCARD`                    |
| **`MULTI`**            | Start a new transaction block.                                              | `MULTI`                      |
| **`EXEC`**             | Execute all commands in the transaction block.                              | `EXEC`                       |
| **`UNLINK`**           | Remove one or more keys asynchronously.                                     | `UNLINK mykey`               |
| **`PING`**             | Check if the server is alive.                                               | `PING`                       |
| **`EVAL`**             | Execute a Lua script.                                                       | `EVAL "return 1" 0`          |
| **`SCRIPT LOAD`**      | Load a Lua script into the script cache.                                    | `SCRIPT LOAD "return 1"`     |
| **`SCRIPT FLUSH`**     | Flush the Lua script cache.                                                 | `SCRIPT FLUSH`               |
| **`SCRIPT EXISTS`**    | Check if a Lua script exists in the cache.                                  | `SCRIPT EXISTS <script_sha>` |
| **`AUTH`**             | Authenticate to the Redis server using a password.                          | `AUTH password`              |


---

### **Explanation of Common Commands**:

- **`SELECT`**: Switches between Redis databases. Redis has a default of 16 databases, with the first one being selected by default.
    
- **`FLUSHDB` & `FLUSHALL`**: Useful for clearing all data from the current database or from all databases.
    
- **`INFO`**: Provides essential server statistics like memory usage, CPU load, and connected clients.
    
- **`SHUTDOWN`**: Shuts down the Redis server, which can be done with or without saving data depending on the command option.
    
- **`MONITOR`**: Useful for debugging, as it lets you monitor all commands being executed in real-time by the Redis server.
    
- **`CLIENT`**: Helps you manage client connections. This is particularly useful in scenarios where you have many Redis clients connected to the server.
    
- **`SLOWLOG`**: Useful for performance monitoring, showing slow queries that could be affecting Redis' efficiency.
    
- **`LATENCY`**: Useful for measuring Redis server latency and diagnosing performance issues.
    
- **`WATCH` & `UNWATCH`**: These commands are used to monitor specific keys for changes, particularly in transactions where the database state matters.