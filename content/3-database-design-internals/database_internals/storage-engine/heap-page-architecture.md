
Resource : 
1. [how postgres stores rows :](https://ketansingh.me/posts/how-postgres-stores-rows/)


Note : 
1. 


## Heap Page : 

A **heap page** (heap block) in PostgreSQL is a fixed-size container (usually 8 KB) that holds rows (tuples) of a table. The entire table is just an array of such pages in a heap file on the disk.

Each page is independent and can store any row of the table (non sorted).

When a query or update touches a table row, the database figures out which page that row is on by its block number (CTID), and then loads that page into memory (the shared buffer cache) for access.

Any new or updated row can go to any free space without reorganizing the whole file. The trade-off is that locating rows by some key usually requires an index.

This design simplifies inserts and concurrency and allows multiple versions of a row can coexist in the heap without blocking readers.


## Page Architecture  : 

Internally, a page is divided into regions header, item pointers, free space, actual tuples. Tuples gets inserted from from the end of a page (Bottom Up) and free space lies between metadata and tuples/data.

The heap page has five main parts :
1. Page Header
2. Item ID Array (Item Pointers)
3. Free Space
4. Tuples (Row Data)
5. Special Space

![[Pasted image 20260415165610.png]]
![[Pasted image 20260415155657.png]]
## 1. Page Header (24 bytes) :

**The page header has 3 jobs**
1. **locate the usable areas of the page** : `pd_lower, pd_upper, pd_special`
2. **protect the page from corruption / crash issues** : `pd_lsn, pd_checksum, pd_pagesize_version`
3. **help PostgreSQL decide whether cleanup is needed** : `pd_flags, pd_prune_xid`




### 1. Boundary fields : 

Used to understand the physical layout. They holds bytes offset value which is used to specify the boundry.

1. `pd_lower` : Tells where free space begins from the front after the Line Pointer arrays ends. It means everything before the pd_lower is occupies by line pointer array and page header.
2. `pd_upper` : Tells where the free space ends from the back and where the tuple area begins. Tuples are stored from here and growing backword.
3. `pd_special` : Tells where special space begins. It tells where the page's reserved tails starts. 
	- For heap pages, this is usually the very end of the page, so special space is effectively unused.
	- For index pages, special space matters a lot, because the index needs page specific metadata there.

**For example :**
If `pd_lower=200` and `pd_upper=7900` then bytes `[201, 7899]` are free.  If we insert a new tuple of size 100 bytes. 
- The line pointer will take 4 bytes from `[201-204]` offset.
- The tuple will take `[7800,7899` offset.
- The new `pd_lower` will be 
- The new `pd_upper` will be

### 2. Safety and recovery fields. 

These fields help PostgreSQL trust the page after crashes or hardware issues.
1. `pd_lsn` (page data log sequence number) : Stores the last WAL position that affected the page. It is used for crash recovery and WAL consistency. During crash recovery postgres compares the WAL position number and `pd_lsn` number and based on that it ensures consistency.
2. `pd_checksum` : Datects byte level corruption. It helps to detect torn writes or damaged pages correctly.
3. `pd_pagesize_version` : Helps postgres interpret the page correctly. It encodes page size and format version.



### 3. Page state and optimization fields : 

These helps postgres make faster decisions.
1. `pd_flags` : 
2. `pd_prune_xid` : Tells postgres whether cleanup is worth checking. It says "there may be tuples on this page that are eligible for prune/vacuum."  Postgres does not inspect every page deeply on every visit, it just check `pd_prune_xid` flag which helps it decide wherther a page is worth pruning.


## 2. Item ID Array (Line Pointer) : 

An array of 4-byte entries (ItemIdData), each pointing to a tuple’s location (offset and length) on the page. 
The array of pointer immediately starts are the 24 bytes header
Each `Item ID` data contains : 
1. `offset` :  offset points to where the tuple starts.
2. `length` : length says how long it is.
3. `flags` (state) :  




## 3. Free Space :

Unused bytes between the item array and tuple storage. As tuples and pointers grow, free space shrinks. New pointers consume space from the bottom up, and new tuples consume from the top down.


## 4. Tuples (Row Data) :

- **Tuples (Row Data):** Actual row bytes stored at the end of free space, growing backward. Each tuple starts with its own header (e.g. xmin, xmax, ctid, infomasks) and then the column values.


## 5. Special Space : 

- Reserved for index pages; ordinary tables have none (pd_special points to page end).





