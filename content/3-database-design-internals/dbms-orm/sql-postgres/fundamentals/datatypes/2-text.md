


## 1. TEXT 

- no limit
- most flexible

```sql
CREATE TABLE users (
    name TEXT
);

INSERT INTO users (name) VALUES ('Suraj'), ('A very long string...');
```


## 2. VARCHAR(N)

- variable length, with limit

```sql
CREATE TABLE users (
    username VARCHAR(20)
);

INSERT INTO users VALUES ('suraj123');
```

exceeding limit will return error.

VARCHAR without limit is equivalent to TEXT.

```sql
CREATE TABLE users (
    name VARCHAR
);

INSERT INTO users (name) VALUES ('Suraj'), ('A very long string...');
```


## CHAR(n) : not recommanded (weird behaviour)