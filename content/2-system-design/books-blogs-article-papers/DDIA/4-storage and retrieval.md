


# Storage and Indexing for OLTP 


The simplest form of database : 
```bash
#!/bin/bash
db_set () { 
	echo "$1,$2" >> database 
} 

db_get () { 
	grep "^$1," database | sed -e "s/^$1,//" | tail -n 1 
}
```
These two functions implement a key value store. The key and value can be almost anything.

The set operation :
- It stores comma seperated key value line by line.
- Every set operation appends a new key value at the end of the file.
- Inserting a value with the same key does not update the existing value but it just inset it at the end of the file, so old values still exists in the file.
- The performance is pretty good as it is just an append operation on a file.

The get operation : 
- It performs lookup from the end of the file.
- Always returns the latest key and value. (updated)
- Performance is bad because it scans the whole file therefore time complexity is O(n).


To improve the performance of the `get` operation we use index data structure.
The idea is to structure the data in such a way that makes it faster to locate example : sort the keys.


