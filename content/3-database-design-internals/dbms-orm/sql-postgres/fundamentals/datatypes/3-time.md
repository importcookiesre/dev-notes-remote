

## 1. DATE : 

- stores : only date
- format : `YYYY-MM-DD`

```sql
CREATE TABLE users (
    dob DATE
);

INSERT INTO users VALUES ('2000-05-15');
```


## 2. TIME : 

- stores : time only 

```sql
CREATE TABLE schedule (
    start_time TIME
);

INSERT INTO schedule VALUES ('14:30:00');
```


## 3. TIMESTAMP 

- stores : date + time
- no timezone awareness

```sql
CREATE TABLE logs (
    created_at TIMESTAMP
);

INSERT INTO logs VALUES ('2026-03-17 14:30:00');
```

- PostgreSQL **does NOT store timezone**
- It assumes value is "local"
👉 This causes bugs in distributed systems



## 4. TIMESTAMPTZ : recommanded

- stores in UTC internally.
- converted based on client timezone when queried.

```sql
CREATE TABLE events (
    created_at TIMESTAMPTZ
);

INSERT INTO events VALUES (NOW());
```


### change timezone : 


```sql
SET TIME ZONE 'Asia/Kolkata';
SELECT NOW();


SET TIME ZONE 'UTC';
SELECT NOW();
```


## 5. INTERVAL : 

- stores duration : 
Use case
- Expiry times
- Retry delays
- Scheduling logic

```sql
SELECT INTERVAL '2 days';
SELECT INTERVAL '3 hours 30 minutes';
```


```sql
SELECT NOW() + INTERVAL '1 day';
```