
- If the table name is Capatalized then to encolse it within douple inverted commans  access it.
	- `Select * from "User";`

## what is DDl ?

- These are the commands used to define and manage database schema objects like tables, indexex, views, sequences, schemas and constraints.
- Core DDL commands are :
	- CREATE - create objects like tables, viwes, indexes, schemas.
	- ALTER - Modify structure of existing objects.
	- DROP - Remove objects permanently.
	- TRUNCATE - Remove all rows from a table instantly without logging row by row.
	- COMMENT - Add metadata descriptions.
	- RENAME - Change the name of database objects.

## Table management : 

```sql
\dt                              # Display all tables
\dn                              # List all schemas
\d employees                     # Show table definition
```


## COMMENT

- attach metadata for documentation.

```sql
COMMENT ON TABLE users IS 'Stores application user accounts';
COMMENT ON COLUMN users.username IS 'Unique login name';
```
