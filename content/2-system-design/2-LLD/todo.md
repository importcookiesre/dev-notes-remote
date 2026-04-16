

1. How nginx handles 10k connections with a single thread ?


Chain of Responsibility: Allows requests to be passed along a chain of handlers, where each handler can process the request or pass it on.
Observer: A publisher-subscriber pattern that allows events to be sent from a publisher to a subscriber
Facade: Hides a complex API behind a simple interface
Singleton: Ensures that a class has only one instance
Abstract Factory: Creates families of related objects without specifying their concrete classes
Builder: Separates the construction of complex objects from their representation
Prototype: Creates new objects by cloning existing ones
Generator Pattern: A common concurrency pattern in Golang

Worker Pool Pattern: A common concurrency pattern in Golang

Pipeline Pattern: A common concurrency pattern in Golang

Fan-In Pattern: A common concurrency pattern in Golang

Semaphore Pattern: A common concurrency pattern in Golang

Timeout Pattern: A common concurrency pattern in Golang
### Phase 1: The Basics (Linear Flow & Signaling)

Before you run multiple things at once, you must master moving data in a straight line and stopping safely.

#### 1. The Pipeline

**Concept:** Breaking a large job into small, sequential stages.

- **Why Learn First?** It teaches you to think of channels as "streams" of data flowing from function to function.
    
- **Analogy:** An Assembly Line. One robot welds the frame, the next paints it, the last installs the tires.
    
- **Key Code:** `OutputChannel <- Processing( <- InputChannel )`
    

#### 2. The "Done" Channel (Broadcasting)

**Concept:** How to stop the pipeline.

- **Why Learn Second?** Once you start a pipeline, you need a way to turn it off without memory leaks (orphaned goroutines).
    
- **Analogy:** The Fire Alarm. You don't call every employee individually to tell them to evacuate; you trigger one signal that everyone hears instantly.
    
- **Key Code:** `close(doneCh)` triggers `case <-doneCh:` in all listeners.
    

---

### Phase 2: Scaling (Parallelism)

Now that you have a single line working, you want to make it faster by running multiple lines.

#### 3. Fan-Out / Fan-In

**Concept:** Splitting work (Fan-Out) and gathering results (Fan-In).

- **Why Learn Third?** This is the most basic form of parallelism.
    
    - **Fan-Out:** 1 Sender $\rightarrow$ 10 Workers.
        
    - **Fan-In:** 10 Workers $\rightarrow$ 1 Receiver.
        
- **Analogy:**
    
    - _Fan-Out:_ Throwing a handful of birdseed. 10 pigeons (goroutines) rush to peck at individual seeds.
        
    - _Fan-In:_ A funnel. Pouring water from 3 different buckets into a single narrow spout.
        

#### 4. The Worker Pool (Buffered Fan-Out)

**Concept:** Capping the number of active goroutines.

- **Why Learn Fourth?** Plain "Fan-Out" is dangerous. If you fan out 10,000 items, you crash the server. A Worker Pool limits this to a fixed number (e.g., 5 workers).
    
- **Analogy:** The Airport Security Check. Even if 5,000 passengers arrive at once, there are only 4 metal detectors. Passengers queue up; the system doesn't crash, it just works at max capacity.
    

---

### Phase 3: Reliability (Time & Control)

Now your system is fast, but what if it hangs? Or what if you need to limit access to a resource?

#### 5. Timeouts & Cancellation (The Context)

**Concept:** putting a time limit on your work.

- **Why Learn Fifth?** In the real world, networks fail. You cannot let a goroutine wait forever.
    
- **Analogy:** The "Pizza Guarantee." If the pizza isn't there in 30 minutes, you cancel the order and stop waiting.
    
- **Key Code:** `select { case <-res: ... case <-time.After(t): ... }`
    

#### 6. The Semaphore (Token Bucket)

**Concept:** Using a channel strictly to count, not to pass data.

- **Why Learn Sixth?** Sometimes you don't want a full worker pool; you just want to ensure only N people access a database at once.
    
- **Analogy:** The Restroom Key.
    
    - There is only 1 key hanging by the door.
        
    - To use the room, you must take the key (Receive/Send).
        
    - If the key is gone, you wait.
        
    - When you leave, you return the key.
        

---

### Phase 4: Advanced Safety (State Management)

This is the "Black Belt" level. How to manage data without getting race conditions.

#### 7. Confinement (Ad-hoc)

**Concept:** Structuring your code so data is _never_ shared. A variable belongs to exactly one goroutine at a time.

- **Why Learn Last?** It is a mindset shift. Instead of using Mutexes (locks) to protect shared data, you use channels to physically move ownership of the data.
    
- **Analogy:** A physical letter.
    
    - While I am writing on the paper, only I have it.
        
    - Once I mail it (send on channel), I physically cannot write on it anymore.
        
    - You receive it; now _you_ are the only one who can write on it. No locks needed.
        

---

### Summary Checklist for Learning

1. **Pipeline:** Connect A to B.
    
2. **Done Channel:** Stop A and B safely.
    
3. **Fan-Out/In:** Run five copies of B.
    
4. **Worker Pool:** Run _exactly_ five copies of B, no matter how much work comes in.
    
5. **Timeouts:** Stop B if it takes too long.
    
6. **Semaphores:** Let B access a shared resource safely.
    
7. **Confinement:** Ensure A and B never fight over data.
    

### A Next Step for You

The **Worker Pool** is the single most employable pattern on this list. It combines Channels, Range, WaitGroups, and Buffers into one robust architecture.

**Would you like me to guide you through building a production-ready Worker Pool from scratch?**