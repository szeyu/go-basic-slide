---
layout: section
---

# Introduction to Concurrency

---
layout: two-cols
---

# Understanding Goroutines

<v-clicks>

## What are Goroutines?

- Lightweight threads managed by the Go runtime
- Much cheaper than OS threads (can create thousands)
- Allow concurrent execution of functions
- Multiplexed onto a smaller number of OS threads
- Starts with the `go` keyword

</v-clicks>

<div class="mt-8">
<v-clicks>

## Basic Goroutine Example

```go {all|1-3|5-8|10-11|13-14|all}
func sayHello() {
    fmt.Println("Hello from goroutine!")
}

func main() {
    // Start a goroutine
    go sayHello()
    
    // Need to wait, otherwise main might exit before goroutine runs
    time.Sleep(100 * time.Millisecond)
    
    fmt.Println("Hello from main!")
}
```

</v-clicks>
</div>

---
layout: two-cols
---

# Working with Goroutines (1/2)

<v-clicks>

## Anonymous Function Goroutines

```go {all|1-4|6-9|all}
// Anonymous function as a goroutine
go func() {
    fmt.Println("Hello from anonymous goroutine!")
}()

// With parameters
go func(msg string) {
    fmt.Println(msg)
}("Hello with parameter")
```

</v-clicks>

::right::

<div class="ml-4">
<v-clicks>

## Common Patterns

```go {all|1-5|7-11|all}
// Processing items concurrently
items := []int{1, 2, 3, 4, 5}
for _, item := range items {
    go processItem(item)
}

// Limiting concurrency
const maxConcurrent = 3
sem := make(chan struct{}, maxConcurrent)
// ... process with semaphore pattern
// (we'll see this with channels)
```

</v-clicks>
</div>

---
layout: two-cols
---

# Working with Goroutines (2/2)

<v-clicks>

## Waiting for Goroutines

```go {all|1-3|5-10|12-13|all}
// Using WaitGroup to wait for completion
var wg sync.WaitGroup

for i := 1; i <= 5; i++ {
    wg.Add(1)  // Increment counter
    go func(n int) {
        defer wg.Done()  // Decrement counter when done
        fmt.Println("Worker", n)
    }(i)
}

// Wait for all goroutines to finish
wg.Wait()
```

</v-clicks>

::right::

<div class="ml-4">
<v-clicks>

## Goroutine Cautions
- Goroutines need coordination (channels/sync)
- Shared memory requires synchronization
- Goroutines can leak if never terminated
- Main function doesn't wait for goroutines

</v-clicks>
</div>

---
layout: default
---

# Channels Basics

<v-clicks>

## What are Channels?

- Pipes that connect concurrent goroutines
- Allow goroutines to communicate and synchronize
- Type-safe: channels can only transport values of a specific type
- Provide built-in synchronization

</v-clicks>

<div class="mt-8">
<v-clicks>

## Creating and Using Channels

```go {all|1-2|4-7|9-10|all}
// Create an unbuffered channel
ch := make(chan string)

// Send value to channel (blocks until someone receives)
go func() {
    ch <- "Hello"  // Send to channel
}()

// Receive value from channel (blocks until someone sends)
msg := <-ch
fmt.Println(msg)  // Hello
```

</v-clicks>
</div>

---
layout: two-cols
---

# Channel Types

<v-clicks>

## Unbuffered Channels

```go {all|1-2|4-7|9-12|all}
// Create an unbuffered channel
ch := make(chan int)

// Sender blocks until receiver is ready
go func() {
    ch <- 42  // Blocks until someone receives
}()

// Receiver blocks until sender sends
go func() {
    value := <-ch  // Blocks until someone sends
}()
```

## Characteristics
- Synchronous communication
- Sender and receiver must be ready at the same time
- Provides guaranteed delivery
- Useful for synchronization between goroutines

</v-clicks>

::right::

<div class="ml-4">
<v-clicks>

## Buffered Channels

```go {all|1-2|4-7|9-12|all}
// Create a buffered channel with capacity 2
ch := make(chan string, 2)

// Send values (won't block until buffer is full)
ch <- "Hello"   // Doesn't block
ch <- "World"   // Doesn't block
// ch <- "More"  // Would block until space is available

// Receive values
fmt.Println(<-ch)  // Hello
fmt.Println(<-ch)  // World
```

## Characteristics
- Asynchronous communication up to buffer size
- Sender only blocks when buffer is full
- Receiver blocks when buffer is empty
- Useful when processing rate differs between sender/receiver

</v-clicks>
</div>

---
layout: default
---

# Channel Operations (1/2)

<v-clicks>

## Closing Channels

```go {all|1-5|7-13|15-17|all}
// Sender closes channel when done sending
ch := make(chan int, 3)
ch <- 1
ch <- 2
ch <- 3
close(ch)

// Receiver can check if channel is closed
for {
    value, ok := <-ch
    if !ok {
        break  // Channel is closed
    }
    fmt.Println(value)
}

// Simpler way to receive all values until channel is closed
for value := range ch {
    fmt.Println(value)
}
```

</v-clicks>

---
layout: default
---

# Channel Operations (2/2)

<v-clicks>

## Channel Direction

```go {all|1-3|5-7|9-11|all}
// Send-only channel parameter
func send(ch chan<- int) {
    ch <- 42
}

// Receive-only channel parameter
func receive(ch <-chan int) {
    val := <-ch
}

// Bidirectional channel
func both(ch chan int) {
    ch <- 42
    val := <-ch
}
```

</v-clicks>

---
layout: two-cols
---

# Select Statement (1/2)

<v-clicks>

## Waiting on Multiple Channels

```go {all|1-4|all}
// Create two channels
ch1 := make(chan string)
ch2 := make(chan string)
```

```go {all|1-5|6-10|all}
// Send values after delays
go func() {
    time.Sleep(1 * time.Second)
    ch1 <- "one"
}()

go func() {
    time.Sleep(2 * time.Second)
    ch2 <- "two"
}()
```

</v-clicks>

::right::

<div class="ml-4">
<v-clicks>

```go {all|1-6|7-11|all}
select {
case msg1 := <-ch1:
    fmt.Println("Received", msg1)
case msg2 := <-ch2:
    fmt.Println("Received", msg2)
case <-time.After(3 * time.Second):
    fmt.Println("Timeout!")
default:
    fmt.Println("No message available")
}
```

</v-clicks>
</div>

---
layout: default
---

# Select Statement (2/2)

<v-clicks>

## Common Select Patterns

```go {all|1-7|9-15|all}
// Non-blocking channel operations
select {
case msg := <-ch:
    fmt.Println("Received:", msg)
default:
    fmt.Println("No message available")
}

// Timeout pattern
select {
case result := <-ch:
    fmt.Println("Received:", result)
case <-time.After(2 * time.Second):
    fmt.Println("Operation timed out")
}
```

</v-clicks>