


- A **many-to-many (M:N)** relationship occurs when **multiple records in one table are related to multiple records in another table**.

	- Relational databases **cannot directly represent** many-to-many relationships.
	- Ex: Student and Class table: 
		- A Student can take multipe classes.
		- A class can have multiple Students.
		- Here we cant say which is parent and child.
	- To handle this, we use an **intermediate (junction or bridge) table** that breaks M:N into **two one-to-many** relationships.

![[Pasted image 20260316065417.png]]
this relationship is then usually implemented using a **join table**
![[Pasted image 20260316065448.png]]


👉 Implemented via a **junction (bridge) table**.
[[3-database-design-internals/database_design/fundamentals/1-junction-table/index]]

1. Create the main tables :
```python
CREATE TABLE Students (
    student_id SERIAL PRIMARY KEY,
    name VARCHAR(100) NOT NULL
);

CREATE TABLE Courses (
    course_id SERIAL PRIMARY KEY,
    title VARCHAR(100) NOT NULL
);
```

2.  Create the Junction table :
```python
CREATE TABLE Enrollments (
    enrollment_id SERIAL PRIMARY KEY,
    student_id INT NOT NULL,
    course_id INT NOT NULL,
    enrolled_at TIMESTAMP DEFAULT NOW(),

    -- enforce referential integrity
    FOREIGN KEY (student_id) REFERENCES Students(student_id) ON DELETE CASCADE,
    FOREIGN KEY (course_id) REFERENCES Courses(course_id) ON DELETE CASCADE,

    -- prevent duplicate enrollments
    UNIQUE (student_id, course_id)
);
```


3. How it works :
```python
INSERT INTO Students (name) VALUES ('Alice'), ('Bob');
INSERT INTO Courses (title) VALUES ('Math'), ('Science');

-- Link students and courses
INSERT INTO Enrollments (student_id, course_id) VALUES
(1, 1),  -- Alice → Math
(1, 2),  -- Alice → Science
(2, 1);  -- Bob → Math
```


- `FOREIGN KEY` ensures referential integrity.
- `ON DELETE CASCADE` ensures cleanup if a student or course is deleted.
- `UNIQUE (student_id, course_id)` prevents the same student from enrolling twice in the same course.


Query Example : 

1. Find all courses taken by Alice : 

```python
SELECT c.title
FROM Courses c
JOIN Enrollments e ON c.course_id = e.course_id
JOIN Students s ON e.student_id = s.student_id
WHERE s.name = 'Alice';
```

2. Find all students in Math:

```python
SELECT s.name
FROM Students s
JOIN Enrollments e ON s.student_id = e.student_id
JOIN Courses c ON e.course_id = c.course_id
WHERE c.title = 'Math';
```