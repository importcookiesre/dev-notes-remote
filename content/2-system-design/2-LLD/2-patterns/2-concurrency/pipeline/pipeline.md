[pipelines go blog](https://go.dev/blog/pipelines)

- The pipeline pattern is a concurrency design pattern where data flows through a series of stages, each stage performing a specific transformation  and communicating exclusively via channels.
- Each stage typically runs in its own goroutine, enabling parallelism, clear separation of concerns, and backpressure.



## 1. what is a pipeline ? 
a pipeline is a series of _stages_ connected by channels, where each stage is a group of goroutines running the same function. In each stage, the goroutines

- receive values from _upstream_ via _inbound_ channels
- perform some function on that data, usually producing new values
- send values _downstream_ via _outbound_ channels


## 2. Use cases : 

Pipelines are extremely common in backend engineering:
- Request → validation → authentication → business logic → response
- Log ingestion → parsing → enrichment → storage
- File upload → chunking → compression → encryption → cloud storage
- ETL jobs (Extract → Transform → Load)
- Streaming systems and event processing

![[Pasted image 20251215095349.png]]


## 3. Preventing Leaks : The Done channel

In production, you rarely want a pipeline to run forever or hang if an error occurs. You need a way to stop the "assembly line."

You should pass a `<-chan struct{}` (often called `done`) to every stage. If `done` is closed, all stages should return immediately to prevent **goroutine leaks**.

```go

```


## 4. Parallise the slow stages :


# Example : 


## 1. Squaring numbers : 

Consider a pipeline with three stages.

**Stage 1 :** generate
The first stage, `gen`, is a function that converts a list of integers to a channel that emits the integers in the list. The `gen` function starts a goroutine that sends the integers on the channel and closes the channel when all the values have been sent:

```go
func gen(nums ...int) <-chan int {
    out := make(chan int)
    go func() {
        for _, n := range nums {
            out <- n
        }
        close(out)
    }()
    return out
}
```

**stage 2:** transform
The second stage, `sq`, receives integers from a channel and returns a channel that emits the square of each received integer. After the inbound channel is closed and this stage has sent all the values downstream, it closes the outbound channel:

```go
func sq(in <-chan int) <-chan int {
    out := make(chan int)
    go func() {
        for n := range in {
            out <- n * n
        }
        close(out)
    }()
    return out
}
```


**stage 3:** consume

The `main` function sets up the pipeline and runs the final stage: it receives values from the second stage and prints each one, until the channel is closed:

```go
func main() {
    // Set up the pipeline.
    c := gen(2, 3)
    out := sq(c)

    // Consume the output.
    // Set up the pipeline and consume the output.
    for n := range sq(sq(gen(2, 3))) {
        fmt.Println(n) // 16 then 81
    }
}
```




### 1. parallize the slow stages :

In our previous linear pipeline, if the `square` stage takes 1 second per item, processing 10 items takes 10 seconds. It doesn't matter how fast the CPU is; the pipeline is blocked by that one slow stage.

**The Solution:**

1. **Fan-out:** Spin up multiple instances of the slow stage (workers) to read from the same input channel.
    
2. **Fan-in:** Combine (multiplex) the results from all those workers back into a single channel.


Imagine the assembly line again. The "squaring" station is too slow.

- **Fan-out:** You hire 3 people to do the squaring instead of 1. They all grab items from the _same_ incoming belt as fast as they can.
    
- **Fan-in:** Since you now have 3 people handing you finished items, you need a funnel (or a manager) to put them all back onto one single belt for the next stage.


#### implementation : 

Step A: The Worker (Fan-out capable)
```go
package main

import (
	"fmt"
	"sync"
	"time"
)

// slowSquare simulates a heavy calculation
func slowSquare(id int, in <-chan int) <-chan int {
	out := make(chan int)
	go func() {
		for n := range in {
			// Simulate processing time
			time.Sleep(200 * time.Millisecond) 
			fmt.Printf("Worker %d processing %d\n", id, n)
			out <- n * n
		}
		close(out)
	}()
	return out
}
```

Step B: The Merge Function (Fan-in)
```go
func merge(cs ...<-chan int) <-chan int {
	var wg sync.WaitGroup
	out := make(chan int)

	// Define a small helper to copy values from one channel to 'out'
	output := func(c <-chan int) {
		for n := range c {
			out <- n
		}
		wg.Done() // Signal that this specific channel is done
	}

	wg.Add(len(cs))
	// Start a goroutine for each input channel
	for _, c := range cs {
		go output(c)
	}

	// Start a separate monitor goroutine to close 'out' when all workers finish
	go func() {
		wg.Wait()
		close(out)
	}()

	return out
}
```

Step C: Wiring it up (Main)
```go
func generator(nums ...int) <-chan int {
	out := make(chan int)
	go func() {
		for _, n := range nums {
			out <- n
		}
		close(out)
	}()
	return out
}

func main() {
	// 1. Create the input stream
	input := generator(1, 2, 3, 4, 5, 6, 7, 8, 9, 10)

	// 2. FAN-OUT: Start 3 workers reading from the SAME input channel
	// Because 'input' is a channel, Go handles the race conditions automatically.
	// Only one worker will grab a specific item.
	c1 := slowSquare(1, input)
	c2 := slowSquare(2, input)
	c3 := slowSquare(3, input)

	// 3. FAN-IN: Merge the results back into one channel
	finalOutput := merge(c1, c2, c3)

	// 4. Consume the final results
	for n := range finalOutput {
		fmt.Printf("Result: %d\n", n)
	}
}
```


### 2. worker pool implementation : 

**Worker Pool Pattern**, is the "production-grade" version of Fan-out/Fan-in.
**How would it be?** Instead of creating 3 separate channels and merging them, you create **one** input channel and **one** output channel. You launch $N$ workers that all share these same two channels.


```go
package main

import (
	"fmt"
	"sync"
	"time"
)

// 1. The Worker Function
// Notice: It doesn't create a channel. It accepts existing ones.
func worker(id int, jobs <-chan int, results chan<- int, wg *sync.WaitGroup) {
	defer wg.Done() // Signal "I'm done" when this function exits

	for n := range jobs {
		fmt.Printf("Worker %d started job %d\n", id, n)
		time.Sleep(200 * time.Millisecond) // Simulate work
		results <- n * n
	}
}

func main() {
	const numJobs = 10
	const numWorkers = 3

	// 2. Create shared channels
	// Buffered channels are CRITICAL here to prevent blocking!
	jobs := make(chan int, numJobs)
	results := make(chan int, numJobs)

	// 3. Start the Worker Pool
	var wg sync.WaitGroup
	
	// Spawn N workers
	for w := 1; w <= numWorkers; w++ {
		wg.Add(1)
		go worker(w, jobs, results, &wg)
	}

	// 4. Send jobs (The Producer)
	for j := 1; j <= numJobs; j++ {
		jobs <- j
	}
	close(jobs) // We must close 'jobs' so workers know when to stop!

	// 5. Wait for workers to finish (The Closer)
	// We do this in a separate goroutine so we don't block the main thread from reading results
	go func() {
		wg.Wait()
		close(results)
	}()

	// 6. Collect results (The Consumer)
	for res := range results {
		fmt.Printf("Result: %d\n", res)
	}
}
```


#### A Critical Detail: Buffered Channels

In the code above, I used `make(chan int, numJobs)`.

If you use **unbuffered** channels (`make(chan int)`), the "Producer" loop (step 4) will block after sending the first job until a worker picks it up. If all workers are busy, the main thread halts.

By **buffering** the channel, you allow the Producer to dump all the jobs into the "queue" instantly and move on, while the workers pick them off at their own pace. This decouples the submission of work from the execution of work.

### 2. explicit cancellation

## 2. Image processing : 

Folder structure
```bash
├── cmd
│   └── main
│       └── main.go
├── go.mod
├── go.sum
├── images
│   ├── image1.jpg
│   ├── image2.jpg
│   ├── image3.jpg
│   ├── image4.jpg
│   └── output
│       ├── image1.jpg
│       ├── image2.jpg
│       ├── image3.jpg
│       └── image4.jpg
└── internal
    └── image_processing
        └── image_processing.go
```

main : 
```go
package main

import (
	"fmt"
	"image"
	"strings"
	imageprocessing "suraj/internal/image_processing"
)

type Job struct {
	InputPath string
	Image     image.Image
	OutPath   string
}

// stage1 : load image
func loadImage(paths []string) <-chan Job {
	out := make(chan Job)

	go func() {
		defer close(out)
		for _, p := range paths {
			job := Job{
				InputPath: p,
				OutPath:   strings.Replace(p, "images/", "images/output/", 1),
				Image:     imageprocessing.ReadImage(p),
			}
			out <- job
		}
	}()
	return out
}

// stage 2: resize image
func resize(inp <-chan Job) <-chan Job {
	out := make(chan Job)
	go func() {
		defer close(out)
		for job := range inp {
			job.Image = imageprocessing.Resize(job.Image)
			out <- job
		}
	}()
	return out
}

// stage 3 :  convert to grayscale image
func convertToGrayscale(input <-chan Job) <-chan Job {
	out := make(chan Job)
	go func() {
		defer close(out)
		for job := range input {
			job.Image = imageprocessing.Grayscale(job.Image)
			out <- job
		}
	}()
	return out
}

// stage 4: save image
func saveImage(inp <-chan Job) <-chan bool {
	out := make(chan bool)
	go func() {
		defer close(out)
		for job := range inp {
			imageprocessing.WriteImage(job.OutPath, job.Image)
			out <- true
		}

	}()

	return out
}

func main() {
	imagePaths := []string{
		"images/image1.jpg",
		"images/image2.jpg",
		"images/image3.jpg",
		"images/image4.jpg",
	}

	channel1 := loadImage(imagePaths)
	channel2 := resize(channel1)
	channel3 := convertToGrayscale(channel2)
	resultChan := saveImage(channel3)

	for res := range resultChan {
		if res {
			fmt.Println("success")
		} else {
			fmt.Println("failed")
		}
	}

}

```

```go
package imageprocessing

import (
	"fmt"
	"image"
	"image/color"
	"image/jpeg"
	"os"

	"github.com/nfnt/resize"
)

func ReadImage(path string) image.Image {
	inputFile, err := os.Open(path)
	if err != nil {
		panic(err)
	}
	defer inputFile.Close()

	// Decode the image
	img, _, err := image.Decode(inputFile)
	if err != nil {
		fmt.Println(path)
		panic(err)
	}
	return img
}

func WriteImage(path string, img image.Image) {
	outputFile, err := os.Create(path)
	if err != nil {
		panic(err)
	}
	defer outputFile.Close()

	// Encode the image to the new file
	err = jpeg.Encode(outputFile, img, nil)
	if err != nil {
		panic(err)
	}
}

func Grayscale(img image.Image) image.Image {
	// Create a new grayscale image
	bounds := img.Bounds()
	grayImg := image.NewGray(bounds)

	// Convert each pixel to grayscale
	for y := bounds.Min.Y; y < bounds.Max.Y; y++ {
		for x := bounds.Min.X; x < bounds.Max.X; x++ {
			originalPixel := img.At(x, y)
			grayPixel := color.GrayModel.Convert(originalPixel)
			grayImg.Set(x, y, grayPixel)
		}
	}
	return grayImg
}

func Resize(img image.Image) image.Image {
	newWidth := uint(500)
	newHeight := uint(500)
	resizedImg := resize.Resize(newWidth, newHeight, img, resize.Lanczos3)
	return resizedImg
}
```