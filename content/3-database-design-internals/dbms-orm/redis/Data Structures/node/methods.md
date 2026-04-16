

| **Category**             | **Method**      | **Description**                                                | **Example**                                                       |
| ------------------------ | --------------- | -------------------------------------------------------------- | ----------------------------------------------------------------- |
| **Initialization**       | `new Redis()`   | Creates a new Redis client instance.                           | `const redis = new Redis();`                                      |
|                          | `on('connect')` | Event listener triggered when the connection is established.   | `redis.on('connect', () => console.log('Connected to Redis'));`   |
|                          | `on('error')`   | Event listener triggered when an error occurs.                 | `redis.on('error', (err) => console.error('Redis error:', err));` |
| **Existence Check**      | `exists()`      | Checks if a key exists.                                        | `const exists = await redis.exists('key');`                       |
| **Basic Operations**     | `set()`         | Sets a key-value pair in Redis.                                | `await redis.set('key', 'value');`                                |
|                          | `get()`         | Retrieves the value associated with a key.                     | `const value = await redis.get('key');`                           |
|                          | `del()`         | Deletes one or more keys.                                      | `await redis.del('key');`                                         |
| **TTL Management**       | `expire()`      | Sets a time-to-live (TTL) for a key in seconds.                | `await redis.expire('key', 3600);`                                |
|                          | `ttl()`         | Retrieves the remaining TTL for a key.                         | `const ttl = await redis.ttl('key');`                             |
|                          | `persist()`     | Removes the TTL from a key, making it permanent.               | `await redis.persist('key');`                                     |
| **Data Types - Strings** | `incr()`        | Increments the value of a key by 1 (key must hold an integer). | `await redis.incr('counter');`                                    |
|                          | `decr()`        | Decrements the value of a key by 1.                            | `await redis.decr('counter');`                                    |
|                          | `append()`      | Appends a value to an existing key.                            | `await redis.append('key', 'more data');`                         |
| **Data Types - Lists**   | `lpush()`       | Pushes a value onto the head of a list.                        | `await redis.lpush('list', 'value');`                             |
|                          | `rpush()`       | Pushes a value onto the tail of a list.                        | `await redis.rpush('list', 'value');`                             |
|                          | `lrange()`      | Retrieves a range of elements from a list.                     | `const items = await redis.lrange('list', 0, -1);`                |
|                          | `lpop()`        | Removes and returns the first element of a list.               | `const first = await redis.lpop('list');`                         |
| **Data Types - Sets**    | `sadd()`        | Adds one or more members to a set.                             | `await redis.sadd('set', 'member1', 'member2');`                  |
|                          | `smembers()`    | Retrieves all members of a set.                                | `const members = await redis.smembers('set');`                    |
|                          | `srem()`        | Removes one or more members from a set.                        | `await redis.srem('set', 'member1');`                             |
| **Data Types - Hashes**  | `hset()`        | Sets one or more fields in a hash.                             | `await redis.hset('hash', 'field', 'value');`                     |
|                          | `hget()`        | Retrieves the value of a field in a hash.                      | `const value = await redis.hget('hash', 'field');`                |
|                          | `hgetall()`     | Retrieves all fields and values in a hash.                     | `const fields = await redis.hgetall('hash');`                     |
| **Pub/Sub**              | `subscribe()`   | Subscribes to a channel to receive messages.                   | `await redis.subscribe('channel');`                               |
|                          | `publish()`     | Publishes a message to a channel.                              | `await redis.publish('channel', 'Hello Redis!');`                 |
| **Transaction**          | `multi()`       | Starts a transaction.                                          | `const pipeline = redis.multi();`                                 |
|                          | `exec()`        | Executes a transaction.                                        | `await pipeline.set('key', 'value').incr('counter').exec();`      |
| **Monitoring**           | `keys()`        | Retrieves all keys matching a pattern.                         | `const keys = await redis.keys('*');`                             |
|                          | `info()`        | Retrieves server information and statistics.                   | `const info = await redis.info();`                                |
|                          | `flushall()`    | Removes all keys from the database.                            | `await redis.flushall();`                                         |

---