---
layout: section
---

# Concurrency in Go

---
layout: two-cols
---

# What are Goroutines?

<v-clicks>

- Lightweight threads managed by the Go runtime
- Much cheaper than OS threads (thousands can run simultaneously)
- Created with the `go` keyword before a function call
- Perfect for concurrent tasks like data fetching or processing files

</v-clicks>

::right::

<div class="ml-4 mt-12">
<v-clicks>

## The Coffee Shop Example

```go
// Brew coffee in a goroutine
func brewCoffee(coffee string, prepTime time.Duration) {
    fmt.Printf("Barista started brewing %s, will take %v\n", 
        coffee, prepTime)
    time.Sleep(prepTime)
    fmt.Printf("Barista finished brewing %s, ready to serve!\n", 
        coffee)
}
```

</v-clicks>
</div>

---
layout: two-cols
---

# Basic Goroutines in Action

<v-clicks>

## Running Concurrent Tasks

```go
func main() {
    fmt.Println("☕ Coffee Shop: Basic Goroutines")
    
    // Launch three concurrent baristas
    go brewCoffee("Espresso", 50*time.Millisecond)
    go brewCoffee("Latte", 100*time.Millisecond)
    go brewCoffee("Cappuccino", 150*time.Millisecond)
    
    // Wait for all to finish (not ideal!)
    time.Sleep(200 * time.Millisecond)
    fmt.Println("Coffee shop closing")
}
```

</v-clicks>

::right::

<div class="ml-4 mt-12">
<v-clicks>

## Sample Output

```
☕ Coffee Shop: Basic Goroutines
Barista started brewing Espresso, will take 50ms
Barista started brewing Latte, will take 100ms
Barista started brewing Cappuccino, will take 150ms
Barista finished brewing Espresso, ready to serve!
Barista finished brewing Latte, ready to serve!
Barista finished brewing Cappuccino, ready to serve!
Coffee shop closing
```

</v-clicks>
</div>

---
layout: two-cols
---

# Goroutine Key Insights

<v-clicks>

- The `go` keyword launches each function concurrently
- Goroutines run in parallel (all baristas work at once)
- Main continues immediately after launching goroutines
- Using `time.Sleep` to wait is not ideal (we'll improve this)

</v-clicks>

::right::

<div class="ml-4 mt-12">
<v-clicks>

## Visualization

```
Main: |------------------->
      |
Espresso: |=======>
Latte:    |===========>
Cappuccino: |===============>
            |
            |--> time
```

- Each barista brews in its own goroutine
- Baristas work simultaneously
- Completion order depends on brew times

</v-clicks>
</div>

---
layout: two-cols
---

# What is WaitGroup?

<v-clicks>

- Part of the `sync` package
- Acts like a counter to track active goroutines
- Provides a way to wait for all goroutines to finish
- No need for arbitrary `time.Sleep()` calls

</v-clicks>

::right::

<div class="ml-4 mt-12">
<v-clicks>

## The Bakery Example: Setup

```go
// Bake pastry in a goroutine
func bakePastry(pastry string, wg *sync.WaitGroup) {
    // Ensure counter is decremented when function completes
    defer wg.Done()
    
    fmt.Printf("Baker started preparing %s, will take 100ms\n", 
        pastry)
    time.Sleep(100 * time.Millisecond)
    fmt.Printf("Baker finished preparing %s, pastry ready!\n", 
        pastry)
}
```

</v-clicks>
</div>

---
layout: two-cols
---

# WaitGroup in Action

<v-clicks>

## Managing Goroutines

```go
func main() {
    fmt.Println("🥐 Bakery: Goroutines with WaitGroup")
    var wg sync.WaitGroup
    pastries := []string{"Croissant", "Muffin", "Scone"}
    
    fmt.Printf("Manager: Preparing to bake %d pastries\n", 
        len(pastries))
    
    for i, pastry := range pastries {
        // Increment counter before launching goroutine
        wg.Add(1)
        fmt.Printf("Manager: Assigning baker %d to %s\n", 
            i+1, pastry)
        go bakePastry(pastry, &wg)
    }
    
    fmt.Println("Manager: Waiting for all bakers to finish")
    // Block until counter reaches zero
    wg.Wait()
    fmt.Println("Manager: All pastries ready, bakery closing")
}
```

</v-clicks>

::right::

<div class="ml-4 mt-12">
<v-clicks>

## WaitGroup Steps

1. Create a `sync.WaitGroup`
2. Call `wg.Add(1)` before each goroutine launch
3. Call `wg.Done()` when goroutine completes (with `defer`)
4. Main goroutine calls `wg.Wait()` to block until done
5. When counter reaches zero, execution continues

</v-clicks>
</div>

---
layout: default
---

# WaitGroup Output

<v-clicks>

## Sample Output

```
🥐 Bakery: Goroutines with WaitGroup
Manager: Preparing to bake 3 pastries
Manager: Assigning baker 1 to Croissant
Manager: Assigning baker 2 to Muffin
Manager: Assigning baker 3 to Scone
Manager: Waiting for all bakers to finish
Baker started preparing Scone, will take 100ms
Baker started preparing Croissant, will take 100ms
Baker started preparing Muffin, will take 100ms
Baker finished preparing Croissant, pastry ready!
Baker finished preparing Scone, pastry ready!
Baker finished preparing Muffin, pastry ready!
Manager: All pastries ready, bakery closing
```

</v-clicks>

---
layout: two-cols
---

# What are Channels?

<v-clicks>

- Communication pipes between goroutines
- Provide safe data exchange (prevent data races)
- Type-safe: only specific data types can be sent
- Created with `make(chan Type)`

## Channel Operations

```go
// Create a channel
ch := make(chan string)

// Send value to channel (blocks until received)
ch <- "Hello"  

// Receive value from channel (blocks until sent)
msg := <-ch

// Close a channel (sender should do this)
close(ch)
```

</v-clicks>

::right::

<div class="ml-4 mt-12">
<v-clicks>

## More Channel Operations

```go
// Check if channel is closed during receive
value, ok := <-ch  // ok is false if channel closed

// Range over channel until closed
for value := range ch {
    // Process value
}
```

## Unbuffered Channels

- Like a direct handoff
- The sender blocks until receiver is ready
- The receiver blocks until sender sends
- Perfect for synchronization

</v-clicks>
</div>

---
layout: two-cols
---

# The Food Truck Example

<v-clicks>

## Channel Example Setup

```go
// Send orders to a channel
func sendOrders(ch chan string) {
    orders := []string{"Burger", "Taco", "Salad"}
    
    for i, order := range orders {
        fmt.Printf("Chef: Preparing to send order %d: %s\n", 
            i+1, order)
        // Send order to channel
        ch <- order
        fmt.Printf("Chef: Sent order %d: %s\n", 
            i+1, order)
    }
    
    fmt.Println("Chef: All orders sent, closing channel")
    close(ch)
}
```

</v-clicks>

::right::

<div class="ml-4 mt-12">
<v-clicks>

## Using the Channel

```go
func main() {
    fmt.Println("🌮 Food Truck: Channels")
    
    // Create an unbuffered channel
    ch := make(chan string)
    
    // Launch goroutine to send orders
    go sendOrders(ch)
    
    // Range over channel until it's closed
    for order := range ch {
        fmt.Printf("Server: Received order: %s\n", order)
    }
    
    fmt.Println("Manager: All orders served")
}
```

</v-clicks>
</div>

---
layout: two-cols
---

# Channel Output and Insights

<v-clicks>

## Sample Output

```
🌮 Food Truck: Channels
Manager: Setting up order channel
Manager: Starting chef to send orders
Manager: Server receiving orders
Chef: Preparing to send order 1: Burger
Chef: Sent order 1: Burger
Server: Received order: Burger
Chef: Preparing to send order 2: Taco
Chef: Sent order 2: Taco
Server: Received order: Taco
Chef: Preparing to send order 3: Salad
Chef: Sent order 3: Salad
Server: Received order: Salad
Chef: All orders sent, closing channel
Manager: All orders served, food truck closing
```

</v-clicks>

::right::

<div class="ml-4 mt-12">
<v-clicks>

## Key Insights

- "Sent" log appears only after "Received" because sender blocks
- We range over the channel until it's closed by the sender
- Channel operations handle synchronization automatically
- No explicit WaitGroup needed with this pattern

## Channel Guarantees

- Channels prevent data races
- Only one goroutine can send/receive at a time
- Close signal is broadcast to all receivers
- Memory is properly synchronized between goroutines

</v-clicks>
</div>

---
layout: two-cols
---

# Worker Pool: Introduction

<v-clicks>

## What is a Worker Pool?

- Multiple workers (goroutines) process jobs from a shared queue
- Buffered channels manage job distribution and results collection
- Ideal for CPU-intensive tasks or batch processing
- Scales work across available CPU cores

## Key Components

1. **Jobs Channel**: Distributes work to available workers
2. **Results Channel**: Collects processed results 
3. **Worker Goroutines**: Process jobs concurrently
4. **Channel Closing**: Signals completion to workers

</v-clicks>

::right::

<div class="ml-4 mt-12">
<v-clicks>

## When to Use Worker Pools

- Processing large datasets in parallel
- Handling multiple concurrent requests
- CPU-bound computations
- I/O operations that can run concurrently
- Batch processing tasks

## Benefits

- Limits number of concurrent goroutines
- Controls resource usage
- Provides load balancing
- Improves throughput
- Simplifies task management

</v-clicks>
</div>

---
layout: two-cols
---

# Worker Pool: Pizza Shop

<v-clicks>

## Worker Function

```go
func cookPizza(id int, jobs <-chan int, results chan<- int) {
    fmt.Printf("Cook %d ready to take orders\n", id)
    
    // Process all jobs in the channel
    for order := range jobs {
        fmt.Printf("Cook %d received order %d\n", id, order)
        fmt.Printf("Cook %d cooking order %d\n", id, order)
        
        // Simulate work with sleep
        time.Sleep(200 * time.Millisecond)
        
        // Calculate result (just double the order number)
        result := order * 2
        fmt.Printf("Cook %d finished order %d, result: %d\n", 
                id, order, result)
                  
        // Send result back through results channel
        results <- result
    }
    
    fmt.Printf("Cook %d done, no more orders\n", id)
}
```

</v-clicks>

::right::

<div class="ml-4 mt-12">
<v-clicks>

## Implementation

```go
func main() {
    numJobs := 5
    numWorkers := 3
    
    // Create buffered channels
    jobs := make(chan int, numJobs)
    results := make(chan int, numJobs)
    
    // Start worker goroutines
    for w := 1; w <= numWorkers; w++ {
        go cookPizza(w, jobs, results)
    }
    
    // Send all jobs
    for j := 1; j <= numJobs; j++ {
        fmt.Printf("Sending order %d\n", j)
        jobs <- j
    }
    
    // Close jobs channel - signals workers 
    // that no more jobs are coming
    close(jobs)
    
    // Collect all results
    for i := 1; i <= numJobs; i++ {
        result := <-results
        fmt.Printf("Received result: %d\n", result)
    }
}
```

</v-clicks>
</div>

---
layout: two-cols
---

# Worker Pool: Flow

<v-clicks>

## Order of Operations

1. Create buffered channels for jobs and results
2. Start worker goroutines 
3. Send all jobs to the jobs channel
4. Close the jobs channel to signal no more work
5. Collect all results from the results channel
6. Workers auto terminate when jobs channel closes

## Communication Pattern

```
Main       Jobs Channel      Workers        Results Channel
  |              |              |                 |
  |--jobs-->-----|--job 1-->---Worker 1           |
  |              |              |                 |
  |              |--job 2-->---Worker 2           |
  |              |              |                 |
  |              |--job 3-->---Worker 3           |
  |              |              |                 |
  |              |              |-----results--->-|---->results
```

</v-clicks>

::right::

<div class="ml-4 mt-12">
<v-clicks>

## Real-world Applications

- Web servers processing multiple requests
- Data processing pipelines
- Image/video processing
- Background task execution
- Batch processing operations

## Performance Considerations

- Optimal number often depends on:
  - Number of CPU cores
  - I/O vs CPU bound tasks
  - Memory constraints

</v-clicks>
</div>

---
layout: two-cols
---

# Concurrency Best Practices: Do's and Don'ts

<v-clicks>

## Do's

- **Use WaitGroup** to wait for goroutines to finish
- **Close channels** from the sender, not the receiver
- **Check if a channel is closed** with `val, ok := <-ch`
- **Use buffered channels** for work queues
- **Use context** for cancellation and timeouts

</v-clicks>

::right::

<div class="ml-4 mt-12">
<v-clicks>

## Don'ts

- **Don't** create goroutines without a way to track them
- **Don't** share memory without synchronization
- **Don't** send on a closed channel (will panic)
- **Don't** close a channel from the receiver
- **Don't** overuse goroutines for tiny tasks

## Design Patterns

- **Fan-out**: Distribute work to multiple workers
- **Fan-in**: Collect results from multiple workers
- **Pipeline**: Chain multiple processing stages
- **Timeouts**: Add deadlines to operations
- **Cancellation**: Stop ongoing work when no longer needed

</v-clicks>
</div>