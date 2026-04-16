
- use single inverted 'commas' instead of "double" inverted commans.
	- `update users set username = 'suraj', email = 'suraj@gmail.comm' where username = 'surajgoraicse';`
- use `RETURNING` to fetch the updated/ deleted rows.
```sql
DELETE FROM users
WHERE id = 3
RETURNING *;

INSERT INTO users (username, email)
VALUES ('charlie', 'charlie@example.com')
RETURNING id, username;
```


### 🔹 4. **TCL (Transaction Control Language)**

Used to **manage transactions** in the database.

- **BEGIN / START TRANSACTION** → start a transaction block.
    
- **COMMIT** → save changes permanently.
    
- **ROLLBACK** → undo changes since last commit.
    
- **SAVEPOINT** → set a checkpoint inside a transaction.
    
- **RELEASE SAVEPOINT** → remove a savepoint.[[_Golang/BACKEND-GO/DATABASE/dbms_postgres/SQL-postgres/sql-commands/Data-Query-Lang/joins/index]]
    

📌 Example:

`BEGIN; UPDATE users SET email = 'bob@example.com' WHERE id = 2; ROLLBACK; -- undo change`

---

### 🔹 5. **DCL (Data Control Language)**

Used for **access control and permissions**.

- **GRANT** → give privileges.
    
- **REVOKE** → remove privileges.
    

📌 Example:

`GRANT SELECT, INSERT ON users TO app_user;`

---

| Command Type                  | Purpose                 | Examples                              |
| ----------------------------- | ----------------------- | ------------------------------------- |
| **DQL** (Data Query)          | Retrieve data           | `SELECT`                              |
| **DDL** (Data Definition)     | Define/modify structure | `CREATE`, `ALTER`, `DROP`, `TRUNCATE` |
| **DML** (Data Manipulation)   | Modify table data       | `INSERT`, `UPDATE`, `DELETE`          |
| **DCL** (Data Control)        | Manage permissions      | `GRANT`, `REVOKE`                     |
| **TCL** (Transaction Control) | Handle transactions     | `COMMIT`, `ROLLBACK`, `SAVEPOINT`     |



### keys : 

Keys in SQL are attributes (columns) used to **identify records uniquely** and establish relationships between tables.

| **Key Type**      | **Description**                                                                                                             | **Example**                                                           |
| ----------------- | --------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------- |
| **Primary Key**   | Uniquely identifies each row in a table. Cannot be NULL or duplicate. Each table can only have one primary key.             | `id INT PRIMARY KEY`                                                  |
| **Foreign Key**   | Creates a relationship between two tables by referencing a Primary Key in another table. There can be multiple foreign keys | `FOREIGN KEY (dept_id) REFERENCES departments(id)`                    |
| **Candidate Key** | A set of columns that can serve as a Primary Key. Only one is chosen as the Primary Key.                                    | `emp_id` and `email` can both uniquely identify employees.            |
| **Super Key**     | A superset of Candidate Keys that uniquely identifies a row. May contain extra attributes.                                  | `emp_id, email, phone_number` together form a Super Key.              |
| **Composite Key** | A key made up of two or more columns to ensure uniqueness.                                                                  | `ORDER_ID + PRODUCT_ID` as a Composite Key in an order details table. |
| **Alternate Key** | A Candidate Key that is **not** chosen as the Primary Key.                                                                  | If `emp_id` is the Primary Key, then `email` is an Alternate Key.     |
| **Unique Key**    | Ensures uniqueness of a column's values but allows one NULL value.                                                          | `email VARCHAR(255) UNIQUE`                                           |

#### Primary key : 

🔹 **Ensures each row is uniquely identified**  
🔹 **Cannot be NULL or duplicate**  
🔹 **Each table can have only ONE Primary Key**

```sql
CREATE TABLE employees (
    id INT PRIMARY KEY,
    name VARCHAR(100),
    email VARCHAR(255) UNIQUE
);
```
**Multiple Column (Composite) Primary Key**

- There may be duplicate order_id or product_id but the **Combination of** `order_id` + `product_id` **must be unique**.
- 
```sql
CREATE TABLE order_details (
    order_id INT,
    product_id INT,
    quantity INT,
    PRIMARY KEY (order_id, product_id)
);
```


#### Foreign key : 

A **Foreign Key (FK)** is a column or a set of columns in one table that refers to the **Primary Key (PK)** in another table. It **establishes a relationship** between the two tables and ensures **referential integrity** (i.e., a child table cannot reference a non-existent parent record).

```sql
CREATE TABLE employees (
    emp_id INT PRIMARY KEY,
    name VARCHAR(100)
);

CREATE TABLE departments (
    dept_id INT PRIMARY KEY,
    dept_name VARCHAR(100)
);

CREATE TABLE emp_department (
    emp_id INT,
    dept_id INT,
    FOREIGN KEY (emp_id) REFERENCES employees(emp_id),
    FOREIGN KEY (dept_id) REFERENCES departments(dept_id)
);
```


 
### **What are Constraints?**

SQL **constraints** are rules applied to table columns to **ensure data integrity, consistency, and accuracy**. They restrict the type of data that can be stored in a table.

| **Constraint**     | **Description**                                                             | **Example**                                         |
| ------------------ | --------------------------------------------------------------------------- | --------------------------------------------------- |
| **NOT NULL**       | Ensures that a column cannot have `NULL` values.                            | `name VARCHAR(100) NOT NULL;`                       |
| **UNIQUE**         | Ensures all values in a column are distinct.                                | `email VARCHAR(255) UNIQUE;`                        |
| **PRIMARY KEY**    | Uniquely identifies each row in a table (combination of NOT NULL + UNIQUE). | `id INT PRIMARY KEY;`                               |
| **FOREIGN KEY**    | Links two tables by referencing a Primary Key from another table.           | `FOREIGN KEY (dept_id) REFERENCES departments(id);` |
| **CHECK**          | Ensures values in a column meet a specified ==condition==.                  | `age INT CHECK (age >= 18);`                        |
| **DEFAULT**        | Assigns a default value if no value is provided.                            | `status VARCHAR(10) DEFAULT 'active';`              |
| **AUTO_INCREMENT** | Automatically generates unique numbers for each row.                        | `id INT PRIMARY KEY AUTO_INCREMENT;`                |
|                    |                                                                             |                                                     |

```sql
CREATE TABLE users (
    id INT PRIMARY KEY AUTO_INCREMENT,
    username VARCHAR(50) UNIQUE,
    email VARCHAR(255) UNIQUE NOT NULL,
    status VARCHAR(100) DEFAULT "active",
    age INT CHECK(age >= 18)
);
```

