

- It is a candidate key.
- It is used to uniquely identify the table.
- It cannot be null.
- There is only one primary key in a table.
- automatically creates index on primary key.


- **Analogy**: Think of `StudentID`, `Email`, and `PhoneNumber` as different forms of valid ID (candidate keys). The school decides that the `StudentID` will be the official ID used for all records. This makes `StudentID` the primary key.
    
- **Example**: In our `Students` table, we would likely choose `StudentID` as the primary key because it's stable (unlikely to change) and guaranteed to be unique.