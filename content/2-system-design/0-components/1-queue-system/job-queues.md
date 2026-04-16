

- job/task queue

### What is a Job Queue?

A **Job Queue** is a mechanism to decouple the creation of a task (Producer) from its execution (Consumer). In Go, we typically implement in-memory job queues using **Buffered Channels** and the **Worker Pool pattern**.

**Why use it?**

- **Throttling:** Limit the number of concurrent tasks (e.g., only 5 database writes at a time) to prevent crashing your system.
    
- **Asynchronous Processing:** Return a response to the user immediately while the heavy lifting (sending emails, image processing) happens in the background.

### Code Example: The Worker Pool

In this example, we will simulate a system that processes image resizing jobs. We will start a fixed number of workers (goroutines) to listen to a `jobs` channel.
```go
package main

import (
	"fmt"
	"math/rand"
	"sync"
	"time"
)

// 1. Define the Job structure (the data needed to process)
type Job struct {
	ID       int
	ImageURL string
}

// 2. Define the Result structure (optional, if you need to track output)
type Result struct {
	JobID  int
	Status string
}

// 3. The Worker Function
// It continuously reads from the `jobs` channel until it's closed.
func worker(workerID int, jobs <-chan Job, results chan<- Result, wg *sync.WaitGroup) {
	defer wg.Done() // Signal that this worker is done when the function exits

	for job := range jobs {
		fmt.Printf("Worker %d: Started job %d (%s)\n", workerID, job.ID, job.ImageURL)
		
		// Simulate processing time (e.g., resizing an image)
		time.Sleep(time.Duration(rand.Intn(1000)) * time.Millisecond)

		fmt.Printf("Worker %d: Finished job %d\n", workerID, job.ID)
		
		// Send the result
		results <- Result{JobID: job.ID, Status: "Success"}
	}
}

func main() {
	const numJobs = 10
	const numWorkers = 3

	// Create buffered channels
	// 'jobs' buffer allows producers to push without blocking immediately
	jobs := make(chan Job, numJobs)
	results := make(chan Result, numJobs)

	// WaitGroup to ensure all workers finish before we exit
	var wg sync.WaitGroup

	// 4. Start the Workers (The Consumers)
	for w := 1; w <= numWorkers; w++ {
		wg.Add(1)
		go worker(w, jobs, results, &wg)
	}

	// 5. Send Jobs (The Producer)
	for j := 1; j <= numJobs; j++ {
		jobs <- Job{
			ID:       j,
			ImageURL: fmt.Sprintf("https://example.com/img_%d.jpg", j),
		}
	}

	// 6. Close the jobs channel
	// This signals to the workers that no more jobs are coming.
	close(jobs)

	// 7. Wait for all workers to finish
	wg.Wait()
	
	// Close results channel now that workers are done writing to it
	close(results)

	// (Optional) Process results
	for res := range results {
		fmt.Printf("Result received for Job %d: %s\n", res.JobID, res.Status)
	}
}
```


## 2. Learn about Job Queue with retry/backoff

[[job-queues-with-retry-backoff]]