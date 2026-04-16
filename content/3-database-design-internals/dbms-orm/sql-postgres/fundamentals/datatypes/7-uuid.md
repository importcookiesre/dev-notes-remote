


## 1. UUID : 

**UUID = Universally Unique Identifier**
It is a **128 bit identifier** designed to be unique across space and time. `2^128 ≈ 3.4 × 10^38`

```sql
CREATE TABLE users (
    id uuid PRIMARY KEY,
    name text
);

-- insert values
INSERT INTO users (id, name)
VALUES (
    '550e8400-e29b-41d4-a716-446655440000',
    'suraj'
);
```


```sql
CREATE TABLE users (
    id uuid DEFAULT uuidv7() PRIMARY KEY,
    name text
);

-- insert values
INSERT INTO users (name)
VALUES (
    'suraj'
);
```
### UUID generation via extension : 

| Method             | Extension               | Version |
| ------------------ | ----------------------- | ------- |
| uuid_generate_v4() | uuid-ossp               | older   |
| gen_random_uuid()  | pgcrypto                | modern  |
| uuidv7()           | built-in newer versions |         |

```sql
CREATE EXTENSION IF NOT EXISTS pgcrypto;
CREATE TABLE users (
    id uuid PRIMARY KEY DEFAULT uuid_generate_v4(),
    name text
);

CREATE EXTENSION IF NOT EXISTS pgcrypto;
CREATE TABLE users (
    id uuid PRIMARY KEY DEFAULT gen_random_uuid(),
    name text
);

CREATE TABLE users (
    id uuid PRIMARY KEY DEFAULT uuidv7(),
    name text
);
```


pgcrypto : 

```sql
CREATE EXTENSION IF NOT EXISTS "pgcrypto";

CREATE TABLE orders (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    amount NUMERIC(10,2)
);

INSERT INTO orders (amount) VALUES (100.50), (250.75);

SELECT * FROM orders;
```