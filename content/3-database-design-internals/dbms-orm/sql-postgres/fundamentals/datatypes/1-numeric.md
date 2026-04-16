

## 1. SMALLINT (int16)

- size :  16 bits


```sql
CREATE TABLE ratings (
    score SMALLINT
);

INSERT INTO ratings (score) VALUES (5), (10), (32767);
```


## 2. INTEGER / INT : (int32)

- size : 32 bits
- range : ~ -2 billion to 2 billion

```sql
CREATE TABLE users (
    id INTEGER
);

INSERT INTO users (id) VALUES (1), (1000), (2000000000);
```


### 3. BIGINT :  int64

- size : 64 bits
- range : ~ +-9 quintrillion


```sql
CREATE TABLE orders (
    id BIGINT
);

INSERT INTO orders (id) VALUES (9223372036854775807);
```


## 4. SERIAL / BIGSERIAL : autoincrement

| Type        | Backed by |
| ----------- | --------- |
| SERIAL      | INTEGER   |
| BIGSERIAL   | BIGINT    |
| SMALLSERIAL | SMALLINT  |
```sql
CREATE TABLE users (
    id SERIAL PRIMARY KEY,
    name TEXT
);
```


## 5. REAL : float32

- size : 4 bytes
- precision : ~6 decimal digits

```sql
CREATE TABLE measurements (
    temp REAL
);

INSERT INTO measurements VALUES (3.1415926535);

-- stored as :
-- 3.14159
```

## 6. DOUBLE PRECISION : float64

- size : bytes
- precision : 15 decimal digits

```sql
CREATE TABLE physics (
    value DOUBLE PRECISION
);

INSERT INTO physics VALUES (3.141592653589793);
```

## 7. NUMERIC / DECIMAL : 

DECIMAL(10,2) same as NUMERIC(10,2)

NUMERIC(precision, scale)
- **precision (10)** → total number of digits
- **scale (2)** → digits after decimal


### `NUMERIC(10,2)`
- Total digits allowed = **10**
- Decimal digits = **2**
- Integer digits = **10 - 2 = 8**

👉 So max value is:

`99999999.99`


```go
INSERT INTO test VALUES (123.45);      -- 5 digits total
INSERT INTO test VALUES (99999999.99); -- max limit
INSERT INTO test VALUES (0.99);

INSERT INTO test VALUES (123.456);
-- ERROR: scale exceeded (more than 2 decimal places)


INSERT INTO test VALUES (123456789.12);
-- ERROR: exceeds 10 total digits
```


```sql
CREATE TABLE products (
    id BIGINT GENERATED ALWAYS AS IDENTITY,
    name TEXT,
    price NUMERIC(10, 2)
);

INSERT INTO products (name, price)
VALUES 
('Laptop', 99999.99),
('Mouse', 499.50);
```

tax calculation : 
```sql
CREATE TABLE invoices (
    id BIGINT GENERATED ALWAYS AS IDENTITY,
    amount NUMERIC(12,2),
    tax_rate NUMERIC(5,2)  -- e.g. 18.00%
);

INSERT INTO invoices (amount, tax_rate)
VALUES (1000.00, 18.00);
```