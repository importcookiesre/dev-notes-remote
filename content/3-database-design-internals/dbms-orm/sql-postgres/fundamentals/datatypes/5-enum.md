



## 1. Creating an ENUM type : 

In PostgreSQL, enums are created using `CREATE TYPE`.

```sql
CREATE TYPE order_status AS ENUM (
    'pending',
    'processing',
    'shipped',
    'delivered',
    'cancelled'
);
```
Now `order_status` becomes a **new datatype**.


## 2. Using : 

use the enum datatype
```sql
CREATE TABLE orders (
    id SERIAL PRIMARY KEY,
    user_id INT,
    status order_status NOT NULL
);
```

insert values :
```sql
INSERT INTO orders (user_id, status)
VALUES
(1, 'pending'),
(2, 'processing'),
(3, 'delivered');
```

query
```sql
SELECT *
FROM orders
WHERE status = 'pending';
```

## 3. Inspect enum values :

```sql
SELECT enum_range(NULL::order_status);
```


## 4. Add a new value to enum : 

```sql
ALTER TYPE order_status
ADD VALUE 'returned';
```

Add enum value at a specific position.
Postgres also allows ordering:

```sql
ALTER TYPE order_status
ADD VALUE 'refunded' AFTER 'cancelled';
```

## rename enum value : 

```sql
ALTER TYPE delivery_status RENAME VALUE 'PACKAGING' TO 'PREPARING';
```
## 5. Ordering ENUM values :

Enums have **internal ordering** based on the order they were defined.

```sql
SELECT *
FROM orders
ORDER BY status;
```
result will follow the enum order :

```sql
pending
processing
shipped
delivered
cancelled
```

## 6. List all Enum types :

```sql
SELECT typname
FROM pg_type
WHERE typtype = 'e';
```

## 7. List enum values :


```sql
SELECT enumlabel
FROM pg_enum
JOIN pg_type ON pg_enum.enumtypid = pg_type.oid
WHERE typname = 'order_status';
```

## 8. Drop an enum :

```sql
DROP TYPE order_status;
```
