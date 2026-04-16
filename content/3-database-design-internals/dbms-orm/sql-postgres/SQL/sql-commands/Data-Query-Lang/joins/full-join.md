




## 4. FULL OUTER JOIN : 

![[Pasted image 20251211100637.png]]

A **FULL OUTER JOIN** returns **all** rows from both tables.

It combines the results of both a `LEFT JOIN` and a `RIGHT JOIN`.
- If rows match, they are combined.
- If a row in the **Left** table has no match, the Right columns are `NULL`.
- If a row in the **Right** table has no match, the Left columns are `NULL`.

Think of this as the union of two circles in a Venn diagram. You get everything: the intersection, the unique left parts, and the unique right parts.



### **Example Setup: Employees and Parking Spots**

We want to audit our parking situation.

1. Some employees have assigned parking spots.
2. Some employees take the bus (no spot).
3. Some parking spots are currently empty (no employee).

We need a single list that shows **all** employees and **all** spots, regardless of whether they are linked.

setup : 

```sql
CREATE TABLE Employees (
    emp_id INT PRIMARY KEY,
    name VARCHAR(100)
);

INSERT INTO Employees (emp_id, name) VALUES
(1, 'Alice'),   -- Has a spot
(2, 'Bob'),     -- Has a spot
(3, 'Charlie'); -- No spot (Takes bus)


CREATE TABLE ParkingSpots (
    spot_id INT PRIMARY KEY,
    spot_number VARCHAR(10),
    emp_id INT -- Can be NULL if spot is empty
);

INSERT INTO ParkingSpots (spot_id, spot_number, emp_id) VALUES
(101, 'A-1', 1),    -- Assigned to Alice
(102, 'A-2', 2),    -- Assigned to Bob
(103, 'B-1', NULL); -- Empty Spot
```


```sql
SELECT
    E.name,
    P.spot_number
FROM
    Employees AS E
FULL OUTER JOIN
    ParkingSpots AS P ON E.emp_id = P.emp_id;
```

result :

| **name** | **spot_number** | **Explanation**                                        |
| -------- | --------------- | ------------------------------------------------------ |
| Alice    | A-1             | **Match**: Employee has a spot.                        |
| Bob      | A-2             | **Match**: Employee has a spot.                        |
| Charlie  | **NULL**        | **Left Only**: Employee exists, but no spot found.     |
| **NULL** | B-1             | **Right Only**: Spot exists, but no employee assigned. |
|          |                 |                                                        |
|          |                 |                                                        |


### use cases : 


- **Data Synchronization/Auditing**: When you are migrating data between two systems (e.g., "Old Database" vs. "New Database") and want to see:
    - Which records transferred perfectly (Inner).
    - Which records are missing in the new DB (Left).
    - Which records appeared in the new DB but weren't in the old one (Right).
    - _A Full Join gives you all three in one query._
        
- **Exception Reporting**: Finding all "orphans" on both sides at once.
    - Query: "Show me employees without badges **AND** badges without employees."
        
- **Merging Monthly Reports**: If you have `January_Sales` and `February_Sales` and want a list of _all_ customers who bought something in _either_ month, aligning their purchases side-by-side.


