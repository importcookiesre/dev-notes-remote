
#### [SchemaType Options](https://mongoosejs.com/docs/schematypes.html#schematype-options)


| **Option**  | **Description**                                                         | **Example**                                                                                                                |
| ----------- | ----------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------- |
| `type`      | Specifies the data type of the field.                                   | `name: { type: String }`                                                                                                   |
| `required`  | Ensures the field is mandatory.                                         | `age: { type: Number, required: true }`                                                                                    |
| `default`   | Sets a default value if none is provided.                               | `isActive: { type: Boolean, default: true }`                                                                               |
| `unique`    | Ensures unique values across documents (enforced by MongoDB).           | `email: { type: String, unique: true }`                                                                                    |
| `enum`      | Restricts the field to specified values (useful for strings).           | `role: { type: String, enum: ['admin', 'user', 'guest'] }`                                                                 |
| `validate`  | Adds custom validation logic.                                           | `username: { type: String, validate: { validator: v => /^[a-z]+$/.test(v), message: 'Only lowercase letters allowed!' } }` |
| `match`     | Validates the field against a regular expression (useful for strings).  | `email: { type: String, match: /^[\w.-]+@[\w.-]+\.[a-z]{2,4}$/ }`                                                          |
| `minlength` | Sets the minimum length for strings.                                    | `name: { type: String, minlength: 3 }`                                                                                     |
| `maxlength` | Sets the maximum length for strings.                                    | `name: { type: String, maxlength: 50 }`                                                                                    |
| `min`       | Sets the minimum value for numbers or dates.                            | `age: { type: Number, min: 18 }`                                                                                           |
| `max`       | Sets the maximum value for numbers or dates.                            | `age: { type: Number, max: 65 }`                                                                                           |
| `trim`      | Automatically trims whitespace from strings.                            | `name: { type: String, trim: true }`                                                                                       |
| `lowercase` | Converts strings to lowercase before saving.                            | `email: { type: String, lowercase: true }`                                                                                 |
| `uppercase` | Converts strings to uppercase before saving.                            | `code: { type: String, uppercase: true }`                                                                                  |
| `immutable` | Prevents the field from being modified after the document is created.   | `createdAt: { type: Date, default: Date.now, immutable: true }`                                                            |
| `select`    | Excludes the field from query results by default.                       | `password: { type: String, select: false }`                                                                                |
| `get`       | Defines a custom function to modify the value when reading it.          | `price: { type: Number, get: v => (v / 100).toFixed(2) }`                                                                  |
| `set`       | Defines a custom function to modify the value when saving it.           | `price: { type: Number, set: v => v * 100 }`                                                                               |
| `alias`     | Creates an alias for easier access to the field.                        | `firstName: { type: String, alias: 'fName' }`                                                                              |
| `index`     | Creates an index on the field for faster queries.                       | `email: { type: String, index: true }`                                                                                     |
| `sparse`    | Creates a sparse index, indexing only documents where the field exists. | `email: { type: String, sparse: true }`                                                                                    |

