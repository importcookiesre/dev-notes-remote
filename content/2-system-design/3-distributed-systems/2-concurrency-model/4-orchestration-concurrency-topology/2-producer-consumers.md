

Producers create jobs/items and push them into a queue; consumers take jobs from the queue and process them. The queue decouples producers from consumers so they can run at different speeds.



## Why producer and consumer ?

- Decoupling: producers and consumers can scale independently.
- Buffering: absorbs bursts so consumers can process at their own pace.
- Backpressure: bounded queues let producers slow down when consumers are overloaded.




## varients :

- Single-producer single-consumer (SPSC): very cheap, simple.
- Multiple-producer multiple-consumer (MPMC): common in web servers, requires safe queue (channels are fine).
- Worker pool: tasks are enqueued and a fixed number of workers consume and process — good for limiting concurrency and resource usage.
- Bounded queue: predictable memory and natural backpressure.
- Unbounded queue: producers rarely block but risk memory growth.
- Non-blocking offer: producer can drop tasks when queue is full (useful for metrics/telemetry).

## Design patterns :

- Fan-out: many workers read from same tasks channel.
- Fan-in: many producers write into same tasks channel.
- Pipelines: chain stages using channels between stages.
- Cancelation: use context to stop producers and workers.
- Graceful shutdown: close tasks channel after producers finish, wait for workers.


## Example : 


### 1. Minimal SPSC :

```go
package main

import "fmt"

func main() {
    ch := make(chan int) // unbuffered = synchronous handoff

    // producer
    go func() {
        for i := 0; i < 5; i++ {
            ch <- i
        }
        close(ch)
    }()

    // consumer
    for v := range ch {
        fmt.Println("got", v)
    }
}
```


### 2. Worker pool  :
This example demonstrates:

- multiple producers
- fixed worker count
- graceful shutdown via channel close
- context usage for early cancellation
- collection of processed results
- optional non-blocking offer to drop tasks when overloade


```go
package main

import (
	"context"
	"fmt"
	"sync"
	"time"
)

// Task is a small unit of work.
type Task struct {
	ID int
	// add payload fields as needed
}

// worker reads from tasks and writes processed IDs to results.
// It returns when tasks channel is closed or context is cancelled.
func worker(ctx context.Context, id int, tasks <-chan Task, results chan<- int, wg *sync.WaitGroup) {
	defer wg.Done()
	for {
		select {
		case <-ctx.Done():
			// early stop requested
			return
		case t, ok := <-tasks:
			if !ok {
				// channel closed - no more tasks
				return
			}
			// simulate work
			time.Sleep(20 * time.Millisecond)
			// send processed result; if results channel is blocked this will block.
			results <- t.ID
			_ = id // worker id could be used for logging
		}
	}
}

// StartWorkerPool launches numWorkers and returns a function to stop them and a results channel.
// Callers should close the tasks channel when all producers are done.
func StartWorkerPool(ctx context.Context, numWorkers int, tasks <-chan Task) (results <-chan int, stop func()) {
	resultsCh := make(chan int, 64)
	ctx, cancel := context.WithCancel(ctx)
	var wg sync.WaitGroup

	// start workers
	wg.Add(numWorkers)
	for i := 0; i < numWorkers; i++ {
		go worker(ctx, i, tasks, resultsCh, &wg)
	}

	// stop function: cancel context and wait then close results
	stop = func() {
		cancel()
		// wait for workers to exit and then close resultsCh
		go func() {
			wg.Wait()
			close(resultsCh)
		}()
	}

	return resultsCh, stop
}

// offer attempts to send a task without blocking. returns false if queue is full.
func offer(tasks chan<- Task, t Task) bool {
	select {
	case tasks <- t:
		return true
	default:
		return false
	}
}

func main() {
	ctx := context.Background()
	tasks := make(chan Task, 10) // bounded queue with capacity 10

	// start pool of 4 workers
	results, stop := StartWorkerPool(ctx, 4, tasks)

	// producers: two goroutines producing tasks
	var prodWg sync.WaitGroup
	prodWg.Add(2)
	go func() {
		defer prodWg.Done()
		for i := 0; i < 25; i++ {
			// blocking push (will backpressure if tasks full)
			tasks <- Task{ID: i}
		}
	}()
	go func() {
		defer prodWg.Done()
		for i := 100; i < 125; i++ {
			// non-blocking offer: drop if full
			if !offer(tasks, Task{ID: i}) {
				fmt.Println("dropped task", i)
			}
		}
	}()

	// wait for producers, then close tasks to signal no more work
	prodWg.Wait()
	close(tasks)

	// collect results until pool shuts down and results channel closes
	for id := range results {
		fmt.Println("processed", id)
	}

	// stop the pool (cancels context and cleans up). safe even after tasks closed.
	stop()
	fmt.Println("done")
}
```

Key points in this example:

- `tasks` channel is the queue. Making it buffered gives temporary capacity and backpressure when full.
- Producers can either block on `tasks <- t` or use `offer` to drop when full.
- Closing `tasks` signals workers there will be no more tasks; workers exit and eventually `results` is closed.
- Context cancellation allows immediate shutdown if you need to abort