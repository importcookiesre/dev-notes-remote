

- This chapter will focus on SELECT statement.


## Setup : 

- We will create a Employee table with name, age, department, and salary columns.


```sql
CREATE TABLE emp(
	id SERIAL PRIMARY KEY,
	name VARCHAR(255) NOT NULL,
	age INT NOT NULL,
	department VARCHAR(255),
	salary INT
)
```

fill data : 
```sql 
INSERT INTO emp (name, age, salary, department)
VALUES
  ('Amit Sharma', 28, 60000, 'Engineering'),
  ('Neha Verma', 32, 75000, 'HR'),
  ('Rohit Gupta', 26, 50000, 'Marketing'),
  ('Priya Singh', 35, 90000, 'Finance'),
  ('Ankit Patel', 29, 65000, 'Engineering'),
  ('Sneha Iyer', 31, 72000, 'Operations'),
  ('Vikram Rao', 40, 120000, 'Management'),
  ('Kiran Mehta', 27, 55000, 'Sales'),
  ('Pooja Nair', 34, 85000, 'Finance'),
  ('Arjun Das', 25, 48000, 'Support');
```



## Problem 1

