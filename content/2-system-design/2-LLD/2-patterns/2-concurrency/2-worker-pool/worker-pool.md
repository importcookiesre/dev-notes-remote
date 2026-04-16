- learn about buffered worker pool

- write a blog on this
- read the pipeline pattern
## 1. What Is a Worker Pool?

A **worker pool** is a _concurrency pattern_ where:
- You have a **fixed number of workers** (goroutines),
- A **shared job queue** (channel),
- Workers continuously take tasks from the queue and process them,
- When done, they pick the next task until the queue closes or the program stops.


## 2. Problem it solves :
- Worker pools solve **controlled concurrency**, needed in backend systems:
- Resource management.
- 

### Use cases : 

| Scenario                                | Explanation                                 |
| --------------------------------------- | ------------------------------------------- |
| API processing heavy workloads          | Control goroutines per incoming request     |
| Sending emails/batch jobs               | Avoid spinning thousands of goroutines      |
| Processing messages from Kafka/RabbitMQ | Parallel work but bounded by pool size      |
| Image/Video transcoding services        | Expensive tasks need limits                 |
| Database-heavy workloads                | Prevent DB overload by limiting concurrency |
| Rate-limited external APIs              | Ensure you don’t exceed allowed request/sec |

## 3. Benefits : 

- Prevents memory exhaustion.
- Smooth performance under heavy load.
- Predictable resource usage.
- Better error handling and graceful shutdown.



## 4. handling worker manually using merge():




## 4. Example : 


- here we initialize a pool of 3 worker threads / goroutines  which handles the load.
- It takes jobs from a common channel and perform action, each job takes some time to perform.
- At one time only 3 jobs can be handled, the rest jobs has to stand in queue.
- A worker takes a new job as soon as it becomes free.
- `close(jobs)` breaks the for range loop and it exit the goroutines.
- concurrency is controlled by `numWorkers`.

```go
package main

import (
	"fmt"
	"sync"
	"time"
)

// Job represents the task to be processed
type Job struct {
	ID int
}

// Worker function that continuously receives jobs
func worker(id int, jobs <-chan Job, wg *sync.WaitGroup) {
	defer wg.Done()

	for job := range jobs {
		// Simulate processing
		time.Sleep(1 * time.Second)
		fmt.Printf("Worker %d processing job %d\n", id, job.ID)
	}
}

func main() {
	const numWorkers = 3 // size of the worker pool

	jobs := make(chan Job) // non-buffered queue for tasks
	var wg sync.WaitGroup
	start := time.Now()

	// Create workers
	for i := 1; i <= numWorkers; i++ {
		wg.Add(1)
		go worker(i, jobs, &wg)
	}

	// Add jobs to the queue
	for j := 1; j <= 20; j++ {
		jobs <- Job{ID: j}
	}

	close(jobs) // no more jobs will be sent
	wg.Wait()   // wait for all workers to finish
	fmt.Println("Task complete : ", time.Since(start))
}
```

