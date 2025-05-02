---
layout: section
---

# Web Development with Gin Framework

---
layout: default
---

# Introduction to Gin

<v-clicks>

## What is Gin?

- Fast and lightweight web framework for Go
- Built on top of the standard net/http package
- Perfect for building APIs and web services
- Simple and intuitive API

## Key Features

- **Fast performance**: One of the fastest Go frameworks
- **Easy routing**: Simple path definitions with HTTP methods
- **JSON handling**: Built-in serialization and binding
- **Group routing**: Organize endpoints logically

</v-clicks>

---
layout: two-cols
---

# Basic Gin Server

<v-clicks>

## Installation

```bash
# Install Gin
go get -u github.com/gin-gonic/gin
```

## Hello World Example

```go
package main
import (
    "github.com/gin-gonic/gin"
    "net/http"
)
func main() {
    // Create a router
    r := gin.Default()
    
    // Define a route
    r.GET("/hello", func(c *gin.Context) {
        c.String(http.StatusOK, "Hello, World!")
    })
    
    // Run the server
    r.Run(":8080")
}
```

</v-clicks>

::right::

<div class="ml-4 mt-12">
<v-clicks>

## Basic Components

- `gin.Default()`: Creates a router with default settings
- HTTP Methods:
  ```go
  r.GET("/path", handlerFunc)
  r.POST("/path", handlerFunc)
  r.PUT("/path", handlerFunc)
  r.DELETE("/path", handlerFunc)
  ```

- Handler Function:
  ```go
  func handlerFunc(c *gin.Context) {
      // Access request data  // Process  // Send response
  }
  ```

- Starting the server:
  ```go
  r.Run(":8080")  // Listen on port 8080
  ```

</v-clicks>
</div>

---
layout: two-cols
---

# Path Parameters

<v-clicks>

## What are Path Parameters?

- Variable parts of a URL path
- Used to identify specific resources
- Defined with a colon `:` in the route path
- Example: `/users/:id`

## Using Path Parameters

```go
// Define a route with a parameter
r.GET("/user/:id", func(c *gin.Context) {
    // Extract the id parameter
    id := c.Param("id")
    
    // Use the parameter
    c.JSON(http.StatusOK, gin.H{
        "id": id,
        "message": "User found",
    })
})
```

</v-clicks>

::right::

<div class="ml-4 mt-12">
<v-clicks>

## Multiple Parameters

```go
// Define a route with multiple parameters
r.GET("/users/:userID/posts/:postID", func(c *gin.Context) {
    userID := c.Param("userID")
    postID := c.Param("postID")
    
    c.JSON(http.StatusOK, gin.H{
        "userID": userID,
        "postID": postID,
    })
})
```

## URL examples:

```
/user/123                   → id = "123"
/users/456/posts/789        → userID = "456", postID = "789"
```

</v-clicks>
</div>

---
layout: two-cols
---

# Query Parameters

<v-clicks>

## What are Query Parameters?

- Optional parameters added to a URL after `?`
- Used for filtering, sorting, pagination, etc.
- Format: `?key1=value1&key2=value2`
- Don't affect route matching

## Using Query Parameters

```go
// Define a route that uses query parameters
r.GET("/search", func(c *gin.Context) {
    query := c.Query("q")  // Get query parameter
    page := c.DefaultQuery("page", "1")  // Get query with default value if not present
    
    c.JSON(http.StatusOK, gin.H{
        "query": query,
        "page": page,
    })
})
```

</v-clicks>

::right::

<div class="ml-4 mt-12">
<v-clicks>

## Query vs Path Parameters

- **Path Parameters**
  - Required for route matching
  - Used to identify resources
  - Example: `/user/:id`

- **Query Parameters**
  - Optional, don't affect routing
  - Used for filtering, sorting, etc.
  - Example: `/products?category=books?sort=true`

</v-clicks>
</div>

---
layout: two-cols
---

# String and JSON Responses

<v-clicks>

## String Response

```go
r.GET("/hello", func(c *gin.Context) {
    c.String(http.StatusOK, "Hello World!")
})
```

## JSON Response

```go
r.GET("/json", func(c *gin.Context) {
    // Using gin.H shorthand (map[string]interface{})
    c.JSON(http.StatusOK, gin.H{
        "message": "success",
        "data": "some data",
    })
})
```

</v-clicks>

::right::

<div class="ml-4 mt-12">
<v-clicks>

## When to Use

- **String Response**
  - Simple text output
  - Plain text APIs
  - Debug messages

- **JSON Response**
  - Modern REST APIs
  - Client-side applications
  - Mobile app backends
  - When structure matters

</v-clicks>
</div>

---
layout: two-cols
---

# Struct and HTML Responses

<v-clicks>

## Returning a Struct as JSON

```go
type User struct {
    ID   int    `json:"id"`
    Name string `json:"name"`
}

r.GET("/user", func(c *gin.Context) {
    user := User{ID: 1, Name: "Gopher"}
    c.JSON(http.StatusOK, user)
})
```

## JSON Tag Customization

```go
type Product struct {
    ID          int     `json:"id"`
    Name        string  `json:"name"`
    Price       float64 `json:"price"`
    Description string  `json:"desc,omitempty"`
    InStock     bool    `json:"-"` // Skip this field
}
```

</v-clicks>

::right::

<div class="ml-4 mt-12">
<v-clicks>

## HTML Response

```go
r.GET("/html", func(c *gin.Context) {
    c.HTML(http.StatusOK, "index.html", gin.H{
        "title": "Gin Framework",
    })
})
```

## HTML Template Setup

```go
// Load templates
r.LoadHTMLGlob("templates/*")

// Or load a single template
r.LoadHTMLFiles("templates/index.html")

// Respond with HTML template
r.GET("/", func(c *gin.Context) {
    c.HTML(http.StatusOK, "index.html", gin.H{
        "title": "Homepage",
        "user": user,
    })
})
```

</v-clicks>
</div>

---
layout: two-cols
---

# Processing Form Submissions

<v-clicks>

## Getting Form Values

```go
r.POST("/form", func(c *gin.Context) {
    // Get form field values
    username := c.PostForm("username")
    password := c.PostForm("password")
    
    // With default value if not present
    remember := c.DefaultPostForm("remember", "false")
    
    c.JSON(http.StatusOK, gin.H{
        "username": username,
        "password": "********",
        "remember": remember,
    })
})
```

## Form Arrays

```go
// Handle form arrays (e.g., multiple checkboxes)
r.POST("/roles", func(c *gin.Context) {
    roles := c.PostFormArray("roles")
    c.JSON(http.StatusOK, gin.H{
        "roles": roles,
    })
})
```

</v-clicks>

::right::

<div class="ml-4 mt-12">
<v-clicks>

## HTML Form Example

```html
<form method="POST" action="/form">
    <input type="text" name="username">
    <input type="password" name="password">
    <input type="checkbox" name="remember" value="true">
    <button type="submit">Login</button>
</form>
```

## Binding Form to Struct

```go
type LoginForm struct {
    Username string `form:"username" binding:"required"`
    Password string `form:"password" binding:"required"`
    Remember bool   `form:"remember"`
}

r.POST("/login", func(c *gin.Context) {
    var form LoginForm
    if err := c.ShouldBind(&form); err != nil {
        c.JSON(http.StatusBadRequest, gin.H{
            "error": err.Error(),
        })
        return
    }
    // Process form...
})
```

</v-clicks>
</div>

---
layout: two-cols
---

# File Upload

<v-clicks>

## Handling File Uploads

```go
r.POST("/upload", func(c *gin.Context) {
    // Get single file
    file, err := c.FormFile("file")
    if err != nil {
        c.JSON(http.StatusBadRequest, gin.H{
            "error": "No file provided",
        })
        return
    }
    
    // Save the file
    filename := filepath.Base(file.Filename)
    c.SaveUploadedFile(file, "uploads/"+filename)
    
    c.JSON(http.StatusOK, gin.H{
        "filename": filename,
        "size": file.Size,
    })
})
```

</v-clicks>

::right::

<div class="ml-4 mt-12">
<v-clicks>

## HTML File Upload Example

```html
<form method="POST" action="/upload" enctype="multipart/form-data">
    <input type="file" name="file">
    <button type="submit">Upload</button>
</form>
```

## Multiple File Upload

```go
r.POST("/uploads", func(c *gin.Context) {
    // Get multiple files
    form, _ := c.MultipartForm()
    files := form.File["files"]
    
    for _, file := range files {
        filename := filepath.Base(file.Filename)
        c.SaveUploadedFile(file, "uploads/"+filename)
    }
    
    c.JSON(http.StatusOK, gin.H{
        "uploaded": len(files),
    })
})
```

</v-clicks>
</div>

---
layout: two-cols
---

# JSON Binding: Concepts

<v-clicks>

## What is Binding?

- Converting request data to Go structs
- Validates required fields automatically
- Handles different content types
- Uses struct tags for validation

## Common Binding Tags

- `binding:"required"` - Field must be present
- `binding:"min=2"` - Minimum length for strings
- `binding:"max=10"` - Maximum length for strings
- `binding:"email"` - Must be valid email format

</v-clicks>

::right::

<div class="ml-4 mt-12">
<v-clicks>

## More Validation Tags

- `binding:"oneof=red green blue"` - Must be one of values
- `binding:"lt=10"` - Less than 10
- `binding:"gt=0"` - Greater than 0
- `binding:"len=8"` - Exact length of 8
- `binding:"numeric"` - Must be numeric
- `binding:"alphanum"` - Must be alphanumeric
- `binding:"dive"` - Validate nested slice elements

## Combining Validations

```go
type User struct {
    Email string `json:"email" binding:"required,email"`
    Age   int    `json:"age" binding:"required,gt=0,lt=120"`
    Role  string `json:"role" binding:"required,oneof=admin user"`
}
```

</v-clicks>
</div>

---
layout: two-cols
---

# JSON Binding: Implementation

<v-clicks>

## Binding JSON to Struct

```go
// Define a struct with validation tags
type LoginRequest struct {
    Username string `json:"username" binding:"required"`
    Password string `json:"password" binding:"required"`
}

r.POST("/login", func(c *gin.Context) {
    var login LoginRequest
    
    // Bind JSON from request body
    if err := c.ShouldBindJSON(&login); err != nil {
        c.JSON(http.StatusBadRequest, gin.H{
            "error": "Invalid request",
        })
        return
    }
    
    // Process login
    c.JSON(http.StatusOK, gin.H{
        "message": "Logged in successfully",
    })
})
```

</v-clicks>

::right::

<div class="ml-4 mt-12">
<v-clicks>

## Example JSON Request

```json
{
  "username": "gopher",
  "password": "secret123"
}
```

## Other Binding Methods

```go
// Bind form data
c.ShouldBind(&form)

// Bind query parameters
c.ShouldBindQuery(&query)

// Bind URI parameters
c.ShouldBindUri(&uri)
```

## Benefits of Using Binding

- Automatic validation saves time
- Cleaner code with explicit validation rules
- Type safety with Go structs
- Consistent error handling

</v-clicks>
</div>

---
layout: two-cols
---

# Route Grouping: Concepts

<v-clicks>

## Why Group Routes?

- Organize related endpoints
- Apply common path prefixes
- Better code organization

## Examples of When to Use Groups

- API versioning (v1, v2)
- Resource hierarchies (users, posts)
- Access control (public, admin)
- Feature modules (auth, products)

</v-clicks>

::right::

<div class="ml-4 mt-12">
<v-clicks>

## Group Features

- Prefix all routes in the group
- Apply middleware to specific groups
- Nest groups for complex hierarchies
- Improves code readability

## Common Pattern

```go
// Main router
r := gin.Default()

// API group
api := r.Group("/api")

// API versioning groups
v1 := api.Group("/v1")
v2 := api.Group("/v2")

// Resource groups
users := v1.Group("/users")
products := v1.Group("/products")
```

</v-clicks>
</div>

---
layout: two-cols
---

# Basic Route Grouping

<v-clicks>

```go
// Create the router
r := gin.Default()

// Create a group with prefix '/api'
api := r.Group("/api")
{
    // GET /api/users
    api.GET("/users", func(c *gin.Context) {
        c.JSON(http.StatusOK, gin.H{"message": "get users"})
    })
    
    // POST /api/users
    api.POST("/users", func(c *gin.Context) {
        c.JSON(http.StatusOK, gin.H{"message": "create user"})
    })
}
```

</v-clicks>

::right::

<div class="ml-4 mt-12">
<v-clicks>

```go
// Create a group with prefix '/public'
public := r.Group("/public")
{
    // GET /public/landing
    public.GET("/landing", func(c *gin.Context) {
        c.String(http.StatusOK, "Public landing page")
    })
}

// Admin group with multiple endpoints
admin := r.Group("/admin")
{
    // GET /admin/dashboard
    admin.GET("/dashboard", func(c *gin.Context) {
        c.JSON(http.StatusOK, gin.H{"page": "dashboard"})
    })
    
    // GET /admin/analytics
    admin.GET("/analytics", func(c *gin.Context) {
        c.JSON(http.StatusOK, gin.H{"page": "analytics"})
    })
}
```

</v-clicks>
</div>

---
layout: two-cols
---

# Nested Route Groups: API v1

<v-clicks>

```go
r := gin.Default()

// API v1 group
v1 := r.Group("/v1")
{
    // User routes in v1
    users := v1.Group("/users")
    {
        // GET /v1/users
        users.GET("", func(c *gin.Context) {
            c.JSON(http.StatusOK, gin.H{
                "version": "1", 
                "entity": "users"
            })
        })
        
        // GET /v1/users/:id
        users.GET("/:id", func(c *gin.Context) {
            id := c.Param("id")
            c.JSON(http.StatusOK, gin.H{
                "version": "1", 
                "entity": "user", 
                "id": id
            })
        })
    }
}
```

</v-clicks>

::right::

<div class="ml-4 mt-12">
<v-clicks>

```go
// API v1 (continued)
v1 := r.Group("/v1")
{
    // Product routes in v1
    products := v1.Group("/products")
    {
        // GET /v1/products
        products.GET("", func(c *gin.Context) {
            c.JSON(http.StatusOK, gin.H{
                "version": "1", 
                "entity": "products"
            })
        })
    }
}

// API v2 group
v2 := r.Group("/v2")
{
    // Same route structure but with v2 implementation
    v2.GET("/users", func(c *gin.Context) {
        c.JSON(http.StatusOK, gin.H{
            "version": "2", 
            "entity": "users"
        })
    })
}
```

</v-clicks>
</div>

---
layout: two-cols
---

# RESTful API: Setup and Model

<v-clicks>

```go
// User struct
type User struct {
    ID   int    `json:"id"`
    Name string `json:"name"`
}

// In-memory data store
var users = []User{
    {ID: 1, Name: "Alice"},
    {ID: 2, Name: "Bob"},
}

// Set up router
func SetupRouter() *gin.Engine {
    r := gin.Default()
    
    // Routes will be defined next
    return r
}
```

</v-clicks>

::right::

<div class="ml-4 mt-12">
<v-clicks>

## REST Architecture

- **RE**presentational **S**tate **T**ransfer
- Resource-based (users, products, etc.)
- Uses standard HTTP methods
- Stateless communication
- Uniform interface

## HTTP Methods in REST

- **GET**: Retrieve a resource
- **POST**: Create a new resource
- **PUT**: Update a resource
- **DELETE**: Remove a resource
- **PATCH**: Partial update (not covered)

</v-clicks>
</div>

---
layout: two-cols
---

# RESTful API: Read Operations

<v-clicks>

## Get All Users

```go
// GET /users - List all users
r.GET("/users", func(c *gin.Context) {
    c.JSON(http.StatusOK, users)
})
```

## Get User by ID

```go
// GET /users/:id - Get a specific user
r.GET("/users/:id", func(c *gin.Context) {
    id, _ := strconv.Atoi(c.Param("id"))
    
    for _, user := range users {
        if user.ID == id {
            c.JSON(http.StatusOK, user)
            return
        }
    }
    
    c.JSON(http.StatusNotFound, gin.H{
        "error": "User not found"
    })
})
```

</v-clicks>

::right::

<div class="ml-4 mt-12">
<v-clicks>

## Status Codes for GET

- **200 OK**: Resource found
- **404 Not Found**: Resource doesn't exist

## Example Responses

Success:
```json
{
  "id": 1,
  "name": "Alice"
}
```

Error:
```json
{
  "error": "User not found"
}
```

## Filtering Example

```go
// GET /users?name=Alice
r.GET("/users", func(c *gin.Context) {
    name := c.Query("name")
    if name == "" {
        c.JSON(http.StatusOK, users)
        return
    }
    
    // Filter users by name
    var filtered []User
    for _, user := range users {
        if user.Name == name {
            filtered = append(filtered, user)
        }
    }
    c.JSON(http.StatusOK, filtered)
})
```

</v-clicks>
</div>

---
layout: two-cols
---

# RESTful API: Create Operation

<v-clicks>

```go
// POST /users - Create a new user
r.POST("/users", func(c *gin.Context) {
    var newUser User
    
    // Bind JSON from request body
    if err := c.ShouldBindJSON(&newUser); err != nil {
        c.JSON(http.StatusBadRequest, gin.H{
            "error": err.Error()
        })
        return
    }
    
    // Assign ID and add to users slice
    newUser.ID = len(users) + 1
    users = append(users, newUser)
    
    c.JSON(http.StatusCreated, newUser)
})
```

## Example POST Request

```json
{
  "name": "Charlie"
}
```

</v-clicks>

::right::

<div class="ml-4 mt-12">
<v-clicks>

## Status Codes for POST

- **201 Created**: Resource created successfully
- **400 Bad Request**: Invalid input data
- **409 Conflict**: Resource already exists

## Success Response

```json
{
  "id": 3,
  "name": "Charlie"
}
```

## Creating With Validation

```go
// POST /users with validation
r.POST("/users", func(c *gin.Context) {
    var newUser struct {
        Name string `json:"name" binding:"required,min=2"`
    }
    
    if err := c.ShouldBindJSON(&newUser); err != nil {
        c.JSON(http.StatusBadRequest, gin.H{
            "error": "Name is required and min length is 2"
        })
        return
    }
    
    // Create user...
})
```

</v-clicks>
</div>

---
layout: two-cols
---

# RESTful API: Update Operation

<v-clicks>

```go
// PUT /users/:id - Update a user
r.PUT("/users/:id", func(c *gin.Context) {
    id, _ := strconv.Atoi(c.Param("id"))
    
    var updatedUser User
    if err := c.ShouldBindJSON(&updatedUser); err != nil {
        c.JSON(http.StatusBadRequest, gin.H{
            "error": err.Error()
        })
        return
    }
    
    for i, user := range users {
        if user.ID == id {
            // Keep the same ID, update other fields
            updatedUser.ID = id
            users[i] = updatedUser
            c.JSON(http.StatusOK, updatedUser)
            return
        }
    }
    
    c.JSON(http.StatusNotFound, gin.H{
        "error": "User not found"
    })
})
```

</v-clicks>

::right::

<div class="ml-4 mt-12">
<v-clicks>

## Status Codes for PUT

- **200 OK**: Resource updated successfully
- **400 Bad Request**: Invalid input data
- **404 Not Found**: Resource doesn't exist

## Example PUT Request

```json
{
  "name": "Charlie Updated"
}
```

## Success Response

```json
{
  "id": 3,
  "name": "Charlie Updated"
}
```

## PUT vs PATCH

- **PUT**: Replace the entire resource
- **PATCH**: Update specific fields only
- Choose based on your API design needs

</v-clicks>
</div>

---
layout: two-cols
---

# RESTful API: Delete Operation

<v-clicks>

```go
// DELETE /users/:id - Delete a user
r.DELETE("/users/:id", func(c *gin.Context) {
    id, _ := strconv.Atoi(c.Param("id"))
    
    for i, user := range users {
        if user.ID == id {
            // Remove user from slice
            users = append(users[:i], users[i+1:]...)
            c.JSON(http.StatusOK, gin.H{
                "message": "User deleted successfully"
            })
            return
        }
    }
    
    c.JSON(http.StatusNotFound, gin.H{
        "error": "User not found"
    })
})
```

</v-clicks>

::right::

<div class="ml-4 mt-12">
<v-clicks>

## Status Codes for DELETE

- **200 OK**: Resource deleted successfully
- **204 No Content**: Successfully deleted, no response body
- **404 Not Found**: Resource doesn't exist

## Success Response

```json
{
  "message": "User deleted successfully"
}
```

## Complete API

- Our API now supports all CRUD operations:
  - Create (POST)
  - Read (GET)
  - Update (PUT)
  - Delete (DELETE)
- Each operation follows REST principles
- Consistent status codes and response formats

</v-clicks>
</div>

---
layout: default
---

# Key Takeaways

<v-clicks>

## Why Use Gin?

- Fast and lightweight
- Simple API design
- Easy to learn and use
- Great for RESTful APIs
- Good performance

## Common Usage Patterns

- JSON APIs
- Web services
- Microservices
- Single page applications (backend)

## Best Practices

- Use route groups for organization
- Handle errors properly
- Use binding for input validation
- Return appropriate HTTP status codes
- Keep route handlers small and focused

## Resources

- [Gin GitHub](https://github.com/gin-gonic/gin)
- [Gin Documentation](https://gin-gonic.com/docs/)

</v-clicks> 
