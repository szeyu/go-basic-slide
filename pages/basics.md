---
layout: section
---

# Basics of Go

---
layout: default
---

# Variables & Data Types

<v-clicks>

## Variable Declaration

```go
// Using var keyword
var name string = "Gopher"
var age int = 5

// Short declaration (type inferred)
name := "Gopher"
age := 5
```

## Basic Data Types

```go
var i int = 42            // Integer
var f float64 = 3.14      // Floating point
var s string = "hello"    // String
var b bool = true         // Boolean
```

## Constants

```go
const Pi = 3.14159
const (
    StatusOK = 200
    StatusNotFound = 404
)
```

</v-clicks>

---
layout: two-cols
---

# Operators

<v-clicks>

## Arithmetic Operators
```go
a + b    // addition
a - b    // subtraction
a * b    // multiplication
a / b    // division
a % b    // modulus
```

## Comparison Operators
```go
a == b   // equal to
a != b   // not equal to
a < b    // less than
a <= b   // less than or equal to
a > b    // greater than
a >= b   // greater than or equal to
```

</v-clicks>

::right::

<div class="ml-4 mt-12">
<v-clicks>

## Logical Operators
```go
&&       // logical AND
||       // logical OR
!        // logical NOT
```

## Assignment Operators
```go
a = b    // simple assignment
a += b   // a = a + b
a -= b   // a = a - b
a *= b   // a = a * b
a /= b   // a = a / b
a %= b   // a = a % b
```

</v-clicks>
</div>

---
layout: default
---

# Conditional Statements

<v-clicks>

## If-Else Statement

```go {all|1-2|3-4|5-6|all}
if x > 10 {
    fmt.Println("x is greater than 10")
} else if x < 0 {
    fmt.Println("x is negative")
} else {
    fmt.Println("x is between 0 and 10")
}
```

</v-clicks>

<div class="mt-8">
<v-clicks>

## Switch Statement

```go {all|1|2-3|4-5|6-7|8-9|all}
switch day {
case "Monday":
    fmt.Println("Start of work week")
case "Friday":
    fmt.Println("End of work week")
case "Saturday", "Sunday":
    fmt.Println("Weekend")
default:
    fmt.Println("Midweek")
}
```

</v-clicks>
</div>

---
layout: two-cols
---

# Basic For Loop

<v-clicks>

## Standard For Loop
```go {all|1|2|3|all}
for i := 0; i < 10; i++ {
    fmt.Println(i)
}
```

## For Loop as While Loop
```go {all|1-2|3-4|5|all}
i := 0
for i < 10 {
    fmt.Println(i)
    i++
}
```

## Infinite Loop
```go {all|1-3|all}
for {
    // Do something forever
    // Use break to exit
}
```

</v-clicks>

::right::

<div class="ml-4">
<v-clicks>

# Iterating with Range

## Over a Slice
```go {all|1-2|3-4|all}
fruits := []string{"apple", "banana", "cherry"}
for index, value := range fruits {
    fmt.Printf("Index: %d, Value: %s\n", index, value)
}
```

## Over a Map
```go {all|1-2|3-4|all}
ages := map[string]int{"Alice": 25, "Bob": 30}
for key, value := range ages {
    fmt.Printf("%s is %d years old\n", key, value)
}
```

## Just Keys
```go {all|1-2|3|all}
for name := range ages {
    fmt.Println(name)
}
```

</v-clicks>
</div> 