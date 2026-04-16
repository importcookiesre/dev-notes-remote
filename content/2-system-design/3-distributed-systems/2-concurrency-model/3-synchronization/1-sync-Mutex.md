
read here 

[[mutex]]
[[read_write_lock]]



## Lock Contention Analysis: 



Lock Contention Analysis is the process of **measuring and understanding how much time goroutines spend waiting for locks**, so you can find performance bottlenecks in concurrent systems.

Imagine 100 goroutines accessing a shared map.

```go
goroutine 1 -> Lock -> work -> Unlock
goroutine 2 -> waiting
goroutine 3 -> waiting
goroutine 4 -> waiting
```

Only **one goroutine can hold the lock**.
The rest are **blocked by the runtime scheduler**.
That waiting time is **lock contention**.
So the total time becomes:
When waiting time grows large, performance collapses.
```go
work time + waiting time
```



## Strategy to Mitigate Contention : 

**1. Shrink the Critical Section** The simplest fix. Do less work inside the lock.

- **Bad:** Lock → Make an HTTP request (takes 200ms) → Update Map → Unlock. (You just locked your data structure for 200ms).
- **Good:** Make HTTP request → Lock → Update Map (takes 10ns) → Unlock.

**2. Reduce Lock Granularity (Sharding)** Instead of locking the entire data structure, lock only the piece you need. If you have a global cache map, and all threads lock the entire map to update different users, you have massive contention. Instead, create an array of 256 separate maps, each with its own Mutex. To find a user, you hash their ID, find which of the 256 "shards" they belong to, and lock _only_ that shard. The contention is instantly divided by 256.

**3. Use the Right Lock Type** Switching from a `Mutex` to an `RWMutex` if the workload is 99% reads. This allows readers to overlap and drastically reduces waiting time.

**4. Go Lock-Free (Atomics / CAS)** If the data you are protecting is a simple counter or a pointer swap, remove the Mutex entirely and use hardware-level atomic operations (`atomic.AddInt64`).



## `sync.Cond`

It is used when goroutines must **wait until a certain condition becomes true**.

Instead of repeatedly checking something in a loop (busy waiting), goroutines can **sleep efficiently and be woken up when the condition changes**.


### The problem :

```go
for {
    mu.Lock()
    if cacheIsReady {
        mu.Unlock()
        break
    }
    mu.Unlock()
    time.Sleep(10 * time.Millisecond) // Please don't do this
}
```
Problems:
- wastes CPU
- adds latency
- ugly logic

### The solution : 

A Condition Variable allows goroutines to peacefully go to sleep and be instantly woken up the exact microsecond a specific condition becomes true. It eliminates both CPU thrashing and artificial latency.

A `sync.Cond` is always paired with a `sync.Mutex` (or `sync.RWMutex`).

- wake one waiting goroutine
	- `cond.Signal()`
- wake all waiting goroutines
	- `cond.Broadcast()`