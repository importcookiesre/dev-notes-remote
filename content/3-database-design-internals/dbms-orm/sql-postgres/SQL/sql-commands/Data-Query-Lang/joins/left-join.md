

## 2. LEFT JOIN / LEFT OUTER JOIN

![[Pasted image 20251211091824.png]]
A **LEFT JOIN** (also known as `LEFT OUTER JOIN`) is a type of SQL join that retrieves **all** rows from the **left table** and the matching rows from the **right table**.

If there is no match in the right table, the columns from the right table will contain `NULL` values for those non-matching rows.


#### Example : 

setup : 
```sql
CREATE TABLE Employees (
    emp_id INT PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    department VARCHAR(50)
);

-- Insert Data
INSERT INTO Employees (emp_id, name, department) VALUES
(101, 'Sarah', 'DevOps'),
(102, 'Mike', 'Frontend'),
(103, 'Lisa', 'HR'),
(104, 'Ben', 'Marketing'); -- Ben has no corresponding task


CREATE TABLE Tasks (
    task_id INT PRIMARY KEY,
    assignee_id INT NOT NULL, -- Links to Employees.emp_id
    description VARCHAR(255) NOT NULL,
    status VARCHAR(50),
    FOREIGN KEY (assignee_id) REFERENCES Employees(emp_id)
);

-- Insert Data
INSERT INTO Tasks (task_id, assignee_id, description, status) VALUES
(2001, 101, 'Deploy new API', 'Completed'),
(2002, 102, 'Design new landing page', 'In Progress'),
(2003, 101, 'Update config files', 'In Progress'),
(2004, 103, 'Schedule onboarding session', 'Pending');
```

query : 

- we want the list of every employee and show any tasks assigned to them. If employee and no task we still want to see their name.
```sql
SELECT
    E.name,
    E.department,
    T.task_id,
    T.description
FROM
    Employees AS E  -- The LEFT table (All rows kept)
LEFT JOIN
    Tasks AS T ON E.emp_id = T.assignee_id
ORDER BY
    E.name;
```

#### use cases : 

1. Find all parents/masters regardless of children/details:
	1. tables : 
		1. parent - Employee
		2. child - department
	2. list all employee with their department, if anyone is not assigned with a department it should still list them with null value.
2. Find missing / null rows :
	1. tables : 
		1. parent - Employee
		2. child - department
	2. find all employee whose departments are not assigned.
```sql
SELECT e.name
FROM Employees AS e
LEFT JOIN Tasks AS T ON e.emp_id = t.assignee_id
WHERE t.task_id IS NULL; -- Filters to only show the rows where the right side failed to match
```


| **Use Case**                           | **Description**                                                                                                                                                                               | **SQL Logic**                                                                                                  |
| -------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------- |
| **Finding Non-Matches (Missing Data)** | Identify records in the main table that are missing associated data in the secondary table.                                                                                                   | Use a `LEFT JOIN` and then filter for `WHERE RightTable.ID IS NULL`. (e.g., Finding users with **no orders**). |
| **User Profile Completion**            | Listing all users and showing whether they have filled out optional profile details (like a biography or shipping address).                                                                   | List all users on the left; join the optional details table on the right.                                      |
| **Inventory Management**               | Listing all products and showing their current stock level from a separate `Inventory` table, even if the stock entry hasn't been created yet (i.e., stock is assumed to be zero or unknown). | List all products on the left; join inventory on the right.                                                    |
| **Tracking Unanswered Items**          | Listing all customer support tickets and showing any associated replies, including those tickets that have received **no replies** yet.                                                       | List all tickets on the left; join replies on the right.                                                       |

