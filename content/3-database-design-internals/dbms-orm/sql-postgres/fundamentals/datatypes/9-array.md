- Allows storing **multiple values in one column**
- Same data type for all elements.
- postgres arrray are 1-based (not 0-based)
- perfer normalized table

use cases : 
- tags, labels,
- permissions
- storing multiple IDs

## basic syntax : 

```sql
CREATE TABLE users (
    id BIGINT,
    tags TEXT[]
);
```

## insert : 
```sql
INSERT INTO users (id, tags)
VALUES 
(1, ARRAY['go', 'postgres', 'docker']),
(2, ARRAY['java', 'spring']);


-- alternative
INSERT INTO users (id, tags)
VALUES (3, '{node,react}');
```


## query  :


```sql
-- get full array 
SELECT tags FROM users;

-- index based 
SELECT tags[1] FROM users;
```

## filter : 


```sql
SELECT * FROM users
where 'go' = ANY(tags);
```

## contains operator : 

```sql
SELECT * FROM users
WHERE tags @> ARRAY['go'];
```
👉 Means:
> tags contains 'go'


## overlap : 

```sql
SELECT * FROM users
WHERE tags && ARRAY['go', 'python'];
```

👉 Means:
> any common value



## updating : 

### 1. append : 


```sql
UPDATE users
SET tags = array_append(tags, 'kubernetes')
WHERE id = 1;
```


### 2. remove element : 

```sql
UPDATE users
SET tags = array_remove(tags, 'docker')
WHERE id = 1;
```