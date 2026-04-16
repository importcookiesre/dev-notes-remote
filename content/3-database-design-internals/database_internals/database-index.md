
A database index is a data structure, that improves data retrieval speed by acting as a lookup table. 
It allows the database engine to find specific rows without scanning the entire table. 
It stores sorted values from selected columns and pointers to the corresponding data rows.
When a query is executed against an indexed column, the database engine traverses the index structure to locate the pointer, and then performs a direct disk seek to fetch the required row.
Without an index, a database must perform a **Sequential Scan** (or full table scan) to find relevant rows, iterating through every tuple on the disk. This takes O(N) time complexity.


## Types of Index :



## database mutation : 

To understand the engineering trade-off between read and write performance, it is necessary to examine the physical disk operations and data structure mechanics that occur during database mutations (`INSERT`, `UPDATE`, `DELETE`).
👉 [[mutation]]


## Trade offs

