

## 1. Fan-Out / Fan-In Pattern in Go

**A concurrency pattern where one goroutine _fans out_ work to multiple workers, and then later _fans in_ (collects) the results into a single channel.**

This pattern exists everywhere in backend systems—API servers, microservices, distributed workers, pipelines, ETL jobs, search engines, and more.


![[Pasted image 20251211195441.png]]


## What fan-out / fan-in is

- Fan-out: split work into multiple concurrent workers that consume from the same input source. This increases throughput and parallelism.
- Fan-in: gather results from multiple concurrent workers into a single sink so the rest of the program can consume them in one place.

Together they form a common concurrency topology: one or more producers feed tasks into a queue; N workers (fan-out) process tasks concurrently; those workers send results into a results channel which is merged (fan-in) and consumed downstream.

- Scalability: increase throughput by adding workers.
- Isolation: a slow worker does not block others.
- Decoupling: producers are decoupled from consumers.
- Backpressure: bounded queues let you apply flow control.
- Composition: you can chain stages (pipelines) where each stage fans out and in.


### Important concerns and tradeoffs

- Ordering: fan-out usually breaks input ordering. If order matters, you must reattach sequence metadata and re-order at the sink.
- Backpressure: without bounds, queues can grow unbounded and cause OOM. Use buffered/bounded channels or drop/overflow strategies.
- Error handling: decide whether a single failure cancels all work or is only logged. Use context cancellation or errgroup for coordinated cancellation.
- Resource contention: too many workers increases contention (CPU, DB connections). Tune worker count.
- Fairness: if producers are faster than consumers, tasks may pile up.

## 2.use cases : 

#### **1. API Server Handling Heavy CPU Work**

Example: Generating thumbnails for uploaded images.  
Request → Fan-Out to CPU workers → Aggregate → Respond.

#### **2. Microservices Pipeline**

You receive a batch of 10,000 customer records.  
Fan-Out: multiple goroutines validate and enrich them.  
Fan-In: produce a single final cleaned dataset to push to Kafka/Postgres.

#### **3. Web Scraping**

Fan-Out: Fetch 100 URLs concurrently.  
Fan-In: Combine all responses into one slice.

#### **4. Search Engine Query**

Fan-Out: Query 10 shard servers (or microservices).  
Fan-In: Merge results, sort by ranking, return to user.

#### **5. Analytics Systems**

Fan-Out jobs → perform aggregations → Fan-In to compute summary metrics.




## 3. Example :

```go
package main

import (
	"fmt"
	"sync"
	"time"
)

func worker(id int, jobs <-chan int, results chan<- int, wg *sync.WaitGroup) {
	defer wg.Done()

	for job := range jobs {
		fmt.Printf("Worker %d processing job %d\n", id, job)
		time.Sleep(2 * time.Second)
		results <- job * job
	}
}

func main() {
	jobs := make(chan int)
	results := make(chan int)

	var workerWG sync.WaitGroup
	var resultWG sync.WaitGroup

	// Start workers
	for i := 1; i <= 3; i++ {
		workerWG.Add(1)
		go worker(i, jobs, results, &workerWG)
	}

	// Producer (fan-out)
	go func() {
		for i := 0; i < 10; i++ {
			jobs <- i
		}
		close(jobs)
	}()

	// Fan-in consumer in separate goroutine
	resultWG.Add(1)
	go func() {
		defer resultWG.Done()
		for res := range results {
			fmt.Println("Result:", res)
		}
	}()

	// Close results after workers finish
	go func() {
		workerWG.Wait()
		close(results)
	}()

	fmt.Println("Main orchestrates and waits…")
	resultWG.Wait() // wait only for result consumption

	fmt.Println("Main exits cleanly.")
}

```