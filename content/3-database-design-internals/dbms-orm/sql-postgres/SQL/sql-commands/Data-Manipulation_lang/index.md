

## What is DML (Data Manipulation Layer) ?

- commands deals with data inside database.


### **1. INSERT Commands Table**



```sql
-- single row
INSERT INTO users (username, email)
VALUES ('alice', 'alice@example.com');

-- multiple row
INSERT INTO users (username, email)
VALUES 
  ('bob', 'bob@example.com'),
  ('charlie', 'charlie@example.com');

-- insert from query
INSERT INTO archived_users (id, username, email)
SELECT id, username, email FROM users WHERE last_login < NOW() - INTERVAL '1 year';

```


| **Command**                                                                            | **Description**                                       | **Example**                                                                        |
| -------------------------------------------------------------------------------------- | ----------------------------------------------------- | ---------------------------------------------------------------------------------- |
| `INSERT INTO table_name (columns) VALUES (values);`                                    | Adds a new row with specific columns                  | `INSERT INTO employees (id, name, age, salary) VALUES (1, 'John Doe', 30, 50000);` |
| `INSERT INTO table_name VALUES (values);`                                              | Adds a new row, specifying all column values in order | `INSERT INTO employees VALUES (2, 'Jane Doe', 28, 60000);`                         |
| `INSERT INTO table_name (columns) SELECT columns FROM another_table;`                  | Inserts data from another table                       | `INSERT INTO employees_archive SELECT * FROM employees WHERE age > 50;`            |
| `INSERT INTO table_name (column1, column2) VALUES (value1, value2), (value3, value4);` | Inserts multiple rows in one query                    | `INSERT INTO employees (name, age) VALUES ('Alice', 25), ('Bob', 30);`             |

---

### **2. UPDATE Commands Table**

- always use where clause otherwise it will perform operation in all the rows.


| **Command**                                                               | **Description**                             | **Example**                                                   |
| ------------------------------------------------------------------------- | ------------------------------------------- | ------------------------------------------------------------- |
| UPDATE table_name SET column = value WHERE condition;                     | Updates a specific column for matching rows | `UPDATE employees SET salary = 70000 WHERE id = 1;`           |
| UPDATE table_name SET column1 = value1, column2 = value2 WHERE condition; | Updates multiple columns at once            | `UPDATE employees SET age = 35, salary = 80000 WHERE id = 2;` |
| `UPDATE table_name SET column = value;`                                   | Updates all rows (Be **careful**!)          | `UPDATE employees SET salary = 90000;`                        |
| `UPDATE table_name SET column = column + value WHERE condition;`          | Increments column value                     | `UPDATE employees SET salary = salary + 5000 WHERE id = 3;`   |

---

### **3. DELETE Commands Table**

| **Command**                                                                  | **Description**                            | **Example**                                                             |
| ---------------------------------------------------------------------------- | ------------------------------------------ | ----------------------------------------------------------------------- |
| `DELETE FROM table_name WHERE condition returning *;`                        | Deletes specific rows from a table         | `DELETE FROM employees WHERE id = 2  returning *;`                      |
| `DELETE FROM table_name;`                                                    | Deletes all rows (Be **careful**!)         | `DELETE FROM employees;`                                                |
| `TRUNCATE TABLE table_name;`                                                 | Deletes all rows but keeps table structure | `TRUNCATE TABLE employees;`                                             |
| `DELETE FROM table_name WHERE column IN (SELECT column FROM another_table);` | Deletes rows matching another table        | `DELETE FROM employees WHERE id IN (SELECT id FROM employees_archive);` |
