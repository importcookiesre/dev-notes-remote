


An aggregate function is a function that performs a calculation on a set of values, and returns a single value.

Aggregate functions are often used with the `GROUP BY` clause of the `SELECT` statement. The `GROUP BY` clause splits the result-set into groups of values and the aggregate function can be used to return a single value for each group.

The most commonly used SQL aggregate functions are:

- `MIN()` - returns the smallest value within the selected column
- `MAX()` - returns the largest value within the selected column
- `COUNT()` - returns the number of rows in a set
- `SUM()` - returns the total sum of a numerical column
- `AVG()` - returns the average value of a numerical column

Aggregate functions ignore null values (except for `COUNT()`).



#### MIN() and MAX()

```
SELECT MIN(_column_name_)  
FROM _table_name_  
WHERE _condition_;

SELECT MAX(_column_name_)  
FROM _table_name_  
WHERE _condition_;
```

**example**:
```
SELECT MIN(salary) FROM employee
WHERE department = 'HR';

SELECT MAX(salary) FROM employee
WHERE department = 'HR';
```

**set column name (alias)**

```
SELECT MIN(salary) AS minSalary FROM employee
WHERE department = 'HR';

SELECT MAX(salary) AS maxSalary FROM employee
WHERE department = 'HR';
```


#### COUNT()

```
SELECT COUNT(*)  
FROM Products;

SELECT COUNT(column_name))  
FROM Products
WHERE condition;
```

```
SELECT count(*) FROM employee
WHERE salary >90000;
```

