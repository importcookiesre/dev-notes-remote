
| **Update**         | `.updateOne(filter, update)`                  | Updates the first document matching the filter.                                              |
| ------------------ | --------------------------------------------- | -------------------------------------------------------------------------------------------- |
|                    | `.updateMany(filter, update)`                 | Updates all documents matching the filter.                                                   |
|                    | `.findByIdAndUpdate(id, update)`              | Finds a document by its `_id`, applies the update, and returns the updated document.         |



### Example : 

```ts
await user.updateOne({ refreshToken });
```