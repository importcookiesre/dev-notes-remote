
```sql
-- Select all columns
SELECT * FROM employees;

-- Select specific columns
SELECT name, department FROM employees;

-- With conditions
SELECT * FROM employees WHERE age > 30;

-- With sorting
SELECT * FROM employees ORDER BY salary DESC;

-- With limit
SELECT * FROM employees LIMIT 5;
```





**where**

```
SELECT * FROM Students WHERE stu_fees < 3500;
SELECT * FROM Students WHERE department  is NULL;

```

##### AND, OR and NOT : 

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


##### BETWEEN : 

```
SELECT * FROM Products  
WHERE Price BETWEEN 10 AND 20;
// both 10 and 20 are included.
```

**not between**
```
SELECT * FROM Products  
WHERE Price NOT BETWEEN 10 AND 20;
```

**BETWEEN and IN**
```
SELECT * FROM Products  
WHERE Price BETWEEN 10 AND 20  
AND CategoryID IN (1,2,3);
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


##### order by
```
SELECT * FROM Students  
ORDER BY stu_fees ASC;

-- desc : 
SELECT * FROM Students  
ORDER BY stu_fees DESC;
```


##### limit and offset

offset : skips a specified number of rows.
limit : returns fixed number of rows.
```
SELECT * FROM employees
LIMIT 5 OFFSET 10;
```


##### DISTINCT :

Only returns unique rows.

```
SELECT DISTINCT department FROM employees;
```
- returns the list of department.
##### dfdf