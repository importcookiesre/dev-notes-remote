


![[Pasted image 20251211094735.png]]
A **RIGHT JOIN** (or `RIGHT OUTER JOIN`) is the exact mirror image of a `LEFT JOIN`. It retrieves **all** rows from the **right table** and the matching rows from the **left table**.

If there is no match in the left table, the columns from the left table will contain `NULL`.

#### **Why is it less common?**

In professional engineering (including Golang backends), `RIGHT JOIN` is somewhat rare. Most developers prefer to simply swap the order of the tables and use a `LEFT JOIN`, as reading "Left to Right" (Primary -> Secondary) is generally more intuitive for English speakers. However, understanding it is crucial for maintaining legacy SQL queries.


#### Example : 

- use the same setup in Left Join

- problem : Return all tasks with their assignee
```sql
SELECT t.description , t.status, e.name,e.department
FROM tasks as t
right join employees as e
on t.assignee_id = e.emp_id;
```

#### Use cases : => same as Left join:
