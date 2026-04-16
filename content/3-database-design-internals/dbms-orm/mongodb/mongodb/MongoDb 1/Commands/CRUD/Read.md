

#### Read Commands

| **Command**                                                                | **Description**                                     |
| -------------------------------------------------------------------------- | --------------------------------------------------- |
| `db.<collection_name>.find()`<br>                                          | Retrieve all documents in a collection.             |
| `db.<collection_name>.find({ key: "value" })`                              | Retrieve documents that match a condition.          |
| `db.<collection_name>.findOne({key : "value"})`                            | Retrieve the first document that match a conditions |
| `db.<collection_name>.find({ key: "value" }, { field: 1 })`                | Retrieve specific fields from documents.            |
| `db.<collection_name>.countDocuments()`,<br>`db.<collection_name>.count()` | Count the number of documents in a collection.      |

##### `db.<collection_name>.find({ key: "value" }, { field: 1 })`

- It performs a query to retrieve documents from a collection which contains only the fields that are explicitly mentioned in the second parameter.
- Only `_id` can be excluded by passing `0` in the parameter.

######  Example:
**`user` collection**
```
[
  { "_id": 1, "name": "Alice", "age": 25, "city": "New York" },
  { "_id": 2, "name": "Bob", "age": 30, "city": "Los Angeles" },
  { "_id": 3, "name": "Charlie", "age": 35, "city": "New York" }
]
```
**Running the command**
```
db.users.find({ city: "New York" }, { name: 1 })
```
**Result**
```
[
  { "_id": 1, "name": "Alice" },
  { "_id": 3, "name": "Charlie" }
]
```



#### Comparison Operators:
- MongoDB provides several comparison operators that can be used in queries to compare values in documents. These operators are often used with the `find()` method and aggregation queries.

##### **1. `$eq` (Equal)**
- The `$eq` operator is used to match documents where the value of a field is equal to the specified value.
```
db.collection.find({ field: { $eq: value } })
db.users.find({ age: { $eq: 25 } })
```
This query finds all users where the `age` is equal to 25.

##### **2. `$ne` (Not Equal)**
The `$ne` operator is used to match documents where the value of a field is not equal to the specified value.
```
db.collection.find({ field: { $ne: value } })
db.users.find({ age: { $ne: 25 } })
```
This query finds all users where the `age` is **not** equal to 25.

##### **3. `$gt` (Greater Than)**
The `$gt` operator is used to match documents where the value of a field is greater than the specified value.
```
db.collection.find({ field: { $gt: value } })
db.users.find({ age: { $gt: 25 } })
```
This query finds all users where the `age` is greater than 25.

##### **4. `$gte` (Greater Than or Equal)**
The `$gte` operator is used to match documents where the value of a field is greater than or equal to the specified value.
```
db.collection.find({ field: { $gte: value } })
db.users.find({ age: { $gte: 25 } })
```
This query finds all users where the `age` is greater than or equal to 25.

##### **5. `$lt` (Less Than)**
The `$lt` operator is used to match documents where the value of a field is less than the specified value.
```
db.collection.find({ field: { $lt: value } })
db.users.find({ age: { $lt: 25 } })
```
This query finds all users where the `age` is less than 25.

##### **6. `$lte` (Less Than or Equal)**
The `$lte` operator is used to match documents where the value of a field is less than or equal to the specified value.
```
db.collection.find({ field: { $lte: value } })
db.users.find({ age: { $lte: 25 } })
```
This query finds all users where the `age` is less than or equal to 25.

##### **7. `$in` (In Array)**
The `$in` operator is used to match documents where the value of a field is in an array of specified values.
```
db.collection.find({ field: { $in: [value1, value2, ...] } })
db.users.find({ age: { $in: [25, 30, 35] } })
```
This query finds all users where the `age` is either 25, 30, or 35.

##### **8. `$nin` (Not In Array)**
The `$nin` operator is used to match documents where the value of a field is not in an array of specified values.
```
db.collection.find({ field: { $nin: [value1, value2, ...] } })
db.users.find({ age: { $nin: [25, 30, 35] } })
```
This query finds all users where the `age` is not 25, 30, or 35.

##### **9. `$exists` (Field Existence)**
The `$exists` operator is used to match documents that contain (or do not contain) a field.
- **`true`**: Field exists
- **`false`**: Field does not exist
```
db.collection.find({ field: { $exists: true/false } })
db.users.find({ address: { $exists: true } })
```
This query finds all users that have an `address` field.

##### **10. `$type` (Field Type)**
The `$type` operator is used to match documents where the field is of a specific BSON type.
```
db.collection.find({ field: { $type: <bson_type> } })
db.users.find({ age: { $type: "int" } })
```
This query finds all users where the `age` field is of the integer type.

##### **11. `$regex` (Regular Expression)**
The `$regex` operator is used to match documents where the value of a field matches a specified regular expression pattern.
```
db.collection.find({ field: { $regex: /pattern/ } })
db.users.find({ name: { $regex: /^J/ } })
```
This query finds all users where the `name` field starts with "J".


#### Cursors:
- Cursors in mongoDb are used to efficiently retrieve large result data from search queries, providing control over data retrieval process.
- MongoDb retrieves query results using batches using cursors.
- Cursors are a pointer to the result set on the server.
- Cursors are used to iterate through query results.


##### 1. count(): 
- Returns the number of documents in the cursor.
- use countDocuments() as count is depreciated.
```
let count = db.users.find({ age: { $gt: 25 } }).count();
print("Number of users older than 25:", count);
```

##### 2. limit(n):
- Limits the number of documents returned by the cursor to `n`.
```
db.users.find().limit(5)
```

##### 3. skip(n):
- Skips the first `n` documents in the cursor and returns the rest.
```
db.users.find().skip(5)
```

##### 4 `sort(1 / -1)`
- Sorts the documents in the cursor based on the specified criteria.
- Use `1` for ascending order and `-1` for descending order.
```
db.users.find().sort({ age: 1 })
```

##### 5. `batchSize(size)`
- Specifies the number of documents to return in each batch.
- Reduces the amount of data sent over the network in one go.
```
db.users.find().batchSize(10);
```

##### 6. explain()
- Provides details on how MongoDB executed the query, useful for debugging and optimisation.
```
db.users.find({ age: { $gt: 25 } }).explain();
```

##### 7. `close()`

- Explicitly closes the cursor. MongoDB automatically closes cursors when the operation is complete, but you can close them manually if needed.
```
let cursor = db.users.find();
cursor.close();
```

##### 8. `next()`

- Retrieves the next document in the cursor.
- Throws an error if there are no more documents.
```
let cursor = db.users.find();
while (cursor.hasNext()) {
    printjson(cursor.next());
}
```

##### 9. `hasNext()`

- Returns `true` if there are more documents to iterate over; otherwise, `false`.
```
let cursor = db.users.find();
if (cursor.hasNext()) {
    print("Documents are available.");
}
```

##### 10. `toArray()`

- Converts all documents in the cursor into an array.
- Useful when you want to process all results at once.
```
let documents = db.users.find().toArray();
printjson(documents);
```

##### 11. `forEach()`

- Iterates over each document in the cursor and applies a specified function.
```
db.users.find().forEach(doc => printjson(doc));
```


##### **Combining Methods**
You can chain multiple cursor methods to filter, sort, and limit the results.
```
db.users.find({ age: { $gte: 25 } })
       .sort({ age: -1 })
       .limit(5)
       .forEach(doc => printjson(doc));
```
#### Logical Operators:

- Logical operators in MongoDB allow you to build complex queries by combining multiple conditions. These operators work with the `find()` method, aggregation pipelines, and other query structures.

##### **1. `$and`**

The `$and` operator is used to match documents that satisfy **all** the specified conditions.
```
db.collection.find({ $and: [ { condition1 }, { condition2 }, ... ] })

db.users.find({
    $and: [
        { age: { $gte: 25 } },
        { city: "New York" }
    ]
});

db.inventory.find( { status: "A", qty: { $lt: 30 } } ) 
// this will also perform 'and' operation
```
This query finds users who are **25 years or older** and live in **New York**.

##### **2. `$or`**

The `$or` operator is used to match documents that satisfy **at least one** of the specified conditions.
```
db.collection.find({ $or: [ { condition1 }, { condition2 }, ... ] })

db.users.find({
    $or: [
        { age: { $lt: 25 } },
        { city: "New York" }
    ]
});
```
This query finds users who are **younger than 25** or live in **New York**.

##### **3. `$nor`**

The `$nor` operator is used to match documents that **do not satisfy any** of the specified conditions.
```
db.collection.find({ $nor: [ { condition1 }, { condition2 }, ... ] })

db.users.find({
    $nor: [
        { age: { $gte: 25 } },
        { city: "New York" }
    ]
});
```
This query finds users who are **not 25 or older** and **do not live in New York**.


##### 4. `$not`

The `$not` operator is used to negate a condition, matching documents that **do not satisfy** the given condition.
```
db.collection.find({ field: { $not: { condition } } })

db.users.find({
    age: { $not: { $gte: 25 } }
});
```
This query finds users whose `age` is **not greater than or equal to 25** (i.e., less than 25).

##### **Combining Logical Operators**
Logical operators can be nested or combined with other query operators to create complex conditions.
```
db.users.find({
    $and: [
        { $or: [ { age: { $gte: 25 } }, { city: "New York" } ] },
        { status: "active" }
    ]
});

db.inventory.find( {
     status: "A",
     $or: [ { qty: { $lt: 30 } }, { item: /^p/ } ]
} )
```
This query finds users who are either **25 or older** or **live in New York**, and whose `status` is **active**.


| **Operator** | **Description**                                                                |
| ------------ | ------------------------------------------------------------------------------ |
| `$and`       | Matches documents that satisfy **all conditions**.                             |
| `$or`        | Matches documents that satisfy **at least one condition**.                     |
| `$nor`       | Matches documents that **do not satisfy any condition**.                       |
| `$not`       | Negates a condition, matching documents that **do not satisfy the condition**. |

#### some more methods

1. `db.collection.distinct("field")` : Returns all the distinct values of the field
   ` db.collection.distinct("category")` : Returns all unique categories  