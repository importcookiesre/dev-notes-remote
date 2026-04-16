
$pull operator : The pull operator removes from an existing array all instances of a value or values that match a specified condition.

### **Operators in MongoDB**

MongoDB operators are special keywords used in queries and update statements to perform specific operations. They allow you to filter, manipulate, and modify documents efficiently. MongoDB operators are grouped into categories based on their functionality.

---

### **Categories of Operators**

|**Category**|**Description**|
|---|---|
|**Comparison Operators**|Compare values in fields with specified criteria.|
|**Logical Operators**|Combine multiple query conditions.|
|**Element Operators**|Check for the existence or type of fields.|
|**Evaluation Operators**|Perform evaluations like regular expressions and JavaScript.|
|**Array Operators**|Work with arrays and their elements.|
|**Bitwise Operators**|Perform bitwise operations on numeric fields.|
|**Projection Operators**|Specify fields to include or exclude in query results.|
|**Update Operators**|Modify field values in documents.|
|**Aggregation Operators**|Perform data aggregation in pipelines.|
|**Geospatial Operators**|Work with geospatial data like points and shapes.|

---

### **1. Comparison Operators**

Used to filter documents based on conditions.

|**Operator**|**Description**|**Example**|
|---|---|---|
|`$eq`|Matches values equal to a specified value.|`{ age: { $eq: 25 } }`|
|`$ne`|Matches values not equal to a value.|`{ age: { $ne: 25 } }`|
|`$gt`|Matches values greater than a value.|`{ age: { $gt: 30 } }`|
|`$gte`|Matches values greater than or equal to.|`{ age: { $gte: 30 } }`|
|`$lt`|Matches values less than a value.|`{ age: { $lt: 20 } }`|
|`$lte`|Matches values less than or equal to.|`{ age: { $lte: 20 } }`|
|`$in`|Matches any value in an array.|`{ city: { $in: ["New York", "Los Angeles"] } }`|
|`$nin`|Matches values not in an array.|`{ city: { $nin: ["Boston", "Miami"] } }`|

---

### **2. Logical Operators**

Used to combine multiple conditions.

|**Operator**|**Description**|**Example**|
|---|---|---|
|`$and`|Matches documents satisfying all conditions.|`{ $and: [{ age: { $gte: 25 } }, { city: "New York" }] }`|
|`$or`|Matches documents satisfying any condition.|`{ $or: [{ age: { $lt: 20 } }, { city: "Chicago" }] }`|
|`$not`|Negates a condition.|`{ age: { $not: { $gte: 25 } } }`|
|`$nor`|Matches documents satisfying no conditions.|`{ $nor: [{ age: { $lt: 20 } }, { city: "Miami" }] }`|

---

### **3. Element Operators**

Used to check field existence or type.

|**Operator**|**Description**|**Example**|
|---|---|---|
|`$exists`|Matches documents with or without a field.|`{ age: { $exists: true } }`|
|`$type`|Matches documents with fields of a specific type.|`{ age: { $type: "int" } }`|

---

### **4. Evaluation Operators**

Used for advanced matching like regex and JavaScript expressions.

|**Operator**|**Description**|**Example**|
|---|---|---|
|`$regex`|Matches documents where a field matches a regex.|`{ name: { $regex: "^A" } }`|
|`$expr`|Allows using aggregation expressions in queries.|`{ $expr: { $gt: ["$sales", "$target"] } }`|
|`$mod`|Performs modulo operation.|`{ age: { $mod: [5, 0] } }`|

---

### **5. Array Operators**

Used to query and manipulate arrays.

|**Operator**|**Description**|**Example**|
|---|---|---|
|`$all`|Matches arrays containing all specified values.|`{ tags: { $all: ["tech", "science"] } }`|
|`$size`|Matches arrays with a specified number of elements.|`{ tags: { $size: 3 } }`|
|`$elemMatch`|Matches documents with at least one array element satisfying all conditions.|`{ scores: { $elemMatch: { $gte: 80, $lt: 90 } } }`|
|`$slice`|Limits the number of array elements returned.|`{ comments: { $slice: 2 } }`|

---

### **6. Bitwise Operators**

Used for bitwise operations on numeric fields.

|**Operator**|**Description**|**Example**|
|---|---|---|
|`$bitsAllSet`|Matches documents where all specified bits are set.|`{ permissions: { $bitsAllSet: 6 } }`|
|`$bitsAnySet`|Matches documents where any of the specified bits are set.|`{ permissions: { $bitsAnySet: 2 } }`|

---

### **7. Update Operators**

Used to modify documents.

|**Operator**|**Description**|**Example**|
|---|---|---|
|`$set`|Sets the value of a field.|`{ $set: { age: 30 } }`|
|`$unset`|Removes a field.|`{ $unset: { age: "" } }`|
|`$inc`|Increments a field by a value.|`{ $inc: { count: 1 } }`|
|`$push`|Appends a value to an array.|`{ $push: { tags: "new" } }`|

---

### **8. Aggregation Operators**

Used in the aggregation framework for transforming data.

|**Operator**|**Description**|**Example**|
|---|---|---|
|`$sum`|Calculates the sum of numeric values.|`{ $group: { _id: null, total: { $sum: "$price" } } }`|
|`$avg`|Calculates the average of numeric values.|`{ $group: { _id: null, avgAge: { $avg: "$age" } } }`|
|`$match`|Filters documents based on conditions.|`{ $match: { age: { $gte: 25 } } }`|
|`$project`|Shapes the output documents by including/excluding fields.|`{ $project: { name: 1, age: 1 } }`|

---

### **9. Geospatial Operators**

Used for querying geospatial data.

|**Operator**|**Description**|**Example**|
|---|---|---|
|`$geoWithin`|Matches documents within a specified geometry.|`{ location: { $geoWithin: { $center: [ [ 0, 0 ], 5 ] } } }`|
|`$near`|Finds documents near a point.|`{ location: { $near: [ 0, 0 ] } }`|

---


MongoDB operators are powerful tools for querying and manipulating data. Let me know if you want a deep dive into a specific category!