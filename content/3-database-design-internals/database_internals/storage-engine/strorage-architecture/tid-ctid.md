
In PostgreSQL, **every row (tuple) has a TID (tuple identifier)** which is essentially a pair _(block, offset)_. The **block number** is the heap page number, and the **offset number** is the index into the page’s line pointer array. For example, `(block=0, offset=5)` means “page 0, fifth line pointer”. This TID is known as a CTID when seen as the system column in SQL.


- **TID (Tuple Identifier):** A physical location identifier used internally by PostgreSQL. It is structured as a pair of integers: $(BlockNumber, OffsetNumber)$.
    
    - `BlockNumber`: The 0-based index of the 8KB physical page (block) within the table's underlying data file.
        
    - `OffsetNumber`: A 1-based index into an array of Line Pointers located at the beginning of that specific 8KB page.
        
- **CTID (Current Tuple Identifier):** A hidden system column present on every row in PostgreSQL. It holds the TID of that specific row version. If a row is updated (and a new tuple is created), the new tuple gets a new CTID, and the old tuple's CTID is updated to point to the new tuple.

On UPDATE, a new row version gets a new TID; the old version’s CTID field is updated to link to the new one. This indirection is why CTIDs move on update and why indexes reference tuple identifiers (ItemPointers)

we can also query the CTID.  `SELECT ctid, * FROM mytable;
It has two pair of data. 
1. page number 
2. tuple number

![[Pasted image 20260415102750.png]]

