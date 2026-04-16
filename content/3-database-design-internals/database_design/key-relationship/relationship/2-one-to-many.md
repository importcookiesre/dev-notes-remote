

- 👉 A row in **Table A** can relate to **multiple rows** in **Table B**.
	- eg. A user can have multiple comments but a comment can have just one user (origin). 
	- eg, A User table and Credit Card Table. A user can own many credit card and a credit card is owned by one user. Here user is a independent entity and credit card is a dependent entity. (credit card cannot exist without a user). 
		- We establish a one to many relationship from a Credit Card to a User ie, a credit is having a field called user_id which references to the id of the user. 
	- eg. A user table and review table : 
		- A user can make one review to a product that he has bought.
		- He can review to multiple products.
		- Here id of user has f_key with user_id field in review.
		- We can make user_id field as unique because there will be multiple reviews with the same user for different products.
		- Review table will contain a product_id filed that has f_key with id field of the product.
		- Here (user_id, product_id) in review table is unique. (composite key)
- here in the orders table the  customer_id does not have any unique field so there can be N numbers of orders associated with customers.

customer & orders
```sql
CREATE TABLE customers (
    customer_id SERIAL PRIMARY KEY,
    name TEXT NOT NULL
);

CREATE TABLE orders (
    order_id SERIAL PRIMARY KEY,
    customer_id INT, -- ensures one to many relationship (as many orders can have same customer_id)
    order_date TIMESTAMP DEFAULT NOW(),
    FOREIGN KEY (customer_id) REFERENCES customers(customer_id) ON DELETE CASCADE
);
```
![[Pasted image 20260316065229.png]]