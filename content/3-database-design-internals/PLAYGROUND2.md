



## 1. constraints : 


| Constraint    | Description                                                                                                       |
| ------------- | ----------------------------------------------------------------------------------------------------------------- |
| `NOT NULL`    | Ensures a column cannot be null.                                                                                  |
| `PRIMARY KEY` | Uniquely identifies each row; combines `NOT NULL` and `UNIQUE`.                                                   |
| `FOREIGN KEY` | Ensures data integrity by referencing another table.                                                              |
| `UNIQUE`      | Ensures all values in a column are distinct.                                                                      |
| `CHECK`       | Validates data according to a condition.                                                                          |
| `DEFAULT`     | Provides a default value if none is given.                                                                        |
| `EXCLUSION`   | Ensures no two rows meet a specific condition combination.                                                        |
| `INDEX`       | Optimizes data retrieval by indexing columns. - Automatically created for `PRIMARY KEY` and `UNIQUE` constraints. |

### ques





## 2. Keys : 


### 1. Primary key and Composite Key: 

- SERIAL & BIGSERIAL autoincrement value.
- It is recommanded to use UUID for serialization.
	- we can genereate UUID using uuid_generate_v4().

#### 1. Define a primary key during table creation.

```sql
CREATE TABLE users (
    user_id SERIAL PRIMARY KEY,
    username VARCHAR(50) NOT NULL,
    email VARCHAR(100) UNIQUE NOT NULL
);

-- use uuid instead of SERIAL.
CREATE EXTENSION IF NOT EXISTS "uuid-ossp";

CREATE TABLE customers (
    customer_id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
    name TEXT NOT NULL
);

```

`CREATE EXTENSION IF NOT EXISTS "uuid-ossp";`

This command instructs the database to **load a pre-packaged collection of functions** called `"uuid-ossp"`. This specific extension provides functions for generating UUIDs (Universally Unique Identifiers), which are globally unique 128-bit numbers.

- **`CREATE EXTENSION`**: This is the command to load an add-on module into the database.
    
- **`IF NOT EXISTS`**: This is a safety clause. It prevents an error if the extension has already been installed. If it's already there, the command does nothing; if it's not, it installs it.
    
- **`"uuid-ossp"`**: This is the name of the extension, which stands for **O**pen **S**ource **S**oftware **P**rogramming UUID. It includes functions like `uuid_generate_v1()` (time-based) and `uuid_generate_v4()` (random).


####  2. with custom name

```sql
CREATE TABLE products (
    product_id INT,
    product_name TEXT,
    CONSTRAINT pk_products PRIMARY KEY (product_id)
);
```

#### 3. composite primary key (multiple columns)
```sql
CREATE TABLE enrollment (
    student_id INT,
    course_id INT,
    PRIMARY KEY (student_id, course_id)
);

-- this is a wrong syntax (multiple primary keys are not allowed)
CREATE TABLE enrollment (
    student_id INT PRIMARY KEY,
    course_id INT PRIMARY KEY,
);
```

#### 4. Add and remove Primary key on existing table:
```sql
ALTER TABLE employees
ADD CONSTRAINT employees_pk PRIMARY KEY (emp_id);

-- remove
ALTER TABLE users DROP CONSTRAINT users_pkey;

```



### 2. Foreign key : 

- A foreign key is a key used to link two tables together.
- It's a field in a table that refers to the primary key in another table.

The fundamental rule of foreign keys is that they must point from the **dependent table (Child)** to the **independent table (Parent)**.
- If there is user table and user_profile, the user_profile table should point to user table and not user table to user_profile even though it looks like violating the composition principles.


##### 1. **Users and Orders (E-commerce)**

- Every `order` must belong to a valid `user`.
- Prevents inserting an order with `user_id = 999` if that user doesn’t exist.
```sql
CREATE TABLE users (
    user_id SERIAL PRIMARY KEY,
    username VARCHAR(50) NOT NULL
);

CREATE TABLE orders (
    order_id SERIAL PRIMARY KEY,
    user_id INT NOT NULL,
    FOREIGN KEY (user_id) REFERENCES users(user_id)
);
```

##### 2. on Delete  : CASCADE , RESTRICT, set NULL

- If a `customer` is deleted, all their `invoices` are also deleted.
- Used in **banking, CRM, or subscription systems**.
```sql
CREATE TABLE customers (
    customer_id SERIAL PRIMARY KEY,
    name TEXT NOT NULL
);

CREATE TABLE invoices (
    invoice_id SERIAL PRIMARY KEY,
    customer_id INT,
    FOREIGN KEY (customer_id) REFERENCES customers(customer_id)
        ON DELETE CASCADE
);
```


- You cannot delete a `category` if products still reference it.
- Useful in **product catalogs** where you don’t want to delete categories still in use.
```sql
CREATE TABLE categories (
    category_id SERIAL PRIMARY KEY,
    name TEXT NOT NULL
);

CREATE TABLE products (
    product_id SERIAL PRIMARY KEY,
    category_id INT,
    FOREIGN KEY (category_id) REFERENCES categories(category_id)
        ON DELETE RESTRICT
);
```

- If an employee leaves, `emp_id` in `projects` becomes NULL.
- Good for **HR systems**: projects can exist even if the employee is gone.
```sql
CREATE TABLE employees (
    emp_id SERIAL PRIMARY KEY,
    name TEXT
);

CREATE TABLE projects (
    project_id SERIAL PRIMARY KEY,
    emp_id INT,
    FOREIGN KEY (emp_id) REFERENCES employees(emp_id)
        ON DELETE SET NULL
);
```

##### 3. add / remove on existing tables : 
```sql
-- Child table (no foreign key yet)
CREATE TABLE orders (
    order_id SERIAL PRIMARY KEY,
    user_id INT
);

-- Now add foreign key
ALTER TABLE orders
ADD CONSTRAINT fk_orders_users
FOREIGN KEY (user_id) REFERENCES users(user_id)
ON DELETE CASCADE;
```

```sql
ALTER TABLE orders DROP CONSTRAINT fk_orders_users;
```
##### 4. many to many relationship :

- Students can enroll in multiple courses.
- Foreign keys enforce **valid enrollments only**.
- Common in **education systems (LMS)**.
```sql
CREATE TABLE students (
    student_id SERIAL PRIMARY KEY,
    name TEXT
);

CREATE TABLE courses (
    course_id SERIAL PRIMARY KEY,
    title TEXT
);

CREATE TABLE enrollment (
    student_id INT,
    course_id INT,
    PRIMARY KEY (student_id, course_id),
    FOREIGN KEY (student_id) REFERENCES students(student_id),
    FOREIGN KEY (course_id) REFERENCES courses(course_id)
);
```


##### 5. composite foreign key : 

```sql
CREATE TABLE classes (
    class_id INT,
    section_id INT,
    PRIMARY KEY (class_id, section_id)
);

CREATE TABLE class_schedules (
    class_id INT,
    section_id INT,
    day TEXT,
    FOREIGN KEY (class_id, section_id)
        REFERENCES classes(class_id, section_id)
);
```

## 3. relationship

- A **relationship** defines how two or more tables are logically connected.
- In **ER Model** we draw relationships with diamonds, but in PostgreSQL we implement them using **foreign keys**.




### 1. one-to-one

👉 Each row in **Table A** relates to **exactly one row** in **Table B**.
- here user_id attribute in user_profiles ensure that there will be only one user_profile row in the table therefore a one-to-one relationship.
```sql
CREATE TABLE users (
    user_id SERIAL PRIMARY KEY,
    username VARCHAR(50) UNIQUE NOT NULL
);

CREATE TABLE user_profiles (
    profile_id SERIAL PRIMARY KEY,
    user_id INT UNIQUE,  -- ensures 1-to-1
    bio TEXT,
    FOREIGN KEY (user_id) REFERENCES users(user_id) ON DELETE CASCADE
);
```
✅ A user has exactly one profile.  
💡 Used in: separating **optional details** (profile, settings, addresses) from main entity.


### 2. one-to-many

- 👉 A row in **Table A** can relate to **multiple rows** in **Table B**.
- here in the orders table the  customer_id does not have any unique field so there can be N numbers of orders associated with customers.

customer & orders
```sql
CREATE TABLE customers (
    customer_id SERIAL PRIMARY KEY,
    name TEXT NOT NULL
);

CREATE TABLE orders (
    order_id SERIAL PRIMARY KEY,
    customer_id INT, -- ensures one to many relationship (as many orders can have same customer_id)
    order_date TIMESTAMP DEFAULT NOW(),
    FOREIGN KEY (customer_id) REFERENCES customers(customer_id) ON DELETE CASCADE
);
```


### 3. many-to-many relationship

👉 Rows in **Table A** can relate to multiple rows in **Table B**, and vice versa.  
👉 Implemented via a **junction (bridge) table**.
```sql
CREATE TABLE students (
    student_id SERIAL PRIMARY KEY,
    name TEXT
);

CREATE TABLE courses (
    course_id SERIAL PRIMARY KEY,
    title TEXT
);

-- Junction table
CREATE TABLE enrollments (
    student_id INT,
    course_id INT,
    PRIMARY KEY (student_id, course_id),
    FOREIGN KEY (student_id) REFERENCES students(student_id) ON DELETE CASCADE,
    FOREIGN KEY (course_id) REFERENCES courses(course_id) ON DELETE CASCADE
);
```



## 4. JOIN


A **JOIN** combines rows from two or more tables based on a related column (usually via a **foreign key**).  
In relational databases, this is how we connect data that’s stored across multiple tables.