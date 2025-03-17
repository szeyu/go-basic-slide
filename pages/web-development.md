---
layout: section
---

# Introduction to Web Development

---
layout: two-cols
---

# Creating a Simple HTTP Server

<v-clicks>

## Basic HTTP Server

```go {all|1-2|4-6|8-10|12-15|17-18|all}
package main

import (
    "fmt"
    "log"
    "net/http"
)

func helloHandler(w http.ResponseWriter, r *http.Request) {
    fmt.Fprintf(w, "Hello, Go Web Development!")
}

func main() {
    // Register a handler function for a specific path
    http.HandleFunc("/hello", helloHandler)
    
    // Start the server on port 8080
    fmt.Println("Server starting on port 8080...")
    log.Fatal(http.ListenAndServe(":8080", nil))
}
```

</v-clicks>

::right::

<div class="ml-4">
<v-clicks>

## Testing the Server

```bash
# Run the server
$ go run main.go

# Test with curl
$ curl http://localhost:8080/hello

# Or open in browser:
http://localhost:8080/hello
```

### Key Points
- Server runs on port 8080
- Handles requests at `/hello` path
- Returns plain text response
- Uses standard `net/http` package

</v-clicks>
</div>

---
layout: two-cols
---

# Handling Different HTTP Methods

<v-clicks>

## HTTP Method Handler

```go {all|1-3|5-6|7-8|9-10|11-12|13-14|16-21|all}
func userHandler(w http.ResponseWriter, r *http.Request) {
    // Check the HTTP method
    switch r.Method {
    case "GET":
        // Retrieve user data
        fmt.Fprintf(w, "Get user data")
    case "POST":
        // Create a new user
        fmt.Fprintf(w, "Create user")
    case "PUT":
        // Update an existing user
        fmt.Fprintf(w, "Update user")
    case "DELETE":
        // Delete a user
        fmt.Fprintf(w, "Delete user")
    default:
        // Method not allowed
        http.Error(w, "Method not allowed", http.StatusMethodNotAllowed)
    }
}
```

</v-clicks>

::right::

<div class="ml-4">
<v-clicks>

## Server Setup

```go {all|1-3|4-5|all}
func main() {
    // Register our user handler
    http.HandleFunc("/user", userHandler)
    // Start the server
    log.Fatal(http.ListenAndServe(":8080", nil))
}
```

### Supported Methods
- GET: Retrieve user data
- POST: Create new user
- PUT: Update existing user
- DELETE: Remove user
- Others: Return 405 Method Not Allowed

### Usage
```bash
# Test different methods
curl -X GET http://localhost:8080/user
curl -X POST http://localhost:8080/user
curl -X PUT http://localhost:8080/user
curl -X DELETE http://localhost:8080/user
```

</v-clicks>
</div>

---
layout: two-cols
---

# Returning JSON Responses

<v-clicks>

## Define Data Structure

```go {all|1-6|all}
type User struct {
    ID        int    `json:"id"`
    Username  string `json:"username"`
    Email     string `json:"email"`
    CreatedAt string `json:"created_at"`
}
```

### Key Points
- Use struct tags for JSON field names
- Fields must be exported (capitalized)
- Supports nested structures
- Handles various data types

</v-clicks>

::right::

<div class="ml-4">
<v-clicks>

## JSON Response Handler

```go {all|1-8|10-13|15-19|all}
func getUserHandler(w http.ResponseWriter, r *http.Request) {
    // Create a user
    user := User{
        ID:        1,
        Username:  "gopher",
        Email:     "gopher@example.com",
        CreatedAt: "2023-01-01",
    }
    
    // Set content type header
    w.Header().Set("Content-Type", "application/json")
    w.WriteHeader(http.StatusOK)
    
    // Encode user to JSON and write to response
    err := json.NewEncoder(w).Encode(user)
    if err != nil {
        http.Error(w, err.Error(), http.StatusInternalServerError)
        return
    }
}
```

### Testing
```bash
curl http://localhost:8080/user
```

</v-clicks>
</div>

---
layout: two-cols
---

# Making HTTP Requests (1/2)

<v-clicks>

## GET Request Example

```go {all|1-3|4-8|9-14|15-19|all}
func fetchData() {
    // URL to fetch
    url := "https://api.example.com/data"
    // Make a GET request
    resp, err := http.Get(url)
    if err != nil { log.Fatal(err) }
    // Always close the response body
    defer resp.Body.Close()
    // Check status code
    if resp.StatusCode != http.StatusOK {
        log.Fatalf("Unexpected status: %d", resp.StatusCode)
    }
    // Read response body
    body, err := io.ReadAll(resp.Body)
    if err != nil {
        log.Fatal(err)
    }
    fmt.Println(string(body))
}
```

</v-clicks>

::right::

<div class="ml-4">
<v-clicks>

### Key Points
- Use `http.Get` for simple GET requests
- Always close response body
- Check status code before reading
- Use `io.ReadAll` to read response

</v-clicks>
</div>

---
layout: two-cols
---

# Making HTTP Requests (2/2)

<v-clicks>

## POST Request Example

```go {all|1-6|7-9|10-15|16-19|all}
func createUser() {
    // Data to send
    data := map[string]string{
        "username": "newuser",
        "email":    "newuser@example.com",
    }
    // Convert data to JSON
    jsonData, err := json.Marshal(data)
    if err != nil { log.Fatal(err)}
    // Make a POST request
    resp, err := http.Post(
        "https://api.example.com/users",
        "application/json",
        bytes.NewBuffer(jsonData),
    )
    if err != nil { log.Fatal(err) }
    defer resp.Body.Close()
    // Process response...
}
```

</v-clicks>

::right::

<div class="ml-4">
<v-clicks>

### Key Points
- Use `json.Marshal` to encode data
- Set proper content type
- Use `http.Post` for POST requests
- Remember to close response body

</v-clicks>
</div>

---
layout: default
---

# Using the HTTP Client

<v-clicks>

## Custom HTTP Client

```go {all|1-4|6-9|11-14|15-19|20-22|all}
func customRequest() {
    // Create a custom client with timeout
    client := &http.Client{
        Timeout: 10 * time.Second,
    }
    
    // Create a new request
    req, err := http.NewRequest("GET", "https://api.example.com/data", nil)
    if err != nil { log.Fatal(err) }
    
    // Add headers
    req.Header.Add("Authorization", "Bearer token123")
    req.Header.Add("Accept", "application/json")
    
    // Send the request
    resp, err := client.Do(req)
    if err != nil {
        log.Fatal(err)
    }
    defer resp.Body.Close()
    // Process response...
}
```
</v-clicks>

---
layout: default
---

# Using Query Parameters

<v-clicks>

## Query String Construction

```go {all|1-4|6-9|11-12|14-17|all}
func searchUsers(query string, page int) {
    // Base URL
    baseURL := "https://api.example.com/users"
    
    // Create URL values
    values := url.Values{}
    values.Add("q", query)
    values.Add("page", strconv.Itoa(page))
    
    // Append query string to URL
    requestURL := baseURL + "?" + values.Encode()
    
    // Make request
    resp, err := http.Get(requestURL)
    if err != nil {
        log.Fatal(err)
    }
    defer resp.Body.Close()
    
    // Process response...
}
```

</v-clicks> 
