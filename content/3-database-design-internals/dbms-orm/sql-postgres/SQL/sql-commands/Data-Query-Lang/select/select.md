
- The `SELECT` statement is used to **retrieve data** from one or more tables in a database.
- use `AS` to change the name of column:
	- `select product_id AS id from ...`

**Syntax**
```sql
SELECT column1, column2, ... FROM table_name;
```

### **Types of `SELECT` Queries**

| **Type**                              | **Description**                     | **Example**                                                                                                           |
| ------------------------------------- | ----------------------------------- | --------------------------------------------------------------------------------------------------------------------- |
| **Basic SELECT**                      | Retrieves specific columns          | `SELECT name, age FROM employees;`                                                                                    |
| **SELECT  (All Columns)**             | Retrieves all columns               | `SELECT * FROM employees;`                                                                                            |
| **Filtering with WHERE**              | Retrieves specific rows             | `SELECT * FROM employees WHERE age > 30;`                                                                             |
| **Sorting with ORDER BY**             | Sorts the result set                | `SELECT * FROM employees ORDER BY age DESC;`                                                                          |
| **Removing Duplicates (DISTINCT)**    | Returns unique values               | `SELECT DISTINCT department FROM employees;`                                                                          |
| **Using Aliases (AS)**                | Renames columns or tables           | `SELECT name AS employee_name FROM employees;`                                                                        |
| **Limiting Results (LIMIT)**          | Restricts number of rows            | `SELECT * FROM employees LIMIT 5;`                                                                                    |
| **Using GROUP BY**                    | Groups data for aggregation         | `SELECT department, COUNT(*) FROM employees GROUP BY department;`                                                     |
| **Using HAVING**                      | Filters grouped data                | `SELECT department, COUNT(*) FROM employees GROUP BY department HAVING COUNT(*) > 5;`                                 |
| **Using JOIN**                        | Combines data from multiple tables  | `SELECT employees.name, departments.dept_name FROM employees JOIN departments ON employees.dept_id = departments.id;` |
| **Subqueries (SELECT within SELECT)** | Retrieves data using nested queries | `SELECT name FROM employees WHERE salary > (SELECT AVG(salary) FROM employees);`                                      |

---

#### DISTINCT : 

```sql
-- SELECT DISTINCT column_name FROM table_name;
SELECT DISTINCT department FROM employee;
```

#### WHERE : 
```
SELECT * FROM employees WHERE age > 30;
```

#### ORDER BY : 
```sql
SELECT * FROM employees ORDER BY salary DESC;
SELECT * FROM employees ORDER BY salary ASC;
```
#### Column Aliases (`AS`)
```sql
SELECT name AS employee_name FROM employees;
```

🚀 **Effect:** Renames the `name` column as `employee_name` in the result.


#### LIMIT : 
```sql
SELECT * FROM employee LIMIT 5;
SELECT * FROM employees LIMIT 5 OFFSET 10; -- retruns 5 rows skipping first 10 rows
```



#### filter : 

```sql
SELECT * FROM users WHERE created_at > NOW() - INTERVAL '7 days';
```



### WHERE : 

The `WHERE` clause in SQL is used to filter records based on a specified condition. It is commonly used in `SELECT`, `UPDATE`, `DELETE`, and `INSERT` statements to retrieve or modify only the rows that meet certain criteria.

```sql
-- syntax
SELECT column1, column2, ...
FROM table_name
WHERE condition;
```



## **Aggregate Function** : 

Aggregate functions in SQL perform calculations on multiple rows and return a single summary value. These functions are commonly used with the `GROUP BY` clause to group records and perform computations on each group.

#### 1. Common Aggregate Functions

| Function  | Description                                   | Example                              |
| --------- | --------------------------------------------- | ------------------------------------ |
| `COUNT()` | Counts the number of rows                     | `SELECT COUNT(*) FROM employees;`    |
| `SUM()`   | Calculates the total sum of a numeric column  | `SELECT SUM(salary) FROM employees;` |
| `AVG()`   | Returns the average value of a numeric column | `SELECT AVG(salary) FROM employees;` |
| `MIN()`   | Returns the smallest value in a column        | `SELECT MIN(salary) FROM employees;` |
| `MAX()`   | Returns the largest value in a column         | `SELECT MAX(salary) FROM employees;` |
|           |                                               |                                      |
```sql
select max(id),count(id), sum(age), avg(id),  min(id) from engineers;
```


#### 2. GROUP BY : 

The `GROUP BY` clause in SQL is used to group rows that have the same values in specified columns into aggregated data. It is typically used with aggregate functions like `COUNT()`, `SUM()`, `AVG()`, `MAX()`, and `MIN()` to perform operations on each group.

```sql
SELECT column_name, aggregate_function(column_name)
FROM table_name
GROUP BY column_name;
```



 **Ques** :  In the following table, find the total amount, total products each category.

| id  | product | category    | amount |
| --- | ------- | ----------- | ------ |
| 1   | Laptop  | Electronics | 1200   |
| 2   | Phone   | Electronics | 800    |
| 3   | Shirt   | Clothing    | 50     |
| 4   | Laptop  | Electronics | 1300   |
| 5   | Shirt   | Clothing    | 60     |

```sql
select category , count(product) as products_count , sum(amount) as total_amount
from products 
group by category
```

| category    | total_amount | products_count |
| ----------- | ------------ | -------------- |
| Electronics | 3300         | 3              |
| Clothing    | 110          | 2              |

#### 3. Filtering with `HAVING` 

Since aggregate functions cannot be used directly in `WHERE`, we use `HAVING` for filtering aggregated results.

```
select category , count(product) as products_count , sum(amount) as total_amount
from products 
group by category
having count(*) >= 3 
```