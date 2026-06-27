# Express.js Interview Notes

## What is Express.js?

- **Express.js** is a lightweight web framework built on top of **Node.js**.
    
- It simplifies creating web servers and REST APIs.
    

**Without Express**

```javascript
http.createServer(...)
```

**With Express**

```javascript
app.get("/users", ...)
```

---

# Why Express?

- Easy routing
    
- Middleware support
    
- REST API development
    
- Better project organization
    
- Large ecosystem
    

---

# Installation

```bash
npm install express
```

---

# Basic Server

```javascript
import express from "express";

const app = express();

app.get("/", (req, res) => {
    res.send("Hello World");
});

app.listen(3000, () => {
    console.log("Server running");
});
```

---

# Important Methods

## express()

Creates an Express application.

```javascript
const app = express();
```

---

## app.listen()

Starts the server.

```javascript
app.listen(3000);
```

---

## app.use()

Registers middleware.

```javascript
app.use(express.json());
```

---

# HTTP Methods (CRUD)

|Method|Purpose|CRUD|
|---|---|---|
|GET|Read data|Read|
|POST|Create data|Create|
|PUT|Replace entire resource|Update|
|PATCH|Update part of resource|Update|
|DELETE|Remove resource|Delete|

Example

```javascript
app.get("/users")

app.post("/users")

app.put("/users/:id")

app.patch("/users/:id")

app.delete("/users/:id")
```

---

# Routing

Routes decide what happens when a specific URL is requested.

```javascript
app.get("/", (req, res) => {});
```

```javascript
app.get("/users", (req, res) => {});
```

```javascript
app.post("/users", (req, res) => {});
```

---

# Request Object (req)

Contains data sent by the client.

Common Properties

```javascript
req.body
req.params
req.query
req.headers
req.method
req.url
```

---

## req.body

Contains request body.

```javascript
req.body.name
```

Requires

```javascript
app.use(express.json());
```

---

## req.params

Access URL parameters.

Route

```javascript
app.get("/users/:id")
```

Request

```
/users/10
```

Access

```javascript
req.params.id
```

Output

```
10
```

---

## req.query

Access query parameters.

Request

```
/users?page=2
```

Access

```javascript
req.query.page
```

Output

```
2
```

---

# Response Object (res)

Used to send data back to the client.

---

## res.send()

Send plain text or HTML.

```javascript
res.send("Hello")
```

---

## res.json()

Send JSON response.

```javascript
res.json({
    id: 1,
    name: "John"
})
```

---

## res.status()

Send HTTP status code.

```javascript
res.status(404).json({
    message: "User Not Found"
});
```

---

# Middleware ⭐⭐⭐⭐⭐

Middleware executes **before** the route handler.

Flow

```
Request
   ↓
Middleware
   ↓
Route
   ↓
Response
```

Example

```javascript
app.use((req, res, next) => {
    console.log(req.method);
    next();
});
```

### next()

Passes control to the next middleware or route.

Without `next()`, the request stops.

---

# express.json()

Converts incoming JSON into JavaScript objects.

```javascript
app.use(express.json());
```

Without it

```javascript
req.body
```

will usually be `undefined`.

---

# Route Parameters

```javascript
app.get("/products/:id", (req, res) => {
    console.log(req.params.id);
});
```

URL

```
/products/5
```

Output

```
5
```

---

# Query Parameters

```javascript
/products?page=1&limit=10
```

Access

```javascript
req.query.page
req.query.limit
```

---

# Status Codes

|Code|Meaning|
|---|---|
|200|OK|
|201|Created|
|204|No Content|
|400|Bad Request|
|401|Unauthorized|
|403|Forbidden|
|404|Not Found|
|500|Internal Server Error|

---

# REST API

REST = **Representational State Transfer**

Good API Design

```
GET      /users
GET      /users/1
POST     /users
PUT      /users/1
PATCH    /users/1
DELETE   /users/1
```

Avoid verbs in URLs.

❌

```
/getUsers
```

✅

```
/users
```

---

# CRUD Mapping

|CRUD|HTTP|
|---|---|
|Create|POST|
|Read|GET|
|Update|PUT / PATCH|
|Delete|DELETE|

---

# Router

Used to split routes into separate files.

```javascript
import express from "express";

const router = express.Router();

router.get("/", () => {});
```

---

# Folder Structure

```
project/
│
├── controllers/
├── models/
├── routes/
├── middleware/
├── config/
├── app.js
└── package.json
```

---

# CORS

**Full Form:** Cross-Origin Resource Sharing

Allows frontend and backend running on different origins to communicate.

```javascript
import cors from "cors";

app.use(cors());
```

Example

```
React
localhost:5173

↓

Express

localhost:5000
```

---

# Environment Variables

Store secrets outside source code.

.env

```
PORT=5000

DB_URL=...

JWT_SECRET=...
```

Access

```javascript
process.env.PORT
```

---

# Error Handling

```javascript
try {

} catch (error) {

    res.status(500).json({
        error: error.message
    });

}
```

---

# Express Request Lifecycle

```
Client

↓

HTTP Request

↓

Express App

↓

Middleware

↓

Route

↓

Controller

↓

Database

↓

Response

↓

Client
```

---

# Interview Differences

## GET vs POST

|GET|POST|
|---|---|
|Read data|Create data|
|No body (typically)|Has body|
|Can be cached|Usually not cached|

---

## PUT vs PATCH

|PUT|PATCH|
|---|---|
|Replace entire resource|Update selected fields|

---

## req.params vs req.query

|req.params|req.query|
|---|---|
|`/users/10`|`/users?page=2`|
|Required|Optional|

---

## req vs res

|req|res|
|---|---|
|Incoming request|Outgoing response|

---

# Full Forms

- **HTTP** → HyperText Transfer Protocol
    
- **REST** → Representational State Transfer
    
- **CRUD** → Create Read Update Delete
    
- **API** → Application Programming Interface
    
- **MVC** → Model View Controller
    
- **CORS** → Cross-Origin Resource Sharing
    
- **JSON** → JavaScript Object Notation
    

---

# Frequently Asked Interview Questions

1. What is Express.js?
    
2. Why use Express over Node's `http` module?
    
3. What is middleware?
    
4. What does `next()` do?
    
5. What is `express.json()`?
    
6. Difference between GET and POST?
    
7. Difference between PUT and PATCH?
    
8. Difference between `req.params` and `req.query`?
    
9. What is REST?
    
10. What is CRUD?
    
11. What is CORS?
    
12. Explain the Express request lifecycle.
    

---

# Revision Checklist ✅

-  Express setup
    
-  `express()`
    
-  `app.listen()`
    
-  Routing
    
-  CRUD
    
-  HTTP methods
    
-  `req` & `res`
    
-  `req.body`
    
-  `req.params`
    
-  `req.query`
    
-  Middleware
    
-  `next()`
    
-  `express.json()`
    
-  Status codes
    
-  REST APIs
    
-  CORS
    
-  Router
    
-  Environment variables
    
-  Error handling