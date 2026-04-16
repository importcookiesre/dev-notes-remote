

An **INNER JOIN** or **JOIN** in SQL is used to combine rows from two or more tables based on a related column between them (common). It returns _only_ the rows where there is a match in **both** tables.

If a row in Table A does not have a matching row in Table B (or vice versa), that row is excluded from the result.
![[Pasted image 20251211084821.png]]
basically it returns an intersection of tables.


#### Example : Users and Orders table :

setup : 
```sql
CREATE TABLE Users (
    user_id INT PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    email VARCHAR(100) UNIQUE
);

INSERT INTO Users (user_id, name, email) VALUES
(1, 'Alice', 'alice@example.com'),
(2, 'Bob', 'bob@example.com'),
(3, 'Charlie', 'charlie@example.com');

CREATE TABLE Orders (
    order_id INT PRIMARY KEY,
    user_id INT NOT NULL,
    product VARCHAR(100) NOT NULL,
    quantity INT,
    FOREIGN KEY (user_id) REFERENCES Users(user_id)
);

INSERT INTO Orders (order_id, user_id, product, quantity) VALUES
(101, 1, 'Laptop', 1),
(102, 2, 'Mouse', 1),
(103, 1, 'Monitor', 1);
```


fetch user's name with their product name : 
```sql
SELECT
	U.name,
	O.product
FROM 
	Users AS U
INNER JOIN
	Orders AS O ON U.user_id = O.user_id;
```
