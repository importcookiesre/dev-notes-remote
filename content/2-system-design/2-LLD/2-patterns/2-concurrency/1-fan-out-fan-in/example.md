
Pipeline with fan-out on a stage, errgroup cancellation, and preserving order

This shows a common production pattern:

- Stage 1: produce tasks
    
- Stage 2: fan-out to workers that do CPU/IO work
    
- Stage 3: collect results and preserve original ordering using index metadata


```go
package main

import (
	"context"
	"fmt"
	"golang.org/x/sync/errgroup"
	"sort"
	"time"
)

type Job struct {
	Idx int
	Payload int
}
type JobResult struct {
	Idx int
	Value int
	Err error
}

func processJob(ctx context.Context, j Job) (int, error) {
	// simulate variable work
	select {
	case <-ctx.Done():
		return 0, ctx.Err()
	case <-time.After(time.Duration(10 + (j.Payload%3)*30) * time.Millisecond):
		// result depends on payload
		return j.Payload * 2, nil
	}
}

func main() {
	ctx := context.Background()
	g, ctx := errgroup.WithContext(ctx)

	numWorkers := 5
	jobCh := make(chan Job, 32)
	resultCh := make(chan JobResult, 32)

	// producer
	g.Go(func() error {
		defer close(jobCh)
		for i := 0; i < 50; i++ {
			select {
			case <-ctx.Done():
				return ctx.Err()
			case jobCh <- Job{Idx: i, Payload: i}:
			}
		}
		return nil
	})

	// fan-out workers
	for w := 0; w < numWorkers; w++ {
		w := w
		g.Go(func() error {
			for j := range jobCh {
				// respect cancellation
				res, err := processJob(ctx, j)
				select {
				case <-ctx.Done():
					return ctx.Err()
				case resultCh <- JobResult{Idx: j.Idx, Value: res, Err: err}:
				}
			}
			return nil
		})
	}

	// collector: closes resultCh when workers finish
	g.Go(func() error {
		// wait for all workers in errgroup to finish
		// when errgroup is done, the go routine returns and we close resultCh outside.
		// But we cannot call g.Wait() here or we deadlock. Instead we run a final monitor goroutine after g.Wait.
		return nil
	})

	// wait for all workers and producer; then close results
	if err := g.Wait(); err != nil {
		// if any worker or producer returned error, cancel and stop
		close(resultCh)
		fmt.Println("errgroup error:", err)
		return
	}
	close(resultCh)

	// collect results and re-order to original sequence
	results := make([]JobResult, 0, 50)
	for r := range resultCh {
		if r.Err != nil {
			fmt.Println("job err:", r.Err)
			continue
		}
		results = append(results, r)
	}

	// reorder by index
	sort.Slice(results, func(i, j int) bool { return results[i].Idx < results[j].Idx })

	for _, r := range results {
		fmt.Printf("idx=%d value=%d\n", r.Idx, r.Value)
	}
}
```

Notes:

- `errgroup` coordinates cancellation: any worker that returns an error cancels the rest.
    
- We attach index metadata to results so we can re-order them at the end.
    
- Reordering uses memory proportional to number of tasks; if the stream is huge, you might stream ordered results via an ordered-emit algorithm that blocks on missing indices.