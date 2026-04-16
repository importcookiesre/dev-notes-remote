
A **junction table** (also known as a bridge table, join table, or intersection table) is ==used to manage and resolve **many-to-many relationships** between two or more other tables==.


## Why junction tables exist : 

Relational databases cannot directly represent **many-to-many relationships**.
Example:
- A **student** can enroll in **many courses**
- A **course** can have **many students**

If we tried putting this directly in either table, it would break normalization.

So we introduce a **third table**.


```go
students
--------
id
name

courses
--------
id
title

student_courses   <-- junction table
----------------
student_id
course_id
```
Each row means:
> student X is enrolled in course Y
- student 1 enrolled in course 101
- student 1 enrolled in course 102
- student 2 enrolled in course 101

```go
student_courses
----------------
student_id | course_id
1          | 101
1          | 102
2          | 101
```

This converts `many-to-many` relationship to `one-to-many` and `many-to-one` relationship.

```go
CREATE TABLE users (
    id SERIAL PRIMARY KEY,
    name TEXT
);

CREATE TABLE bootcamps (
    id SERIAL PRIMARY KEY,
    title TEXT
);

CREATE TABLE bootcamp_users (
    user_id INT REFERENCES users(id),
    bootcamp_id INT REFERENCES bootcamps(id),
    role TEXT,
    PRIMARY KEY (user_id, bootcamp_id)
);
```
- same user cannot join same bootcamp twice
### 1. Junction Tables Can Have Extra Data : 

A junction table **does not have to contain only foreign keys**.
It can contain **relationship metadata**.


```go
student_courses
---------------
student_id
course_id
enrolled_at
progress
grade
```


## ER diagram : 

Student course relationship : many to many 
![[Pasted image 20260316064522.png]]



![[Pasted image 20260316064543.png]]