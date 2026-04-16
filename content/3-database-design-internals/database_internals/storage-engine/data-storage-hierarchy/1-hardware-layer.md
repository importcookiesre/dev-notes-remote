At the hardware level, disks store data as binary states (0s and 1s) using either magnetic polarity or trapped electrons.

The OS does not directly interact with the physical geometry directly :

## Physical Geometry in Hard Disk Drives : 

How data is physically arranged on magnetic platters, consisting of concentric tracks, subdivided into sectors (often 4KB or 512 bytes)
To read data at the physical level, the hardware must physically move the actuator arm to the correct **Cylinder**, select the active **Head**, and wait for the platter to rotate to the specific **Sector**. This is know as Cylinder-Head-Sector(CHS). CHS is now replaced by Logical Block Addressing (LBA) which we will study in the later part.
### Key components of HDD Physical Geometry :

1. Platters : Circular disks coated with magnetic material that store data. A drive may contains multiple platters stacked together. Both side of the platter are used for storing data.
2. Surface/Heads : Each surface of the platter has a dedicated Read/Write head.
3. Tracks: Concentric rings on a surface where data is stored.
4. Sectors : The smallest physical storage unit, dividing tracks into ==arcs==. Sectors generally store 512 bytes or 4KB.
5. Cylinder : A cylinder in a hard disk drive (HDD) is ==the set of all tracks located at the same distance from the spindle across all platters==. It aligns track numbers vertically, allowing read/write heads to access data on all surfaces without moving the actuator arm, which minimizes latency and improves data access speed.
	1. **Purpose:** Operating systems fill an entire cylinder (multiple surfaces) before moving the arm to the next cylinder to optimize performance. So that it can access data from all the cylinder at once.
![[Pasted image 20260411110806.jpg]]
![[Pasted image 20260411110904.jpg]]



## Physical Geometry in SSD : 

Solid State Disk are build from NAND flash memory.
	**Nand Flash Memory** is a type of non-volatile storage technology that retains data without power, commonly used in SSDs, USB drives, smartphonse and memory cards.
	Uses NAND gate transistors for storing data by trapping electorn.
	They have high storage density, fast write speeds and low cost per bit.

### Components and hierarchy : 

1. NAND package (chip) : The physical black chips soldered onto the SSD's printed circuit board. A single package contains multiple stacked silicon dies.
![[Pasted image 20260411114513.jpg]]


2. Die (LUN) : A die is the smallest independent unit of the SSD that can execute commands and report status. An SSD controller can execute operations concurrently across multiple dies to achieve high throughput.
![[Pasted image 20260411114614.jpg]]
![[Pasted image 20260411114637.jpg]]

3. Plane : Each die is divided into planes (typically 2, 4, or 8 per die). A plane contains an array of memory blocks and a dedicated Page Register (a small SRAM buffer). Operations can be executed simultaneously on different planes within the same die, provided the operations are of the same type (e.g., two parallel reads).
![[NAND-Die-Multi-Plane-Small-19f75846.webp]]

4. Block : A plane is subdivided into thousands of blocks. **A block is the smallest unit of erasure** ie, data can only be erased at block level. A typical block contains 128 to 512 pages, resulting in a block size ranging from 4MB to 16MB.
![[Pasted image 20260411114924.png]]


5. Page : A block is subdivided into pages. **A page is the smallest unit of read and write operations.** Typical page sizes in modern SSDs are 4KB, 8KB, or 16KB.

6. Cell : The fundamental transistor (floating-gate or charge-trap) that stores electrons. Cells are grouped to form pages. Depending on the architecture, a cell stores different amounts of data: SLC (1 bit/cell), MLC (2 bits/cell), TLC (3 bits/cell), or QLC (4 bits/cell).


### read, write and erase : 

1. read : data can be read independently at the **page level** and it is extremely fast.
2. write : data can be written at the page level but only to empty pages(erased). Overwrite is not supported. Writing takes longer than reading. Writes must also happen sequentially within a block.
3. Erase : To overwrite a single 8KB page, the SSD must physically erase the data. However, the hardware cannot erase a single page; **it must erase the entire 16MB block** that contains the page. Erasure involves applying a high voltage to reset the cells and is extremely slow (several milliseconds).


