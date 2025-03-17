---
layout: section
---

# Functions in Go

---
layout: default
---

# Basic Function Syntax

<v-clicks>

## No Parameters, No Return Value
```go {all|1|2-4|all}
// Function with no parameters and no return value
func sayHello() {
    fmt.Println("Hello, Go!")
}
```

## With Parameters
```go {all|1|2-4|all}
// Function with parameters
func greet(name string) {
    fmt.Printf("Hello, %s!\n", name)
}
```

## With Return Value
```go {all|1|2-4|all}
// Function with return value
func add(a, b int) int {
    return a + b
}
```

</v-clicks>

---
layout: default
---

# Multiple Return Values

<v-clicks>

## Function Returning Multiple Values
```go {all|1-2|3-5|6-7|all}
// Function returning multiple values
func divide(a, b float64) (float64, error) {
    if b == 0 {
        return 0, errors.New("cannot divide by zero")
    }
    return a / b, nil
}
```

## Using the Function
```go {all|1-2|3-5|6-8|all}
// Using the function
result, err := divide(10, 2)
if err != nil {
    fmt.Println("Error:", err)
} else {
    fmt.Println("Result:", result)
}
```

</v-clicks>

---
layout: two-cols
---

# Named Return Values

<v-clicks>

```go {all|1-2|3-4|5|all}
func calculate(width, height float64) (area, perimeter float64) {
    area = width * height
    perimeter = 2 * (width + height)
    return // naked return - returns named variables
}
```

## Benefits
- Self-documenting
- Pre-initialized to zero values
- Useful for deferred functions
- Cleaner code in some cases

</v-clicks>

::right::

<div class="ml-4">
<v-clicks>

# Variadic Functions

```go {all|1-2|3-7|all}
// Function accepting variable number of arguments
func sum(numbers ...int) int {
    total := 0
    for _, num := range numbers {
        total += num
    }
    return total
}
```

## Calling the Function
```go {all|1-2|3-4|6-8|all}
// Different ways to call
fmt.Println(sum(1, 2))          // 3
fmt.Println(sum(1, 2, 3, 4, 5)) // 15

// Passing a slice
nums := []int{1, 2, 3, 4}
fmt.Println(sum(nums...))       // 10
```

</v-clicks>
</div>

---
layout: default
---

# Defer Statement

<v-clicks>

- `defer` schedules a function call to be executed just before the function returns
- Useful for cleanup operations (closing files, etc.)
- Deferred calls are executed in LIFO order (last-in, first-out)

</v-clicks>

<div class="mt-4">
<v-clicks>

## Example: File Handling

```go {all|1-2|3-6|8-9|11-12|14|all}
func processFile(filename string) error {
    // Open file
    f, err := os.Open(filename)
    if err != nil {
        return err
    }
    // Schedule file to be closed when function returns
    defer f.Close()
    
    // Process the file...
    // ...
    
    return nil
} // f.Close() is called here automatically
```

</v-clicks>
</div>

---
layout: default
---

# Multiple Defer Statements

<v-clicks>

## LIFO Order (Last In, First Out)

```go {all|1-2|3-5|6|all}
func countDown() {
    fmt.Println("Starting...")
    for i := 3; i > 0; i-- {
        defer fmt.Println(i)
    }
    fmt.Println("Go!")
}
```

## Output:
```
Starting...
Go!
1
2
3
```

</v-clicks>

---
layout: default
---

# Common Use Cases for Defer

<v-clicks>

- Closing files
- Releasing locks
- Closing database connections
- Logging function exit
- Recovering from panics

</v-clicks> 