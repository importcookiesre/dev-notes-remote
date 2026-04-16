

- A foreign key is a key used to link two tables together.
- It's a field in a table that refers to the **primary key** in another table.

The fundamental rule of foreign keys is that they must point from the **dependent table (Child)** to the **independent table (Parent)**.
- If there is user table and user_profile, the user_profile table should point to user table and not user table to user_profile even though it looks like violating the composition principles.
![[foreign-keys-example.svg]]

 
## Example 1. **Users and Orders (E-commerce)**

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

## Foreign Keys constraints : 

### 1. on Delete  : CASCADE , RESTRICT, set NULL 

- On Delete is a constraint on a foreign key
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
    FOREIGN KEY (customer_id) REFERENCES customers(customer_id)  ON DELETE CASCADE
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


### 2. ON UPDATE : CASCADE 


## 3. add / remove foreign key from existing tables : 

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
## 4. many to many relationship :

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


## 5. composite foreign key : 

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
