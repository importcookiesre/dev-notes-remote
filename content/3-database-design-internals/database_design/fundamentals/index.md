

## 1. Data Integrity

- It assures data is accurate, consistent and reliable throughout its lifecycle.
- It ensures that database does not have : 
	- duplicate records : eg multiple address fields in different tables pointing to the same entity.
	- missing or invalid data  
	- contradictory information : 

- Types of data integrity :
	- Entity integrity : 
		- Ensures that every row in a table is uniquely identified.
		- Achieved by Primary Key.
	- Referential Integrity : 
		- Ensures relationships between tables remain consistent.
		- Achieved by Foreign Key.

```python
CREATE TABLE Orders (
    orderId SERIAL PRIMARY KEY,
    userId INT NOT NULL,
    orderDate TIMESTAMP DEFAULT NOW(),
    FOREIGN KEY (userId) REFERENCES Users(id)
);
```
- order cannor exist without userId.

	- Domain Integrity : 
		- Ensures data in a column follows a defined dataype and range.
		- Achieved by data types, constraints and rules.

```python
CREATE TABLE Products (
    productId SERIAL PRIMARY KEY,
    name VARCHAR(50) NOT NULL,
    price NUMERIC CHECK(price > 0)
);
```
- price must be greater than 0.
- name cannot be null.



## 2. Atomicity :

- Keep things at atomic level or dont store array in a field ie, one field should contain one value.
	- FullName - > first_name, middle_name, last_name
	- Phone Numebe -> Country code, number