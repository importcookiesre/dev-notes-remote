

### mongodb shell : 

- Automatically includes an **id** field.
- All or nothing behavior can be controlled with `ordered: false`

| **Command**                                                               | **Description**                              |
| ------------------------------------------------------------------------- | -------------------------------------------- |
| `db.<collection_name>.insertOne({ key: "value" })`                        | Insert one document into a collection.       |
| `db.<collection_name>.insertMany([{ key: "value" }, { key2: "value2" }])` | Insert multiple documents into a collection. |

**examples**
```bash
db.users.insertOne({
  name: "Alice",
  email: "alice@example.com",
  age: 25,
  createdAt: new Date()
});

db.users.insertMany([
  { name: "Bob", email: "bob@example.com", age: 30 },
  { name: "Carol", email: "carol@example.com", age: 28 }
]);
```

#### 🧠 Insert Options

| Option             | Description                   | Default    |
| ------------------ | ----------------------------- | ---------- |
| `ordered: boolean` | If true, stops on first error | `true`     |
| `writeConcern`     | Controls durability of write  | `{ w: 1 }` |
##### Ordered and Unordered Inserts:
- **Ordered Insert** (default):
    
    - Operations are executed in the given order.
    - If any operation fails, the entire batch stops, and no further operations are executed.
    - Useful when order matters and all operations must succeed.
- **Unordered Insert**:
    
    - Operations are executed out of order, in parallel.
    - If one operation fails, MongoDB continues with the remaining operations.
    - Ideal for large batches where performance is prioritised, and errors can be ignored for some documents.
- **Syntax**:
```
db.collection.insertMany([
  { _id: 1, name: "Alice" },
  { _id: 2, name: "Bob" },
  { _id: 3, name: "Charlie" }
], { ordered: false });
```

```bash
db.users.insertMany(
  [...],
  { ordered: false, writeConcern: { w: "majority" } }
);
```

##### **Additional methods to create documents.**


```
`db.collection.updateOne()` when used with the upsert: true option.
`db.collection.updateMany()` when used with the upsert: true option.
`db.collection.findAndModify()` when used with the upsert: true option.
`db.collection.findOneAndUpdate()` when used with the upsert: true option.
`db.collection.findOneAndReplace()` when used with the upsert: true option.
`db.collection.bulkWrite()`.
```

When `upsert: true` is specified, the operation will insert a new document if no existing document matches the filter criteria.





### mongoose : 


```ts
await UserModel.create({
  name: "Alice",
  email: "alice@example.com",
  age: 25
});
```




### prisma : 


