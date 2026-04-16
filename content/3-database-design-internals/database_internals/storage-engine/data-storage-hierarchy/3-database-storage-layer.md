
Blocks and Pages has different meaning in different layers of storage stack.

| **Architectural Layer**            | **Terminology**               | **Standard Size** | **Definition & Function**                                                                                                                                                                                                                  |
| ---------------------------------- | ----------------------------- | ----------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| **1. Database (PostgreSQL)**       | **Page / Block** (Synonymous) | 8KB               | The fundamental unit of database storage, memory (shared_buffers), and I/O. Postgres uses "page" when referring to data in RAM and "block" when referring to that same data on disk, but structurally, they are the exact same 8KB entity. |
| **2. Operating System (ext4/xfs)** | **Logical Block**             | 4KB               | The smallest unit of file system allocation. The OS groups raw hardware sectors into these logical blocks. An 8KB Postgres Block spans two 4KB OS Logical Blocks.                                                                          |
| **3. Hardware (SSD NAND)**         | **Hardware Page**             | 4KB - 16KB        | The smallest unit of physical **read and write** operations at the silicon level.                                                                                                                                                          |
| **3. Hardware (SSD NAND)**         | **Erase Block**               | 4MB - 16MB        | The smallest unit of physical **erasure**. It is a physical grouping of hundreds of Hardware Pages.                                                                                                                                        |

The database interaction layer refers to the internal storage engine architecture of the DBMS which include Buffer manager and storage manager.

It interacts with the host operating system's file system eg ext5/xfs

Databases like postgres engineer their **physical data layout** to strictly align with the underlying OS logical blocks and storage device physical geometry to achieve high throughput, ACID compliance etc.


## Architecture : 

While the OS interacts with storage via 4KB logical blocks, a database imposes its own structural abstraction. In PostgreSQL, the fundamental unit of I/O, storage, and memory management is the **Page** (also known as a block in PostgreSQL terminology).

- **Page Size:** PostgreSQL uses an 8KB (8192 bytes) page size by default.
- **Page Structure:** Every 8KB page contains a standard header (tracking transaction visibility/MVCC metadata), an array of item pointers, and the actual tuple (row) data growing from the bottom up.
- **Atomic I/O:** When a backend process executes an `UPDATE` that modifies a single integer in a row, PostgreSQL does not write 4 bytes to disk. It modifies the 8KB page in RAM (marking it as a "dirty" page in the `shared_buffers`), and eventually, the background writer flushes the entire 8KB page to the OS file system.

- Page/block is the smallest unit of data access in a database.
### **The Alignment Mechanism** 
Because PostgreSQL writes in 8KB chunks and the Linux `ext4` or `xfs` file system manages data in 4KB logical blocks, perfect alignment is critical.

- An aligned 8KB database write translates deterministically to exactly two 4KB OS block writes.
    
- The OS then translates this to the storage controller, which ideally maps it to a single 8KB physical SSD page or two contiguous 4KB physical SSD pages.
    

### **The "Torn Page" Anomaly and Crash Consistency**
The architectural mismatch between the 8KB database page and the 4KB OS block creates a severe vulnerability regarding crash consistency.

Because the OS file system guarantees atomicity only at the 4KB block level, writing an 8KB PostgreSQL page requires two separate 4KB I/O operations at the kernel level. If a hardware failure, kernel panic, or power loss occurs precisely after the first 4KB block is written but before the second 4KB block is finalized, the disk retains an 8KB chunk of data containing 4KB of new data and 4KB of old data.

This state is called a **Torn Page**. If the database attempts to read this page upon recovery, the internal checksum will fail, resulting in total data corruption.

To mitigate this hardware-level limitation, PostgreSQL implements **Write-Ahead Logging (WAL)** with **Full Page Writes (FPW)**. When an 8KB page is modified for the first time after a checkpoint, PostgreSQL writes the _entire_ 8KB page directly to the sequential WAL before writing the modified page to the primary data file. If a torn page occurs, PostgreSQL reconstructs the block during crash recovery by replaying the complete page image from the WAL.

