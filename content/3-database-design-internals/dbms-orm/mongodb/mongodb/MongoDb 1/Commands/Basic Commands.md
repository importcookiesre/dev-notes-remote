 
#### Rules:
- syntax are case sensitive.
- 

#### **0. Server Management**

| **Command**                    | **Purpose**                         |
| ------------------------------ | ----------------------------------- |
| `sudo systemctl start mongod`  | Start mongoDb server.               |
| `sudo systemctl status mongod` | Check the status of mongoDb server. |
| `sudo systemctl stop mongod`   | Stop mongoDb server.                |



#### **1. Database Management Commands**

| **Command**           | **Description**                |
| --------------------- | ------------------------------ |
| `show dbs`            | List all databases.            |
| `use <database_name>` | Switch to a specific database. |
| `db`                  | Show the current database.     |
| `db.dropDatabase()`   | Drop the current database.     |
#### **2. Collection Management Commands**

| **Command**                                | **Description**                               |
| ------------------------------------------ | --------------------------------------------- |
| `show collections`                         | List all collections in the current database. |
| `db.createCollection("<collection_name>")` | Create a new collection.                      |
| `db.<collection_name>.drop()`              | Drop a specific collection.                   |


### **4. Index Management Commands**

| **Command**                                      | **Description**                                                  |
| ------------------------------------------------ | ---------------------------------------------------------------- |
| `db.<collection_name>.createIndex({ key: 1 })`   | Create an index on a field (1 for ascending, -1 for descending). |
| `db.<collection_name>.getIndexes()`              | List all indexes in a collection.                                |
| `db.<collection_name>.dropIndex("<index_name>")` | Drop a specific index.                                           |

---

### **5. Aggregation Commands**

|**Command**|**Description**|
|---|---|
|`db.<collection_name>.aggregate([{ $match: { key: "value" } }])`|Filter data using aggregation.|
|`db.<collection_name>.aggregate([{ $group: { _id: "$key", total: { $sum: 1 } } }])`|Group data and calculate aggregates like sums or counts.|

---

### **6. User Management Commands**

|**Command**|**Description**|
|---|---|
|`db.createUser({ user: "username", pwd: "password", roles: [{ role: "readWrite", db: "db" }] })`|Create a new user with specific roles.|
|`db.getUsers()`|List all users in the current database.|
|`db.dropUser("username")`|Delete a user.|

---

### **7. Administrative Commands**

|**Command**|**Description**|
|---|---|
|`db.serverStatus()`|Display server status information.|
|`db.stats()`|Show statistics about the current database.|
|`db.currentOp()`|View currently running operations on the server.|
|`db.killOp(<operation_id>)`|Terminate a specific operation.|

---

### **8. Backup and Restore Commands**

|**Command**|**Description**|
|---|---|
|`mongodump --db=<database_name> --out=<backup_folder>`|Backup a database.|
|`mongorestore --db=<database_name> <backup_folder>`|Restore a database from a backup.|

---

### **9. Export and Import Commands**

| **Command**                                                                                | **Description**                                 |
| ------------------------------------------------------------------------------------------ | ----------------------------------------------- |
| `mongoexport --db=<database_name> --collection=<collection_name> --out=<output_file.json>` | Export data from a collection to a JSON file.   |
| `mongoimport --db=<database_name> --collection=<collection_name> --file=<input_file.json>` | Import data into a collection from a JSON file. |