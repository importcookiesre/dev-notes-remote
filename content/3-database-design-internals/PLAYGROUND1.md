

## 1. DDL

#### ques 1. 

- Create a table `students` with columns: `id` (integer, primary key, auto-increment), `name` (text, not null), `age` (integer), and `enrolled_at` (date).
- Alter the `students` table to add a column `email` with a unique constraint.
- Drop the column `age` from the `students` table.
- Create a schema called `school` and move the `students` table into it.

##### soln : 

```sql
-- Step 1: Create the students table
CREATE TABLE students (
    id SERIAL PRIMARY KEY,
    name TEXT NOT NULL,
    age INT,
    enrolled_at DATE 
);

-- Step 2: Alter the students table to add a unique email column
ALTER TABLE students ADD COLUMN email VARCHAR(255) UNIQUE;
-- A VARCHAR(255) is a common and flexible choice for email lengths.

-- Step 3: Drop the age column from the students table
ALTER TABLE students DROP COLUMN age;

-- Step 4: Create a schema named 'school'
CREATE SCHEMA school;

-- Step 5: Move the students table into the 'school' schema
ALTER TABLE students SET SCHEMA school;
  
```


#### ques 2. 

- Create a table `courses` with `id`, `title`, and `credits`. Add a constraint that ensures `credits` must be between 1 and 6.
- Add a foreign key from `students` to `courses` such that each student is enrolled in one course.
- Rename the `courses` table to `subjects`.
- Create an index on the `email` column of the `students` table to speed up lookups.
- Write a statement to truncate the `students` table but keep the structure intact.

##### soln


```sql
CREATE TABLE courses (
	id SERIAL PRIMARY KEY,
	title VARCHAR(255),
	credits INT CHECK (credits >= 1 and credits <= 6)
);
```



#### ques 3. 

- Create a partitioned table `orders` by range on `order_date`, with partitions for 2023 and 2024.
- Add a generated column `name_length` to the `students` table that always stores the length of `name`.
- Define a check constraint on `students` to ensure `email` contains `@`.
- Create an event trigger that logs whenever a new table is created in the database.
- Create a sequence `invoice_seq` that starts at 1000 and increments by 5. Attach it to a column in a new table `invoices`.
- Drop the `school` schema and ensure that all dependent objects are also dropped in a single command.


##### soln

