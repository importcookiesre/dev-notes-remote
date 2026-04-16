

# 1. soft delete : 


- The row still remains physically in the database but it is treated as deleted by the application.
- Every read operation must exclude it.
- Soft delete is just an update operation.

## 1. Why soft delete exists ? 

Hard delete fails in real systems because of:

• Accidental deletion  
• Debugging needs  
• Audit and compliance requirements  
• Data recovery  
• Asynchronous systems (eventual consistency)  
• Distributed references (foreign services still holding IDs)

Once data is hard-deleted:
- You cannot reconstruct history
- You cannot explain “what happened”
- You cannot recover user mistakes

This is why **most production systems default to soft delete**.

## 2. Rules :
**Every SELECT must exclude deleted rows by default**

If you forget this once, you leak deleted data.
This condition must appear in:
- SELECT
- UPDATE
- DELETE (soft delete update itself)
- JOINs
- Subqueries
    
Soft delete is useless without **discipline**.

```go
WHERE deleted_at IS NULL
```

### 1. soft delete and unique constraint : 

- In users table, if a user is soft deleted the email cannot be reused. It still exists in the db and new data cannot be created with the same email. That is usually wrong.


**solution** : partial unique index
```go
CREATE UNIQUE INDEX users_email_active_key
ON users(email)
WHERE deleted_at IS NULL;
```
Now:
- Active users must be unique
- Deleted users do not block new inserts


### 2. Soft delete and foreign keys : 

Soft delete does **not** trigger `ON DELETE` behavior.

That means:
- Child rows remain untouched
- Referential integrity is logical, not physical
This is good, but **you must handle it intentionally**.
Typical pattern:
- Parent soft-deleted
- Children remain but filtered out by joins


### 3. restore : 
You can:
- Restore accounts
- Re-enable data
- Recover from mistakes
```go
UPDATE users
SET deleted_at = NULL
WHERE id = $1;
```


### 4. Retention and hard delete : 

Typical production strategy
1. Soft delete immediately
2. Keep data for 30–180 days
3. Background job hard deletes old soft-deleted rows
```go
DELETE FROM users
WHERE deleted_at < now() - interval '90 days';
```


### 5. performance : partial indexes

`WHERE deleted_at IS NULL` on large tables can be slow.
```go
CREATE INDEX users_active_idx
ON users(id)
WHERE deleted_at IS NULL;
```
Every hot query should be backed by a **partial index**.



### 6. Avoid soft delete when : 

Avoid it when:
- Data is ephemeral (sessions, cache, tokens)
- High-write, short-lived tables
- GDPR requires immediate erasure
- Data volume is extreme (logs, metrics)


## 3. Soft delete design pattern : 

### 1. `deleted_at` timestamp 

```go
CREATE TABLE users (
	id SERIAL PRIMARY KEY,
	name VARCHAR(64) NOT NULL,
	email VARCHAR(255) NOT NULL UNIQUE,
	deleted_at TIMESTAMP DEFAULT NULL
);
```
- `NULL` → active
- non-NULL → deleted
Why timestamp instead of boolean:
- You know **when** deletion happened
- Supports retention policies
- Useful for audits and restores


### 2. `is_deleted` boolean : not recommanded alone : 

```go
CREATE TABLE users (
	id SERIAL PRIMARY KEY,
	name VARCHAR(64) NOT NULL,
	email VARCHAR(255) NOT NULL UNIQUE,
	id_deleted boolean DEFAULT false
);
```
Problems:
- No deletion time
- No audit value
- Weak recovery semantics
Acceptable only for **temporary or internal tables**.


### 3. Soft delete with metadata (advanced)

```go
CREATE TABLE users (
	id SERIAL PRIMARY KEY,
	name VARCHAR(64) NOT NULL,
	email VARCHAR(255) NOT NULL UNIQUE,
	deleted_at TIMESTAMP DEFAULT NULL
	deleted_by BIGINT NULL,
	delete_reason TEXT NULL;
);
```
Used in:
- Enterprise systems
- Admin panels
- Moderation workflows


## retrival pattern : 

### 1. select operation 

- It must not query deleted rows
```go
SELECT id, email
FROM users
WHERE id = $1 AND deleted_at IS NULL;
```


### 2. joins 

```go
SELECT *
FROM orders
JOIN users
  ON users.id = orders.user_id
 AND users.deleted_at IS NULL;
```
- Deleted users still appear via joins
- Bugs surface months later
