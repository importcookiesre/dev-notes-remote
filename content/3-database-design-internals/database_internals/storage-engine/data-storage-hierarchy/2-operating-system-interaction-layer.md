

The OS does not interact with files; it interacts with LBAs via a file system (e.g., ext4, xfs, ZFS).
- **Logical Blocks:** The file system groups sectors or physical pages into logical blocks. The standard block size for ext4 in Linux is **4KB**.
- **Inodes (Index Nodes):** A data structure that stores metadata about a file (permissions, size, timestamps) and an array of pointers to the actual logical blocks on the disk where the file's data resides.


### Logical Block Addressing (LBA)

Logical Block Addressing is a technique used ot identify and locate individual data blocks(sectors) stored on devices (HDDs, SSDs) by assigning a linear integer index to each block, starting from LBA 0. eg `LBA0 LAB1 LAB2 LAB3...`

Historically, storage systems relied on Cylinder-Head-Sector (CHS) addressing, which required the host OS to understand the physical, mechanical layout of magnetic disks. LBA delegates this responsibility to the storage controller firmware. LBA abstracts the physical geometry of the storage medium into a contiguous, zero-indexed linear array of blocks.

- **CHS (Old):** `Address = Cylinder + Head + Sector`
- **LBA (New):** `Address = Single Sector Number` (e.g., LBA 1000)

