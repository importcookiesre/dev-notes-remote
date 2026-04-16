- postgres validates the json.

| Feature        | JSON      | JSONB         |
| -------------- | --------- | ------------- |
| Storage        | Text      | Binary        |
| Read speed     | Slow      | Fast          |
| Indexing       | ❌ No      | ✅ Yes         |
| Key order      | Preserved | Not preserved |
| Duplicate keys | Allowed   | Removed       |

## 1.JSON : 

- stores exact input text.

- Preserves:
	- formatting
	- key order
	- duplicate keys

```sql
CREATE TABLE logs (
    data JSON
);

INSERT INTO logs (data)
VALUES ('{"name": "suraj", "age": 25}');
```





## 2. JSONB : binary json :  (recommanded)


- Stored in **binary format**
- Removes:
    - whitespace
    - key order
        
- **Faster queries**
- Supports indexing


### example
```sql
CREATE TABLE logs (
    data JSONB
);

INSERT INTO logs (data)
VALUES ('{"name": "suraj", "age": 25}');
```

```sql
CREATE TABLE test_json (
    id SERIAL,
    data JSONB
);

INSERT INTO test_json (data)
VALUES 
('{"name": "suraj", "skills": ["go", "postgres"]}'),
('{"name": "rahul", "skills": ["java"]}');


-- query
SELECT * FROM test_json
WHERE data->>'name' = 'suraj';
```

inserting value : 
```sql
INSERT INTO logs (data)
VALUES ('{"name": "suraj", "age": 25, "city": "Ranchi"}');
```

insert nested json : 

```sql
INSERT INTO logs (data)
VALUES ('{
  "user": {
    "name": "suraj",
    "age": 25
  }
}');


-- Query 
SELECT data->'user'->>'name' FROM logs;

```

accessing field : 

```sql
SELECT data->'name' FROM logs;

-- returns json value : "suraj"

SELECT data->>'name' FROM logs;

-- returns text value : suraj
```

filter by value ; 

```sql
SELECT * FROM logs
WHERE data->>'name' = 'suraj';
```

check if key exist : 

```sql
SELECT * FROM logs
WHERE data ? 'age';
```

conatinment : 
👉 Finds rows containing this structure
```sql
SELECT * FROM logs
WHERE data @> '{"name": "suraj"}';

```



### indexing on json/jsonb


```sql
CREATE INDEX idx_logs_data ON logs USING GIN (data);
```
💡 Without index → slow  
💡 With GIN index → fast queries

