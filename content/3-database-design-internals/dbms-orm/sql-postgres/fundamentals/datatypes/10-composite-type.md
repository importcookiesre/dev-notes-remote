
- They are custom types .
- Use normalized table (recommanded)

```sql
CREATE TYPE address AS (
    street TEXT,
    city TEXT,
    pincode TEXT
);
```

use it
```sql
CREATE TABLE users (
    id BIGINT,
    home_address address
);
```

insert data : 

```sql
INSERT INTO users (id, home_address)
VALUES 
(1, ('Main St', 'Ranchi', '834001'));
```

query : 

```sql
SELECT home_address FROM users;

-- returns : (Main St,Ranchi,834001)
```

access fields : 

```sql
SELECT 
    (home_address).city,
    (home_address).pincode
FROM users;
```


## example : 



```sql
CREATE TYPE point2d AS (
    x INT,
    y INT
);

CREATE TABLE shapes (
    id SERIAL,
    position point2d
);

INSERT INTO shapes (position)
VALUES ((10, 20));
```
query
```sql
SELECT (position).x, (position).y FROM shapes;
```