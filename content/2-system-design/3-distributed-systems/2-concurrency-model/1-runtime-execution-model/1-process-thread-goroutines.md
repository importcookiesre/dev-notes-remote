

## Process vs Thread 


### 1. First what is Program ?

Program is a ==set of instruction== written in a programming language. It is strored as a file in the disk.
One program can have multiple processes. For example, the Chrome browser creates a different process for every single tab.

### 2. Process : 

When the code in the program is loaded into the memoy and executed by the processor, it becomes a process. Therefore, a process is an ==executing instance of  a program==.
The process requires some essential resources such as registers, program counter, and stack which is provided by the OS.

- context switching in a process is expensive.
	- Switching from process A to process B requires the OS to save the CPU registers, flush the Translation Lookaside Buffer (TLB), and load the new memory maps. This takes time.

==“A process is the OS's fundamental unit of resource allocation and isolation.”==

1. Resource Allocation : 
	1. When OS runs a program, it creates a process and assign resources to it, which includes memory, cpu time, open files, sockets, file descriptors etc.
	2. The OS allocates resouces per process at the fundamental level.
2. Isolation : 
	1. Isolation means **processes cannot interfere with each other’s memory or resources**.
		1. Therefore chrome cannot access the memory of your Go process or VS code process.

![[Pasted image 20260306163239.png]]

**Independent memory**
- Each process has its own memory address space (heap, stack, code, data).
- One process cannot directly access another process's memory.
- It means that if one process mal-functions, doesn't effects other process.




## 3. Thread : 

- A Thread is the smallest ==unit of execution== within a process.
	Thread creation, termination, and context switching are faster and lighter than processes.

- Each thread has its own stack, registers and program counters.
![[Pasted image 20260306165846.png]]


- A single process can have many threads and they have a share memory within the process which allows them to communicate.
![[Pasted image 20260306170029.png]]


**How it works:**

- **Shared Memory:** All threads within a process share the exact same virtual memory space, heap, and file descriptors.
    
- **Private State:** Each thread only gets its own private Stack (for local variables) and CPU Registers.
    
- **Kernel Scheduled:** Just like processes, the OS kernel is completely responsible for scheduling threads.
    
- **Context Switching is Cheaper (but not free):** Because threads share memory maps, the OS doesn't need to flush the TLB when switching between threads in the same process. However, it still requires a trip into kernel space (Ring 0) to perform the switch.

**Real-World Backend Use Case:** 
Traditional web servers (like Apache's worker MPM or a Java Spring Boot application) spawn an OS thread for every incoming HTTP request. This is faster than spawning processes, but if 10,000 concurrent users hit the server, the OS struggles to manage 10,000 threads, leading to high CPU overhead just from context switching (thrashing).


### Why Thread is not the unit of isolation ?

1. Threads live inside a process. All threads shares memory, heap, global variables. If one thread corrupts memory due to panic or segmentation fault the entire process crashers.
2. So the isolation is at the process level and not thread level.




### 3. Green Threads : Goroutines 

==A _goroutine_ is a lightweight user-space thread managed by the Go runtime.==

Green threads (called Goroutines in Go) are user-space threads. **The operating system has absolutely no idea they exist.**
