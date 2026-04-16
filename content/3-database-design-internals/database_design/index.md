
**Resource :**
1. CMU notes  : [notes](https://15445.courses.cs.cmu.edu/fall2025/notes/)


- Maintain consistency in naming entities, either singular or plural throughout. singular is recommanded.

## 1. What is Entity Relationship Model ?


- A **conceptual model** used to describe the structure of a database.


## Entity : 

- An entity is someting from the real world, like a person, place, event or idea.
- Each entity has special features or traits (attributes, constraints) that describe it.
- Think of an entity as a single table in a relational database.


### Examples of Entities

| Entity      | Real-World Representation     | Example Attributes                                  |
| ----------- | ----------------------------- | --------------------------------------------------- |
| **Student** | A person enrolled in classes. | `StudentID`, `Name`, `Major`, `Email`               |
| **Course**  | A subject offered for study.  | `CourseID`, `Title`, `Credits`, `Department`        |
| **Book**    | A publication.                | `ISBN`, `Title`, `Author`, `PublicationYear`        |
| **Order**   | A record of a transaction.    | `OrderID`, `OrderDate`, `CustomerID`, `TotalAmount` |


### Strong vs. Weak Entities

Entities are generally categorized into two types:

| Type              | Description                                                                                                                                                                                                                                                                                        | Example                                                   |
| ----------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------- |
| **Strong Entity** | An entity that has its own **primary key** (unique identifier) and can exist independently of any other entity. It's represented by a **single rectangle** in an ER diagram.                                                                                                                       | **Employee**, **Department**                              |
| **Weak Entity**   | An entity that **does not have a primary key** of its own and its existence is **dependent** on another entity (the _identifying owner_ or _parent_). It must use the primary key of the owner entity as part of its own identifier. It is represented by a **double rectangle** in an ER diagram. | **Dependent** (of an Employee), **Payment** (of an Order) |
