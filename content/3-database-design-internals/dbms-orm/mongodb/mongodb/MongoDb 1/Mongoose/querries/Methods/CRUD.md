


Mongoose [models](https://mongoosejs.com/docs/models.html) provide several static helper functions for [CRUD operations](https://en.wikipedia.org/wiki/Create,_read,_update_and_delete). Each of these functions returns a [mongoose `Query` object](https://mongoosejs.com/docs/api/query.html#Query).

#### Methods:

| **CRUD Operation** | **Method**                                    | **Description**                                                                              |
| ------------------ | --------------------------------------------- | -------------------------------------------------------------------------------------------- |
| **Create**         | `.create(data , ...list)`                     | Inserts  new documents into the collection.                                                  |
|                    | `new Model(data).save()`                      | Creates a new document and saves it to the collection.                                       |
| **Read**           | `.find(filter)`                               | Retrieves all documents matching the filter.                                                 |
|                    | `.findOne(filter)`                            | Retrieves the first document matching the filter.                                            |
|                    | `.findById(id)`                               | Retrieves a document by its `_id`.                                                           |
|                    | `.findOneAndUpdate(filter, update)`           | Finds a document, applies an update, and returns the document (can be used for updates too). |
| **Update**         | `.updateOne(filter, update)`                  | Updates the first document matching the filter.                                              |
|                    | `.updateMany(filter, update)`                 | Updates all documents matching the filter.                                                   |
|                    | `.findByIdAndUpdate(id, update)`              | Finds a document by its `_id`, applies the update, and returns the updated document.         |
| **Delete**         | `.deleteOne(filter)` or<br>`item.deleteOne()` | Deletes the first document matching the filter.                                              |
|                    | `.deleteMany(filter)`                         | Deletes all documents matching the filter.                                                   |
|                    | `.findByIdAndDelete(id)`                      | Finds a document by its `_id` and deletes it.                                                |

#### Queries :
The following are different Queries:

- **Model.deleteMany():** This query takes the parameters of any field that matches and then deletes all the entries in the database that matches.
- **Model.deleteOne():**  This query takes the parameters of any field that matches and then deletes any one of the entries in the database that matches.
- **Model.find():** This query takes the parameters of one or more fields that match and then returns all the entries in the database that matches.
- **Model.findById()**: This query takes the id as the parameter and then returns the entry in the database if it exists matches.
- **Model.findByIdAndDelete()**: This query takes the id as the parameter and then deletes the entry in the database if it exists matches.
- **Model.findByIdAndRemove()**: This query takes the id as the parameter and then removes the entry in the database if it exists and then returns it to the callback function.
- **Model.findByIdAndUpdate()**: This query takes the id and the update parameters and values as the parameter and then updates the entry in the database if it exists.
- **Model.findOne():** This query takes the parameters of any field that matches and then returns any one of the entries in the database that matches.
- **Model.findOneAndDelete()**: This query takes the parameters of any field that matches and then returns and deletes any one of the entries in the database that matches.
- **Model.findOneAndRemove()**: This query takes the parameters of any field that matches and then returns and removes any one of the entries in the database that matches.
- **Model.findOneAndReplace()**: This query takes the parameters of any field and the replace document and then replaces any one of the entries in the database that matches.
- **Model.findOneAndUpdate()**: This query takes the parameters of one or more fields and the updated fields and values and then updates any one of the entries in the database that matches.
- **Model.replaceOne()**: This query takes the parameters as a filter and the replacement document and then replaces any one of the entries in the database that matches.
- **Model.updateMany()**: This query takes the parameters as a filter and the updating fields and values and then updates all of the entries in the database that matches.
- **Model.updateOne()**: This query takes the parameters as a filter and the updating fields and values and then updates any one of the entries in the database that matches.


---
