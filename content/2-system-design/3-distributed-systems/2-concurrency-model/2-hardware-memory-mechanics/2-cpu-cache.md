

## The CPU cache : 

Main memory (RAM) is incredibly slow compared to a modern CPU. If a 3GHz CPU core had to wait for RAM to fetch every single variable, it would spend most of its life frozen.

To solve this, CPUs have built-in SRAM (Static RAM) caches. These are physically located on the CPU die, organized in a hierarchy:

- **L1 Cache (Level 1):** The smallest and fastest. It is strictly private to a single CPU core. It takes roughly ~1-2 nanoseconds to access.
    
- **L2 Cache (Level 2):** Larger, but slightly slower (~3-5 nanoseconds). In modern architectures, it is also usually private to a single core.
    
- **L3 Cache (Level 3):** The largest cache, but the slowest (~10-20 nanoseconds). Crucially, **the L3 cache is shared across all CPU cores.**


## The Cache line : 

When you read a single `int64` (8 bytes) in Go, the CPU does **not** travel to RAM, grab those 8 bytes, and bring them back. That would be wildly inefficient.

Hardware is designed around the principle of **Spatial Locality**: if you read a specific memory address, you are highly likely to read the neighboring addresses very soon (e.g., iterating through an array or reading fields in a struct).

Instead of fetching single bytes, the CPU fetches memory in fixed-size chunks called **Cache Lines**. On almost all modern x86 and ARM architectures, a cache line is exactly **64 bytes**.

If you ask for a 1-byte boolean, the CPU fetches that byte _and the 63 bytes sitting next to it_, locking them all into the ultra-fast L1 cache as a single 64-byte block.



## The False Sharing Problem : 

Because of the Visibility Problem (Cache Coherence) we discussed previously, CPU cores must coordinate. If Core 1 modifies a variable in its L1 cache, it must tell all other cores to invalidate their copies of that data.

**The Catch:** The CPU does not track invalidations byte-by-byte. It tracks them **cache-line by cache-line**. If Core 1 modifies _any byte_ within a 64-byte cache line, the _entire 64-byte line_ is marked as invalid for all other cores.

### The Problem: 
Assume there are two variable in an array, there are two goroutines running at core 1 and core 2. 
Core1 fetches the first variable which in turns fetches the entire cache line (here 8 bytes). It changes the value which invalidates the entire cache line.
Now if core 2 has the same cache line, it can't use it as core 1 has invalidated it, so it has to fetch the cache line again.

Core 1 and Core 2 are not sharing data (they are updating completely independent variables). But because the variables share the _same physical cache line_, they constantly invalidate each other's L1 caches. The cores spend all their time fetching data from RAM instead of doing math. Performance collapses.


### The solution : 

To fix False Sharing, you must force the variables to sit in completely different cache lines. You do this by artificially injecting empty space (padding) between them so they are spaced at least 64 bytes apart in memory.

```go
type Metrics struct {
    Requests uint64     // 8 bytes (Cache Line A)
    
    // Padding: 56 bytes of useless data to fill the rest of the 64-byte line
    _ [56]byte          
    
    Errors   uint64     // 8 bytes (Pushed safely into Cache Line B)
}
```

Go provides a cleaner way to do this using `cpu.CacheLinePad` from the `golang.org/x/sys/cpu` package, which automatically calculates the correct padding size for the architecture

