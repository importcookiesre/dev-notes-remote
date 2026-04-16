
| Clause        | Description                                                                 | Example                                                                                                             |
| ------------- | --------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------- |
| **SELECT**    | Specifies the columns to return.                                            | `SELECT name, age FROM employees;`                                                                                  |
| **FROM**      | Specifies the table(s) to query data from.                                  | `SELECT * FROM employees;`                                                                                          |
| **WHERE**     | Filters rows based on a condition.                                          | `SELECT * FROM employees WHERE age > 30;`                                                                           |
| **GROUP BY**  | Groups rows sharing values into summary rows.                               | `SELECT department, COUNT(*) FROM employees GROUP BY department;`                                                   |
| **HAVING**    | Filters groups based on a condition (used with GROUP BY).                   | `SELECT department, AVG(salary) FROM employees GROUP BY department HAVING AVG(salary) > 50000;`                     |
| **ORDER BY**  | Sorts the result set by one or more columns.                                | `SELECT name, salary FROM employees ORDER BY salary DESC;`                                                          |
| **LIMIT**     | Limits the number of rows returned.                                         | `SELECT * FROM employees LIMIT 5;`                                                                                  |
| **OFFSET**    | Skips a number of rows before starting to return rows.                      | `SELECT * FROM employees OFFSET 5;`                                                                                 |
| **JOIN**      | Combines rows from multiple tables based on a condition.                    | `SELECT e.name, d.dept_name FROM employees e JOIN departments d ON e.dept_id = d.dept_id;`                          |
| **DISTINCT**  | Removes duplicate rows from the result set.                                 | `SELECT DISTINCT department FROM employees;`                                                                        |
| **WITH**      | Defines a temporary named result set (CTE) for use in a query.              | `WITH top_salaries AS (SELECT * FROM employees WHERE salary > 70000) SELECT * FROM top_salaries;`                   |
| **RETURNING** | Returns the affected rows.<br>This will return the id and created_at field. | insert into account (first_name, last_name, number) values ('suraj' , 'gorai', 213123)<br>returning id, created_at; |
|               |                                                                             |                                                                                                                     |


**where**

```
SELECT * FROM Students  
WHERE stu_fees < 3500;
```
using **AND**
```
SELECT * FROM employees
WHERE age > 30 AND department = 'HR';
```

using **OR**
```
SELECT * FROM employees
WHERE age > 30 OR department = 'HR' OR department = 'Finance';
```

using **NOT**
```
SELECT * FROM Customers  
WHERE NOT Country = 'Spain' AND NOT Country = 'US';
```

#####  IN

using **IN**
- The `IN` operator is a shorthand for multiple `OR` conditions.
```
SELECT * FROM Customers  
WHERE Country IN ('India','US')';
```
using **NOT IN**

```
SELECT * FROM Customers  
WHERE Country NOT IN ('India','US')';
```

**IN (SELECT)**
We can also use `IN` with a subquery in the `WHERE` clause.

With a subquery you can return all records from the main query that are present in the result of the subquery.
```
SELECT * FROM Customers  
WHERE CustomerID IN (SELECT CustomerID FROM Orders);
```

**NOT IN (SELECT)**

```
SELECT * FROM Customers  
WHERE CustomerID NOT IN (SELECT CustomerID FROM Orders);
```

##### IS NULL

**IS NULL**
```
SELECT _column_names  
_FROM _table_name_  
WHERE _column_name_ IS NULL;

SELECT * FROM employee
WHERE department IS NULL;
```

**IS NOT NULL**
```
SELECT * FROM employee
WHERE department IS NOT NULL;
```


**order by**
```
SELECT * FROM Students  
ORDER BY stu_fees ASC;
```


**limit and offset**

offset : skips a specified number of rows.
limit : returns fixed number of rows.
```
SELECT * FROM employees
LIMIT 5 OFFSET 10;
```


**DISTINCT :**

Only returns unique rows.
```
SELECT DISTINCT department FROM employees;
```


**group by**

