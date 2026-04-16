
- A **composite key** (or compound key) is a primary key that consists of **two or more** attributes to uniquely identify a record. This is used when a single column is not enough to guarantee uniqueness.

- **Example**: In our `Enrollments` table, a student could enroll in multiple courses. So, neither `StudentID` nor `CourseName` alone can be the primary key. However, the combination of `(StudentID, CourseName)` would be unique (assuming a student can't enroll in the exact same course twice). Therefore, `{StudentID, CourseName}` could serve as a composite primary key for the `Enrollments` table.


- The attributes should be NOT NULL.
```sql
CREATE TABLE enrollment (
	student_id uuid NOT NULL,
	course_id uuid NOT NULL,
	PRIMARY KEY (student_id, course_id)
);
```

```sql
CREATE TABLE team_members (
    team_id INTEGER NOT NULL,
    person_id INTEGER NOT NULL,
    PRIMARY KEY (team_id, person_id)
);
```

create Primary key in existing table : 

```sql
-- first ensure that the attributes are NOT NULL
ALTER TABLE my_table
ALTER COLUMN column_a SET NOT NULL,
ALTER COLUMN column_b SET NOT NULL;

-- add the PRIMARY KEY CONSTRAINT
ALTER TABLE my_table
ADD CONSTRAINT my_table_pkey PRIMARY KEY (column_a, column_b);
```