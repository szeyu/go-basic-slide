---
layout: section
---

# Working with Collections

---
layout: default
---

# Arrays

<v-clicks>

## Declaring Arrays

```go {all|1-2|4-5|7-8|10-11|all}
// Declaring an array with a specific size
var numbers [5]int // Creates an array of 5 integers, all initialized to 0

// Initializing with values
var fruits [3]string = [3]string{"apple", "banana", "cherry"}

// Short declaration with initialization
colors := [4]string{"red", "blue", "green", "yellow"}

// Using ... to let the compiler count the elements
animals := [...]string{"dog", "cat", "bird"}
```

</v-clicks>

---
layout: two-cols
---

# Working with Arrays

<v-clicks>

## Accessing Elements
```go {all|1-2|4-5|all}
// Accessing elements (zero-indexed)
fmt.Println(fruits[0]) // "apple"

// Modifying elements
fruits[1] = "orange"
```

## Array Length
```go
// Getting array length
fmt.Println(len(fruits)) // 3
```

## Iterating Over Arrays
```go {all|1-2|3-5|all}
// Using a for loop
for i := 0; i < len(fruits); i++ {
    fmt.Println(fruits[i])
}
```

</v-clicks>

::right::

<div class="ml-4">
<v-clicks>

## Using Range
```go {all|1-2|3-5|all}
// Using range
for index, fruit := range fruits {
    fmt.Printf("%d: %s\n", index, fruit)
}
```

## Multi-dimensional Arrays
```go {all|1-5|7-8|all}
// 2D array
matrix := [3][3]int{
    {1, 2, 3},
    {4, 5, 6},
    {7, 8, 9},
}

// Accessing elements
fmt.Println(matrix[1][2]) // 6
```

</v-clicks>
</div>

---
layout: default
---

# Slices

<v-clicks>

## Creating Slices

```go {all|1-3|5-6|8-10|all}
// Creating a slice from an array
array := [5]int{1, 2, 3, 4, 5}
slice := array[1:4] // Elements 1, 2, 3 (indices 1, 2, 3)

// Creating a slice directly
numbers := []int{1, 2, 3, 4, 5}

// Creating a slice with make
// make([]T, length, capacity)
slice := make([]int, 5, 10)
```

</v-clicks>

---
layout: two-cols
---

# Modifying Slices

<v-clicks>

## Appending Elements
```go {all|1-2|3-4|all}
numbers := []int{1, 2, 3}
numbers = append(numbers, 4, 5)
fmt.Println(numbers) // [1 2 3 4 5]
```

## Appending Slices
```go {all|1-3|4-5|all}
slice1 := []int{1, 2, 3}
slice2 := []int{4, 5, 6}
slice1 = append(slice1, slice2...)
fmt.Println(slice1) // [1 2 3 4 5 6]
```

## Slicing Operations
```go {all|1-2|3-4|5-6|all}
s := []int{1, 2, 3, 4, 5}
fmt.Println(s[1:4]) // [2 3 4]
fmt.Println(s[:3])  // [1 2 3]
fmt.Println(s[2:])  // [3 4 5]
```

</v-clicks>

::right::

<div class="ml-4">
<v-clicks>

## Copying Slices
```go {all|1-3|4-5|all}
src := []int{1, 2, 3}
dst := make([]int, len(src))
copy(dst, src)
fmt.Println(dst) // [1 2 3]
```

## Slice Length and Capacity
```go {all|1-2|3-4|5-6|all}
s := make([]int, 3, 5)
fmt.Println(len(s)) // 3
fmt.Println(cap(s)) // 5

// Growing beyond capacity
s = append(s, 1, 2, 3) // New backing array allocated
```

## Slice Internals
- Reference to underlying array
- Length (number of elements)
- Capacity (max size without reallocation)

</v-clicks>
</div>

---
layout: default
---

# Maps

<v-clicks>

## Creating Maps

```go {all|1-2|4-5|7-12|all}
// Declaring a map
var ages map[string]int

// Initializing a map
ages = make(map[string]int)

// Declaration and initialization
ages := map[string]int{
    "Alice": 25,
    "Bob":   30,
    "Carol": 27,
}
```

</v-clicks>

---
layout: two-cols
---

# Working with Maps

<v-clicks>

## Adding/Updating Entries
```go {all|1-2|4-5|all}
// Adding a new entry
ages["Dave"] = 29

// Updating an existing entry
ages["Alice"] = 26
```

## Accessing Values
```go {all|1-2|4-7|all}
// Simple access
fmt.Println(ages["Alice"]) // 26

// Checking if a key exists
age, exists := ages["Eve"]
if exists {
    fmt.Println("Eve's age is", age)
}
```

</v-clicks>

::right::

<div class="ml-4">
<v-clicks>

## Deleting Entries
```go {all|1-2|3-4|all}
// Deleting an entry
delete(ages, "Bob")
fmt.Println(ages) // map[Alice:26 Carol:27 Dave:29]
```

## Iterating Over Maps
```go {all|1-3|5-7|all}
// Iterating over key-value pairs
for name, age := range ages {
    fmt.Printf("%s is %d years old\n", name, age)
}

// Iterating over just keys
for name := range ages {
    fmt.Println(name)
}
```

## Map Size
```go
// Getting the number of entries
fmt.Println(len(ages)) // 3
```

</v-clicks>
</div> 