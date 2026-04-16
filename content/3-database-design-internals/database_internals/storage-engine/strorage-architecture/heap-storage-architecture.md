
- Resource :
	- https://dev.to/khold/inside-the-database-a-deep-dive-into-disk-oriented-dbms-o34
- 


## Heap File

A heap file is a file where the database(postgres) stores data for a table in a unorganized/unsorted order.
It is generally located at `/home/user/pgdata/base/12705/16384*` address.
- PostgreSQL creates a separate physical file for every new table you create, though the actual file creation on disk may be deferred until the first row is inserted.
- To make the data management easier, each heap file is splitted into different **pages** with a default size 8KB.
- Data is inserted in these pages/blocks without any sorting.
- the heap file is divided into segments when it exceeds 1 GB which is cofigurable during compile time using `--with-segsize`.
- Supporting Forks : In addition to the main heap file containing table data, PostgreSQL creates auxiliary files for each table, such as the Free Space Map (`_fsm`) and the Visibility Map (`_vm`).



