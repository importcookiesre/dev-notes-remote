
==A _goroutine_ is a lightweight user-space thread managed by the Go runtime.==

It is the smallest unit fo concurrent execution in Go.

## The Goal of the Go Scheduler : 

The operating system has no idea what a goroutine is. It only schedules OS Threads. The Go Scheduler’s job is to act as a hyper-efficient middleman, multiplexing millions of user-space goroutines onto a tiny, fixed number of OS Threads.

To achieve this, it uses three primary entities: **G**, **M**, and **P**.

### The GMP Actors

#### 1. G: Goroutine (The Work)

A `G` represents a single goroutine.

- It contains the execution stack (starting at just ~2KB), the instruction pointer (which line of code is executing), and its current status (runnable, blocked, or running).
    
- **Scale:** You can easily have 1,000,000 `G`s in memory.
    

#### 2. M: Machine (The OS Thread)

An `M` represents an actual OS Thread managed by the kernel.

- `M` is the physical worker that executes the code.
    
- An `M` does not execute a `G` directly; it must first acquire a `P`.
    
- **Scale:** The Go runtime tries to keep the number of active `M`s low (usually matching your CPU cores) to avoid the expensive context-switching overhead of the OS.
    

#### 3. P: Processor (The Context / Dispatcher)

A `P` represents a Logical Processor. This is the stroke of genius in Go's design.

- A `P` acts as a localized queue manager. Every `P` holds a **Local Run Queue (LRQ)** of up to 256 runnable `G`s.
- By default, the number of `P`s is set exactly to the number of physical CPU cores on the host machine (controlled by the `GOMAXPROCS` environment variable).
    
- **Rule:** An `M` _must_ hold a `P` to execute Go code.
    

There is also a **Global Run Queue (GRQ)**, which holds `G`s that have not yet been assigned to a specific `P`'s local queue.


## How they works  ?


### Scenario 1 : 

- You have a 4-core machine, so Go creates 4 `P`s.
    
- Go spins up 4 `M`s (OS Threads) and attaches one to each `P`.
    
- You spawn 100 goroutines (`G`s). They are divided among the Local Run Queues of the 4 `P`s.
    
- Each `M` continuously pops a `G` off its attached `P`'s queue and executes it. Because this happens entirely in user-space, context switching between `G`s takes nanoseconds.

### Scenario 2 : Work Stealing (Load Balancing)

What happens if `P1` finishes all the goroutines in its queue, but `P2` still has 50 goroutines waiting? We don't want CPU core 1 sitting idle.

1. `P1` looks at its empty local queue.
    
2. It checks the Global Run Queue. If it's empty, it moves to the next step.
    
3. **The Steal:** `P1` looks at `P2`'s local queue and literally _steals_ half of its runnable `G`s (25 tasks).
    
4. `P1` resumes executing. This guarantees all CPU cores are constantly utilized without requiring a central, locked bottleneck queue.


### Scenario 3: Blocking System Calls (The Hand-off)

This is where backend engineers need to pay attention. What happens when a `G` makes a blocking system call (like reading a heavy file from disk)? The underlying OS Thread (`M`) is going to block at the kernel level.

1. `G1` makes a blocking file read. The kernel suspends `M1`.
    
2. If `M1` is blocked, its attached `P1` (and all the other `G`s in `P1`'s queue) are now stuck!
    
3. **The Hand-off:** The Go runtime detects this. It aggressively detaches `P1` from the blocked `M1`.
    
4. The runtime then wakes up an idle `M` (or creates a new one, `M2`) and attaches `P1` to it.
    
5. `P1` continues executing its queue on the new `M2`.
    
6. When the original blocking call finishes, `G1` is put back into a queue, and `M1` is put to sleep for future use.
    

_(Note: Network I/O, like querying Postgres, is handled slightly differently using the **Network Poller**. Instead of blocking the `M`, the `G` is parked asynchronously, and the `M` immediately moves to the next `G`. When the network responds, the `netpoller` wakes the `G` back up. This is how Go handles thousands of database connections on a single thread)._



## The Goroutine Lifecycle : 


Here is the exact journey of a goroutine.

### 1. Creation (`_Gidle -> _Grunnable`)

- When you use the `go` keyword, the runtime allocates a small ~2KB stack.
- It places the new goroutine into a **Runnable** state (`_Grunnable`) and drops it into a Local Run Queue (LRQ) of a Processor (`P`). At this point, it is waiting for its turn on the CPU.

### 2. Execution : `_Grunnning`

- An OS Thread (`M`) pops the goroutine off the queue.
    
- The state changes to **Running** (`_Grunning`). The CPU's Program Counter (PC) is pointed to the goroutine's function, and the code actually executes.


### 3. Blocking : `_Gwaiting`

- Goroutines rarely run to completion without stopping. If the code makes a network request to Postgres, reads from a file, waits on a `sync.Mutex`, or reads from a Go Channel, it cannot continue.
    
- The runtime changes the state to **Waiting** (`_Gwaiting`).
    
- **Crucial step:** The goroutine is detached from the `M`. The `M` goes to the queue and instantly picks up the next `_Grunnable` goroutine. The blocked goroutine sits in a special waiting structure (like the Network Poller or a Channel's wait queue).


### 4. Unblocking `_Gwaiting -> _Grunnalbe`

- When the Postgres query returns data, the Network Poller fires an event.
    
- The runtime grabs the waiting goroutine, flips its state back to **Runnable** (`_Grunnable`), and tosses it back into an LRQ to wait for CPU time again.


### 5. Termination : `_Gdead`

- Once the function `return`s, the state becomes **Dead** (`_Gdead`).
    
- The runtime does not immediately destroy the underlying `g` struct. To save memory allocation overhead later, it strips the goroutine of its data and puts the empty `g` shell into a "free list" (a pool) to be recycled the next time you use the `go` keyword.


## Context Switching in Goroutine : 

A context switch is the act of saving the state of the current running task so that it can be paused, and loading the state of a new task to be executed.


### 1. The Heavy OS Thread Swithch : (Context : Ring 0 / Kernel Space  )

When the Linux kernel swaps out OS Thread A for OS Thread B, it is a violent, expensive operation:

1. **The Trap:** The CPU must transition from User Space (Ring 3) down to Kernel Space (Ring 0). This transition alone takes time.
    
2. **The State Save:** The OS must save 16+ advanced CPU registers (including floating-point and vector registers).
    
3. **Memory Map Overhead:** While threads share memory, the OS still has to do safety checks and potentially flush parts of the CPU's Translation Lookaside Buffer (TLB), leading to CPU cache misses when Thread B starts running.
    

- **Cost:** ~1,000 to 2,000 nanoseconds. (Which means if you have 10,000 threads, your CPU spends all its time switching and no time computing).


### 2. The Lightweight Goroutine Switch (Context: Ring 3 / User Space)

When the Go runtime swaps Goroutine A for Goroutine B, the OS Kernel is completely blind to it. The OS just sees a single thread executing normally.

1. **No Trap:** Everything happens in User Space. No kernel interrupts.
    
2. **The Minimal State Save:** The Go runtime only needs to save three main things into the `gobuf` struct:
    
    - **Program Counter (PC):** "What line of code was I on?"
        
    - **Stack Pointer (SP):** "Where is my local memory?"
        
    - **DX Register:** (Used for context).
        
3. **No Cache Flushing:** Because the OS thread never actually paused, the CPU cache stays hot.
    

- **Cost:** ~200 nanoseconds. (Roughly 80-90% faster than an OS thread switch).


### The Analogy: The Chef in the Kitchen

- **OS Thread Context Switch:** The Chef (CPU) is cooking a soup (Thread A). To switch to cooking a steak (Thread B), the Chef must leave the kitchen, change out of their uniform, wash their hands, put on a new uniform, re-enter the kitchen, and get out entirely new pots and pans.
    
- **Goroutine Context Switch:** The Chef is waiting for the soup to boil (Network I/O). Instead of leaving the kitchen, the Chef simply pivots 90 degrees and starts chopping onions on the cutting board right next to them.