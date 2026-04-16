
**Tables**

- To avoid erro r in case table already exists use : `CREATE TABLE IF NOT EXISTS users;`

```sql
CREATE TABLE users (
    id SERIAL PRIMARY KEY,
    username VARCHAR(50) UNIQUE NOT NULL,
    email VARCHAR(100) NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

```
- In Postgres, `CREATE TABLE` also supports `INHERITS`, partitioning, constraints, etc.


**Schema**

```sql
CREATE SCHEMA analytics;
```

**Index**

```sql
CREATE INDEX idx_users_username ON users(username);
```

