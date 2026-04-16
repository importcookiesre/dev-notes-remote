

The storage engine (or database engine) is a software component of a database management system responsible for storing, retrieving, and managing data in memory and on disk, designed to capture a persistent, long-term memory of each node.

Storage engine provide simple data manipulation API, allowing users to create, update, delete and retrieve records.

At storage engine layer both keys and values can be arbitary sequences of bytes with no prescribed types. 

Their representation is defined at higher level of the database architecture.

Storage engines were developed independently from the database management systems they’re now embedded into. Simply they are pluggable which allows to bootstrap database system easily using existing storage engine.
- Example : BerkeleyDB, LevelDB and its descendant RocksDB, LMDB and its descendant libmdbx, Sophia, HaloDB, and many others 

This pluggable design allows to switch between different engines for specific use cases. 
Example : MySQL has several database engines like InnoDB, MyISAM and RocksDB. Similarly MongoDB allows switching between WiredTiger, In-Memory and the MMAPv1 (deprecated) storage engines.




