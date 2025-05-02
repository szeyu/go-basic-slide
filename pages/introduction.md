---
layout: section
---

# Introduction to Golang

---
layout: two-cols
---

# What is Go?

<v-clicks>

- Created by Google engineers in 2007
- Officially launched in 2009
- Designed for simplicity, efficiency, and productivity
- Statically typed, compiled language
- Garbage collected
- Built-in concurrency support

</v-clicks>

::right::

<div class="ml-4">
  <img src="https://upload.wikimedia.org/wikipedia/commons/0/05/Go_Logo_Blue.svg" class="h-60 rounded shadow" />
</div>

---

# Features and Benefits

<v-clicks>

- **Fast compilation** - builds large projects quickly
- **Garbage collection** - automatic memory management
- **Concurrency** - goroutines and channels
- **Simplicity** - small language specification
- **Standard library** - rich and well-documented
- **Cross-platform** - compile for different OS/architectures

</v-clicks>

---
layout: default
---

# Installation & Setup

```bash {all|1|2-3|4-5|all}
# Download and install Go from https://golang.org/dl/

# Verify installation
go version
# Should display: go version go1.x.x [your OS]

# Set up environment variables (if needed)
# GOPATH, GOROOT
```

<v-click>

## Your First Go Program

```go {all|1|3|5-7|all}
package main

import "fmt"

func main() {
    fmt.Println("Hello, Go!")
}
```

Run with: `go run hello.go`

</v-click>

---

# Understanding Go Workspace

<v-clicks>

- **GOROOT**: Where Go is installed
- **GOPATH**: Your Go workspace
- **Go Modules**: Modern dependency management

</v-clicks>

<v-click>

## Creating a Module

```bash
# Initialize a new module
go mod init example.com/myproject

# Add dependencies
go get github.com/some/dependency

# Build your program
go build

# Run your program
go run main.go
```

</v-click> 