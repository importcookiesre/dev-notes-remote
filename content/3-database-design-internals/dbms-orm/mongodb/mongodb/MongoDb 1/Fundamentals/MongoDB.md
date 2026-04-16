#### What is MongoDB ?
- MongoDB is an open-source, document-oriented,  NoSQL database management system.
- Unlike traditional relational databases that use tables, rows, and columns, MongoDB stores data in a document-oriented format, making it more suitable for handling diverse and dynamic data structures.
- Designed for flexibility, scalability, and performance in handling unstructured or semi-structured data.


#### Key features of mongoDb.

- **Document-Oriented Storage**
    
    - Data is stored in JSON-like documents called BSON (Binary JSON).
    - Documents are schema-flexible, allowing fields to vary between documents in a collection.
- **Collections**
    
    - Documents are grouped into collections, which are analogous to tables in SQL.
    - Collections do not enforce a fixed schema, enabling developers to adapt to changes quickly.
- **Scalability and Performance**
    
    - MongoDB supports horizontal scaling through sharding, distributing data across multiple servers.
    - It uses in-memory storage for high-speed read and write operations.
- **Indexing**
    
    - Efficient querying is enabled by various types of indexes, including single-field, compound, and geospatial indexes.
- **High Availability**
    
    - MongoDB provides built-in replication for fault tolerance.
    - Replica sets ensure automatic failover and data redundancy.
- **Aggregation Framework**
    
    - Enables complex data processing and transformation using an expressive pipeline of stages like `$match`, `$group`, `$project`, and `$sort`.
- **Integration with Programming Languages**
    
    - MongoDB has official drivers for languages such as Node.js, Python, Java, C#, and more, allowing seamless integration with applications.
- **MongoDB Atlas**
    
    - A fully managed cloud database platform, offering automated backups, monitoring, and scaling.


#### **Common Use Cases**

1. **E-Commerce Applications**: For managing products, orders, and customer data.
2. **Real-Time Analytics**: For processing large amounts of unstructured or semi-structured data.
3. **Content Management Systems (CMS)**: For storing diverse content types with varying schemas.
4. **IoT Applications**: For collecting and analysing sensor data.


#### Terminologies 
![[Pasted image 20250112092900.png]]

**Database --> Collections --> Documents**


#### JSON vs BSON

- In MongoDB, we write in JSON format only but behind the scene data is stored in BSON (Binary JSON) format, a binary representation of JSON.
- By utilising BSON, MongoDB can achieve higher read and write speeds, reduced storage requirements, and improved data manipulation capabilities, making it well-suited for handling large and complex datasets while maintaining performance efficiency. 

