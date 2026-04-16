


### table management : 


#### create table : 
```sql
CREATE TABLE table_name (
    column1 datatype [constraints],
    column2 datatype [constraints],
    ...
);
```

```
CREATE TABLE employees (
    id SERIAL PRIMARY KEY,
    name TEXT NOT NULL,
    age INT,
    department TEXT
);

DROP TABLE employees;
ALTER TABLE employees ADD COLUMN salary NUMERIC;
```


####  ALTER table : 


**add a new table:**
```
ALTER TABLE _table_name_  
ADD _column_name datatype_;

ALTER TABLE Customers  
ADD Email varchar(255);
```

**drop table**

```
ALTER TABLE _table_name_  
DROP COLUMN _column_name_;

ALTER TABLE Customers  
DROP COLUMN Email;
```

**rename table:**
```
ALTER TABLE _table_name_  
RENAME COLUMN _old_name_ to _new_name_;
```

**change datatype**

```
ALTER TABLE table_name
ALTER COLUMN column_name TYPE new_data_type;

ALTER TABLE employees
ALTER COLUMN age TYPE INTEGER;

```