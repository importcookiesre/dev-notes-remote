

## 1. naming convection : 

1. postgres converts uppercase to lowercase if they are not in double quotes :
	1. Product -> product  : both are same for postgres
	2. "Product" -> "Product"
2. single quotes : used for string literals
3. double quotes : for identifiers ie name of database, table, columns 
	1. generally used when uppercase literal is used.



## 1. file system vs database : 


File systems organize data in a hierarchical structure of files and folders. While simple and intuitive for basic data storage, this approach presents several significant challenges in a data-intensive environment:

- **Data Redundancy and Inconsistency:** In a file system, the same piece of information might be stored in multiple files. For instance, a customer's address could exist in a sales file, a marketing file, and a customer service file. When this address needs to be updated, it must be changed in every location. Failure to do so leads to data inconsistency, where different files hold conflicting information.
    
- **Limited Data Sharing and Security:** Sharing data across different applications is cumbersome and often requires custom programming. Furthermore, security features in file systems are generally basic, often limited to read, write, and execute permissions at the file or directory level. This makes it difficult to implement granular access control, where different users have varying levels of access to the same data.
    
- **Concurrent Access Issues:** When multiple users or applications attempt to access and modify the same file simultaneously, the file system offers little to no mechanism to manage these concurrent updates. This can lead to data corruption and inconsistent states. Imagine two users trying to book the last seat on a flight; without proper concurrency control, both might end up with a confirmed booking.
    
- **Lack of Data Integrity and Atomicity:** File systems do not enforce data integrity constraints. For example, there is no built-in way to ensure that a product ID entered into a sales record actually corresponds to an existing product. Additionally, file systems lack transaction management. A transaction is a sequence of operations that must be completed in an "all-or-nothing" manner. If a system crashes mid-transaction (e.g., during a fund transfer), the data can be left in an inconsistent state.
    
- **Difficult Data Retrieval:** Retrieving specific information from a large number of files can be slow and inefficient. It often requires writing custom programs to parse and filter the data. There is no standardized way to query the data across different file formats.





### What is a database ?

> A **database** is an organized collection of data that is stored, managed, and accessed electronically. It allows users to store large amounts of information efficiently and retrieve it quickly when needed. eg. SQL, NoSQL or relational db.

### What is DBMS ?

> A **DBMS (Database Management System)** is software that helps users create, manage, and interact with databases efficiently. It acts as an interface between the database and users or applications, ensuring that data is stored securely, retrieved quickly, and managed effectively.
> eg. PostgresSql, MongoDb, MySQL


### What is SQL ?

> SQL is a standard programming language used to manage and manipulate relational databases. It allows users to store, retrieve, update, and delete data efficiently.








#### aliases

- SQL aliases are used to give a table, or a column in a table, a temporary name.
- An alias only exists for the duration of that query.
- An alias is created with the `AS` keyword.
- Allow creating alias without `AS` keyword.

```sql
SELECT r.name
FROM roles r
JOIN user_roles ur ON ur.role_id = r.id
WHERE ur.user_id = 1;

FROM orders o
JOIN order_items oi
JOIN products p
JOIN users u
```
