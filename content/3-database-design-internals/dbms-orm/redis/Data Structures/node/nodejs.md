
- Use `JSON.stringify()` and `JSON.parse()` to handle complex data:
#### Connect redis

```
new Redis(); // Connect to 127.0.0.1:6379
new Redis(6380); // 127.0.0.1:6380
new Redis(6379, "192.168.1.1"); // 192.168.1.1:6379
new Redis("/tmp/redis.sock");
new Redis({
	port: 6379, // Redis port
	host: "127.0.0.1", // Redis host
	username: "default", // needs Redis >= 6
	password: "my-top-secret",
	db: 0, // Defaults to 0
});
```

#### Configuration

```
import Redis from 'ioredis';

// Initialize Redis client
const redis = new Redis({
    host: '127.0.0.1', // Default Redis host
    port: 6379,        // Default Redis port
    // password: 'your_password', // Uncomment if Redis has authentication
});

// Handle connection errors
redis.on('connect', () => {
    console.log('Connected to Redis');
});

redis.on('error', (err) => {
    console.error('Redis connection error:', err);
});

export default redis;
```

- Use `JSON.stringify()` and `JSON.parse()` to handle complex data: