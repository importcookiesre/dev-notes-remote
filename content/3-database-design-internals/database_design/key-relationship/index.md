
[[PLAYGROUND2]]


- Used to uniquely identify a table, establish relationship between tables. 
- eg : {Email , Name , StudentsID} is a super key but Name is a redundant attribute as it is not contributing anything for the uniqueness.
- Each row should contain a unique field which is a Primary key.

## what are keys ?

- A key is an attribute or a set of attributes used to uniquely identify rows in a table.
	- eg userID is used to uniquely identify a row in a table.


## Types of keys : 

### 1. super key : 

- It is basically a list of all the ways in which attributes can be uniquely identified.
- It may be a single attribute (like userID) or group of attributes like `{course, studentId}`
- A table can have many super keys.

- **Example**: In a `Students` table with columns `(StudentID, Name, Email, PhoneNumber)`, some possible super keys are:
    
    - `{StudentID}` (since each ID is unique)
    - `{Email}` (since each email is unique)
    - `{PhoneNumber}` (assuming each student has a unique phone number)
    - `{StudentID, Name}` (This combination is also unique, even though `Name` alone isn't)
    - `{Email, Name, PhoneNumber}`

The key thing to remember is that a super key includes _all_ possible combinations that guarantee uniqueness, including redundant ones (like `{StudentID, Name}`).


### 2. Candidate Key : 

- A **minimal super key** (no redundant attributes).
- **Example**: From the super keys listed above, the candidate keys are:
    
    - `{StudentID}`
    - `{Email}`
    - `{PhoneNumber}`

The combination `{StudentID, Name}` is **not** a candidate key because `StudentID` by itself is already unique, making `Name` a redundant attribute for identification purposes.


### 3. Primary Key : 

[[1-primary-key]]
### 4. Composite Key : 
[[2-composite-key]]

### 5. Foreign Key : 


- A foreign key is a key used to link two tables together.
- It's a field in a table (child table) that refers to the primary key in another table (parent table).
- 

Example : 
- Let's say we have a second table called Enrollments with columns (EnrollmentID, CourseName, StudentID).
	- The StudentID column in the Enrollments table is a foreign key.
	- It refers to the SudentID (the primary key ) in the Students table.
	- This ensures that we cannot enroll a student that does'nt exists in the Students table.



## Example : 



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

- CREATE EXTENSION: This is the command to load an add-on module into the database.
    
- IF NOT EXISTS : This is a safety clause. It prevents an error if the extension has already been installed. If it's already there, the command does nothing; if it's not, it installs it.
    
- "uuid-ossp" :  This is the name of the extension, which stands for **O**pen **S**ource **S**oftware **P**rogramming UUID. It includes functions like `uuid_generate_v1()` (time-based) and `uuid_generate_v4()` (random).


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
[[3-foreign-key]]