---
layout: section
---

# Structs & Pointers

---
layout: default
---

# Understanding Pointers

<v-clicks>

## What are Pointers?

- Pointers store memory addresses of variables
- Allow you to pass references to values and records
- Enable more efficient memory usage

</v-clicks>

<div class="mt-4">
<v-clicks>

## Declaring and Using Pointers

```go {all|1-2|4-5|7-8|10-11|all}
// Declaring a pointer
var p *int

// Creating a pointer to a variable
x := 42
p = &x  // p now points to x

// Dereferencing a pointer (accessing the value)
fmt.Println(*p)  // 42

// Modifying the value through the pointer
*p = 100
fmt.Println(x)  // 100 (x has been changed)
```

</v-clicks>
</div>

---
layout: default
---

# Pointers to Structs

<v-clicks>

```go {all|1-4|6|7|9-10|12-13|15-16|all}
type Person struct {
    Name string
    Age  int
}

person := Person{"Alice", 30}
ptr := &person

// Accessing fields through a pointer
fmt.Println((*ptr).Name)  // Alice

// Shorthand syntax (Go automatically dereferences)
fmt.Println(ptr.Name)     // Alice

// Modifying through a pointer
ptr.Age = 31
fmt.Println(person.Age)   // 31
```

</v-clicks>

---
layout: default
---

# When to Use Pointers

- When you need to modify the original value
- When working with large structs (for efficiency)
- When implementing methods that modify the receiver
- When a value might be nil (using nil pointers)

---
layout: default
---

# Defining Structs

<v-clicks>

## Basic Struct Definition

```go {all|1-4|6-10|12-17|all}
// Basic struct definition
type Rectangle struct {
    Width  float64
    Height float64
}

// Struct with different types
type Product struct {
    ID    int
    Name  string
    Price float64
}

// Nested structs
type Address struct {
    Street  string
    City    string
    Country string
}
```

</v-clicks>

---
layout: default
---

# Working with Nested Structs

<v-clicks>

```go {all|1-5|7-12|14-15|17-18|all}
type Address struct {
    Street  string
    City    string
    Country string
}

type Employee struct {
    Name    string
    Age     int
    Address Address  // Nested struct
    Active  bool
}

// Creating with nested struct
emp := Employee{Name: "John", Age: 30, Address: Address{Street: "123 Main St", City: "Boston", Country: "USA"}, Active: true}

// Accessing nested fields
fmt.Println(emp.Address.City)  // Boston
```

</v-clicks>

---
layout: two-cols
---

# Creating Struct Instances

<v-clicks>

## Named Fields
```go {all|1-2|all}
// Creating with field names
rect := Rectangle{Width: 10.5, Height: 5.0}
```

## Positional Syntax
```go {all|1-2|all}
// Order matters!
rect := Rectangle{10.5, 5.0}
```

## Zero-valued Struct
```go {all|1-2|all}
// Fields set to zero values
var rect Rectangle
```

</v-clicks>

::right::

<div class="ml-4">
<v-clicks>

## Using new Function
```go {all|1-2|all}
// Returns a pointer to a zero-valued struct
rectPtr := new(Rectangle)
```

## Creating and Modifying
```go {all|1-2|3-4|all}
rect := Rectangle{}
rect.Width = 10
rect.Height = 5
```

## Anonymous Structs
```go {all|1-4|all}
person := struct {
    Name string
    Age  int
}{"Alice", 30}
```

</v-clicks>
</div>

---
layout: two-cols
---

# Struct Methods

<v-clicks>

## Method with Value Receiver

```go {all|1-3|5-7|9-10|all}
type Rectangle struct {
    Width, Height float64
}

func (r Rectangle) Area() float64 {
    return r.Width * r.Height
}

// Using the method
rect := Rectangle{10, 5}
fmt.Println(rect.Area())  // 50
```

</v-clicks>

::right::

<div class="ml-4">
<v-clicks>

## Method with Pointer Receiver

```go {all|1-3|5-7|9-10|all}
// Method with pointer receiver
func (r *Rectangle) Scale(factor float64) {
    r.Width *= factor
    r.Height *= factor
}

// Using the method
rect.Scale(2)
fmt.Println(rect.Width, rect.Height)  // 20 10
```

</v-clicks>
</div>

---
layout: two-cols
---

# Value vs Pointer Receivers

<v-clicks>

## Value Receivers
- Get a copy of the struct
- Cannot modify the original struct
- Good for read-only operations
- Safer for concurrent access

```go {all|1-3|5-6|all}
func (r Rectangle) Double() Rectangle {
    return Rectangle{r.Width * 2, r.Height * 2}
}

newRect := rect.Double() // Original rect unchanged
fmt.Println(rect, newRect)
```

</v-clicks>

::right::

<div class="ml-4">
<v-clicks>

## Pointer Receivers
- Get a reference to the struct
- Can modify the original struct
- More efficient for large structs
- Required when modifying the receiver

```go {all|1-4|6-7|all}
func (r *Rectangle) Enlarge(factor float64) {
    r.Width *= factor
    r.Height *= factor
}

rect.Enlarge(2) // Original rect is modified
fmt.Println(rect) // Width and Height are doubled
```

</v-clicks>
</div>

---
layout: two-cols
---

# Interfaces

<v-clicks>

## Defining Interfaces

```go {all|1-4|all}
// Interface definition
type Shape interface {
    Area() float64
    Perimeter() float64
}
```

</v-clicks>

::right::

<div class="ml-8">
<v-clicks>

## Implementing Interfaces

```go {all|1-4|6-8|10-12|14-17|19-22|all}
// Rectangle type
type Rectangle struct {
    Width, Height float64
}

func (r Rectangle) Area() float64 {
    return r.Width * r.Height
}

func (r Rectangle) Perimeter() float64 {
    return 2 * (r.Width + r.Height)
}

// Circle type
type Circle struct {
    Radius float64
}

func (c Circle) Area() float64 {
    return math.Pi * c.Radius * c.Radius
}

func (c Circle) Perimeter() float64 {
    return 2 * math.Pi * c.Radius
}
```

</v-clicks>
</div>

---
layout: default
---

# Using Interfaces

<v-clicks>

```go {all|1-4|6-9|all}
// Function that accepts any Shape
func PrintShapeInfo(s Shape) {
    fmt.Printf("Area: %.2f\n", s.Area())
    fmt.Printf("Perimeter: %.2f\n", s.Perimeter())
}

// Using the function with different shapes
rect := Rectangle{Width: 5, Height: 10}
circ := Circle{Radius: 7}
PrintShapeInfo(rect)  // Works with Rectangle
PrintShapeInfo(circ)  // Works with Circle
```

</v-clicks>

<div class="mt-8">
<v-clicks>

## Interface Benefits
- Enables polymorphism
- Decouples implementation from interface
- Allows for modular, testable code
- Facilitates code reuse
- No explicit declaration needed (implicit implementation)

</v-clicks>
</div>