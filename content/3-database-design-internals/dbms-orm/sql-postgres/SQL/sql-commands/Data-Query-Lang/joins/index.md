
- https://sql-joins.leopard.in.ua/ : visualizer

join combines the two tables and return a single one based on condition. 
- The matching data based on foreign key are placed in the same row.
- Unmatching are placed seperately with null values from other table like in left join the matching values of the left table and right table based on foreign keys are written in the same row, while the values which does not match in the left are kept as it is with null fields.



# What is a JOIN?

A **JOIN** combines rows from two or more tables based on a related column (usually via a **foreign key**).  
In relational databases, this is how we connect data that’s stored across multiple tables.

![[Pasted image 20251211084535.png]]

- If there is no match, null is returned.
![[Pasted image 20260312222547.png]]
