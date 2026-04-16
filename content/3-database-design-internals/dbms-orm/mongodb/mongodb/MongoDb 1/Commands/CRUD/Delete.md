
#### **Key Methods for Remove Operations**

|**Method**|**Description**|**Use Case**|
|---|---|---|
|`deleteOne()`|Deletes one document matching the filter|Delete a specific document without affecting others.|
|`deleteMany()`|Deletes all matching documents|Clear all documents meeting a certain condition.|
|`findOneAndDelete()`|Deletes one document and returns it|Retrieve and delete a specific document in one operation.|
|`drop()`|Deletes the entire collection|Remove all data and schema of a collection.|

**Additional Methods**

`db.collection.findOneAndDelete()`
`db.collection.findAndModify()`
`db.collection.bulkWrite()`


#### Syntax:

##### **1. `deleteOne()`**

- Deletes the first document that matches the filter.
- If no document matches, no operation is performed.
```
db.collection.deleteOne({ filterCondition })
db.users.deleteOne({ name: "John" });
```
This deletes the **first document** where the `name` is `"John"`.

##### **2. `deleteMany()`**

- Deletes all documents that match the filter criteria.
```
db.collection.deleteMany({ filterCondition })
db.users.deleteMany({ status: "inactive" });
```
This deletes **all documents** where the `status` is `"inactive"`.

##### **3. `findOneAndDelete()`**

- Finds a single document, deletes it, and returns the deleted document.
- Useful when you need to retrieve the document before deletion.
```
db.collection.findOneAndDelete({ filterCondition })
db.users.findOneAndDelete({ name: "John" });
```
This finds the first document where `name` is `"John"`, deletes it, and returns it.

##### **4. `drop()`**

- Completely removes a collection from the database, including all its documents and metadata.
```
db.collection.drop()
db.users.drop();
```
This removes the entire `users` collection from the database.


####  **Options for Remove Operations**

| **Option**     | **Description**                                                                       |
| -------------- | ------------------------------------------------------------------------------------- |
| `writeConcern` | Specifies the level of acknowledgment required from MongoDB for the remove operation. |
| `collation`    | Specifies language-specific rules for string comparison.                              |
```
db.users.deleteOne(
    { name: "JOHN" },
    { collation: { locale: "en", strength: 2 } }
);
```


#### **Combining Filters for Precise Deletion**
You can combine filters with logical and comparison operators to delete specific documents.
```
db.users.deleteMany({
    $and: [
        { age: { $gte: 25 } },
        { city: "New York" }
    ]
});

```
This deletes all users who are **25 or older** and live in **New York**.