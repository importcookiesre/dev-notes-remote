
## Key Concepts

1. **Exponential Backoff:** When a job fails, we wait for a duration calculated as $wait = base \times 2^{retries}$. This prevents overwhelming a struggling service with immediate retries.
    
2. **Non-Blocking Retries:** We do not want the worker to sleep (block) while waiting for the backoff time. Instead, we spawn a separate "waiter" goroutine that sleeps and then pushes the job back into the queue.
    
3. **Dead Letter Queue (DLQ):** If a job exceeds the maximum retry count, we move it to a separate channel (DLQ) for manual inspection, rather than deleting it.



```go
package main

import (
	"fmt"
	"math"
	"math/rand"
	"sync"
	"time"
)

// 1. Job definition with metadata for retries
type Job struct {
	ID         int
	Payload    string
	RetryCount int
}

const (
	MaxRetries = 3
	BaseBackoff = 500 * time.Millisecond // Start with 0.5s wait
)

// 2. The Processor (Simulates work that might fail)
// Returns an error randomly to simulate instability
func processJob(job Job) error {
	// Simulate 20% chance of failure
	if rand.Float32() < 0.2 {
		return fmt.Errorf("network timeout")
	}
	// Simulate processing time
	time.Sleep(100 * time.Millisecond)
	return nil
}

func worker(id int, jobs chan Job, dlq chan<- Job, wg *sync.WaitGroup) {
	defer wg.Done()

	for job := range jobs {
		fmt.Printf("[Worker %d] Processing Job %d (Attempt %d)\n", id, job.ID, job.RetryCount+1)

		err := processJob(job)
		if err != nil {
			fmt.Printf("[Worker %d] Failed Job %d: %s\n", id, job.ID, err)
			handleFailure(job, jobs, dlq)
		} else {
			fmt.Printf("[Worker %d] SUCCESS Job %d\n", id, job.ID)
		}
	}
}

// 3. Retry Logic
func handleFailure(job Job, jobs chan Job, dlq chan<- Job) {
	if job.RetryCount < MaxRetries {
		job.RetryCount++
		
		// Calculate exponential backoff: 500ms * 2^retries
		// Attempt 1 fail -> wait 1s (if retrycount starts at 0 and becomes 1)
		backoffDuration := BaseBackoff * time.Duration(math.Pow(2, float64(job.RetryCount)))
		
		fmt.Printf("-> Retrying Job %d in %v\n", job.ID, backoffDuration)

		// VITAL: Do not block the worker! Launch a separate goroutine to wait.
		go func() {
			time.Sleep(backoffDuration)
			// Push back into the main queue
			jobs <- job
		}()
	} else {
		// 4. Dead Letter Queue logic
		fmt.Printf("!! Job %d reached MaxRetries. Moving to DLQ.\n", job.ID)
		dlq <- job
	}
}

func main() {
	jobs := make(chan Job, 100)
	dlq := make(chan Job, 100) // Channel for failed jobs
	var wg sync.WaitGroup

	// Start Workers
	for w := 1; w <= 3; w++ {
		wg.Add(1)
		go worker(w, jobs, dlq, &wg)
	}

	// Send Jobs
	fmt.Println("--- Enqueuing Jobs ---")
	for i := 1; i <= 10; i++ {
		jobs <- Job{ID: i, Payload: "Data"}
	}

	// Wait a bit to let processing happen (for demo purposes)
	// In a real app, you would keep running or wait for a signal.
	time.Sleep(5 * time.Second)
	
	close(jobs)
	wg.Wait()
	close(dlq)

	// Check DLQ
	fmt.Println("\n--- DLQ Report ---")
	for failedJob := range dlq {
		fmt.Printf("Permanently failed: Job %d\n", failedJob.ID)
	}
}
```