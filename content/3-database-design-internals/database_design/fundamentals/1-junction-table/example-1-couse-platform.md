

# 1. student, course and enrollement table : 


## 1. Create Table : 

```sql
CREATE TABLE students (
    id uuid PRIMARY KEY DEFAULT uuidv7(),
    name VARCHAR(50) NOT NULL
);

CREATE TABLE course (
    id uuid PRIMARY KEY DEFAULT uuidv7(),
    subject VARCHAR(50)
);

CREATE TABLE enrollment (
    id uuid PRIMARY KEY DEFAULT uuidv7(),
    student_id uuid REFERENCES students(id) ON DELETE CASCADE NOT NULL,
    course_id uuid REFERENCES course(id) ON DELETE CASCADE NOT NULL,
    created_at timestamptz DEFAULT now(),
    UNIQUE(student_id, course_id)
);
```


## 2. Insert Students : 

```sql
INSERT INTO students (name) VALUES
('Suraj'),
('Aman'),
('Riya'),
('Neha'),
('Rahul');

INSERT INTO course (subject) VALUES
('PostgreSQL'),
('Distributed Systems'),
('Operating Systems'),
('Computer Networks'),
('Algorithms');


INSERT INTO enrollment (student_id, course_id)
VALUES
((SELECT id FROM students WHERE name='Suraj'), (SELECT id FROM course WHERE subject='PostgreSQL')),
((SELECT id FROM students WHERE name='Suraj'), (SELECT id FROM course WHERE subject='Algorithms')),

((SELECT id FROM students WHERE name='Aman'), (SELECT id FROM course WHERE subject='PostgreSQL')),
((SELECT id FROM students WHERE name='Aman'), (SELECT id FROM course WHERE subject='Distributed Systems')),

((SELECT id FROM students WHERE name='Riya'), (SELECT id FROM course WHERE subject='Operating Systems')),
((SELECT id FROM students WHERE name='Riya'), (SELECT id FROM course WHERE subject='PostgreSQL')),

((SELECT id FROM students WHERE name='Neha'), (SELECT id FROM course WHERE subject='Algorithms')),

((SELECT id FROM students WHERE name='Rahul'), (SELECT id FROM course WHERE subject='Computer Networks'));
```


## 3. Questions : 

#### Q. 1 : List all the students with their courses : 

```sql
SELECT
    s.name AS student,
    c.subject AS course
FROM enrollment e
JOIN students s ON s.id = e.student_id
JOIN course c ON c.id = e.course_id;
```

#### Q. 2 : Find all courses taken by **Suraj**

```sql
SELECT c.subject
FROM enrollment e
JOIN students s ON s.id = e.student_id
JOIN course c ON c.id = e.course_id
WHERE s.name = 'Suraj';
```

#### Q. 3 Count how many courses each student takes.

```sql
SELECT
    s.name,
    COUNT(e.course_id) AS total_courses
FROM students s
LEFT JOIN enrollment e ON s.id = e.student_id
GROUP BY s.name;
```


#### Q4. Find the course with the **most students**


```sql
SELECT
    c.subject,
    COUNT(e.student_id) AS total_students
FROM course c
JOIN enrollment e ON c.id = e.course_id
GROUP BY c.subject
ORDER BY total_students DESC
LIMIT 1;
```

#### Q5. Find students who **are not enrolled in any course**

```sql
SELECT s.name
FROM students s
LEFT JOIN enrollment e ON s.id = e.student_id
WHERE e.student_id IS NULL;
```


#### Q6. Find courses that **no student has taken**

```sql
SELECT c.subject
FROM course c
LEFT JOIN enrollment e ON c.id = e.course_id
WHERE e.course_id IS NULL;
```

#### Q7. Find students taking **more than one course**

```sql
SELECT
    s.name,
    COUNT(e.course_id) AS course_count
FROM students s
JOIN enrollment e ON s.id = e.student_id
GROUP BY s.name
HAVING COUNT(e.course_id) > 1;
```

#### Q8. List students and courses ordered by enrollment time

```sql
SELECT
    s.name,
    c.subject,
    e.created_at
FROM enrollment e
JOIN students s ON s.id = e.student_id
JOIN course c ON c.id = e.course_id
ORDER BY e.created_at DESC;
```


#### Q9. Find number of students per course.

```sql
SELECT
    c.subject,
    COUNT(e.student_id) AS students
FROM course c
LEFT JOIN enrollment e ON c.id = e.course_id
GROUP BY c.subject
ORDER BY students DESC;
```


#### Q10. Find students enrolled in **PostgreSQL**

```sql
SELECT s.name
FROM students s
JOIN enrollment e ON s.id = e.student_id
JOIN course c ON c.id = e.course_id
WHERE c.subject = 'PostgreSQL';
```

