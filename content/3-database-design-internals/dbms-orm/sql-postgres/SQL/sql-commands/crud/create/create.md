

**create a table:**
```
CREATE TABLE employees (
    id SERIAL PRIMARY KEY,
    name TEXT NOT NULL,
    age INT,
    department TEXT,
    salary NUMERIC
);
```


**insert data**
```sql
-- Insert a single row
INSERT INTO employees (name, age, department, salary)
VALUES ('Alice', 30, 'HR', 55000);

INSERT INTO employees (id, name, age, department, salary)
VALUES (112,  'Alice', 30, 'HR', 55000);


-- Insert multiple rows
INSERT INTO employees (name, age, department, salary)
VALUES 
  ('Bob', 28, 'IT', 60000),
  ('Charlie', 35, 'Finance', 70000);

```