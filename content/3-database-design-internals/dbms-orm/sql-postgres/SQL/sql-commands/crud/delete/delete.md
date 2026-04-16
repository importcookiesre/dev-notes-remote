


```sql
-- Delete a specific row
DELETE FROM employees
WHERE name = 'Charlie';

-- Delete all rows
DELETE FROM employees;

-- Truncate the table (faster, resets SERIAL ID)
TRUNCATE TABLE employees RESTART IDENTITY; -- id restarts from 1

```


## reset the sequence after deleting everything using delete: 

1. find the sequence associated with the column
	1. should return something like : `nextval('employees_id_seq'::regclass)`
2. alter sequence.
3. It can be also used to start the id from a specific point.
```sql
SELECT column_default
FROM information_schema.columns
WHERE table_schema = 'public' -- Or your specific schema
  AND table_name = 'employees'
  AND column_name = 'id';
--output:  nextval('employees_id_seq'::regclass)

ALTER SEQUENCE employees_id_seq RESTART WITH 1;
```