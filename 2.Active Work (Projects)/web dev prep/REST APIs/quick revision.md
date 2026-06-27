# REST API Interview Notes

## What is REST?

**REST** = **Representational State Transfer**

REST is an architectural style used to design web APIs.

A REST API allows different applications (Frontend, Backend, Mobile Apps) to communicate over HTTP.

---

# What is an API?

**API** = **Application Programming Interface**

An API is a bridge that allows two applications to communicate.

Example

```text
React Frontend

↓

REST API

↓

Express Backend

↓

Database
```

---

# Why REST?

Without APIs

```text
Frontend

×

Backend
```

No communication.

With REST

```text
Frontend

↓

REST API

↓

Backend
```

Everything communicates through requests and responses.

---

# Client-Server Architecture

## Client

Requests data.

Examples

- React
    
- Angular
    
- Mobile Apps
    
- Browser
    

---

## Server

Processes requests and sends responses.

Examples

- Express.js
    
- Spring Boot
    
- Django
    
- ASP.NET
    

---

# HTTP

**HTTP** = **HyperText Transfer Protocol**

REST APIs use HTTP.

Flow

```text
Client

↓

HTTP Request

↓

Server

↓

HTTP Response

↓

Client
```

---

# Request-Response Cycle

```text
React

↓

GET /users

↓

Express

↓

Database

↓

JSON Response

↓

React Updates UI
```

---

# REST Resources

A resource is anything your API manages.

Examples

```text
/users

/products

/orders

/employees

/posts
```

Resources are usually **nouns**, not verbs.

---

# HTTP Methods ⭐⭐⭐⭐⭐

## GET

Retrieve data.

```http
GET /users
```

Returns

```json
[
  {
    "id":1,
    "name":"John"
  }
]
```

---

## POST

Create new data.

```http
POST /users
```

Request Body

```json
{
  "name":"John"
}
```

---

## PUT

Replace the entire resource.

```http
PUT /users/1
```

---

## PATCH

Update only selected fields.

```http
PATCH /users/1
```

Example

```json
{
   "name":"Alice"
}
```

---

## DELETE

Delete resource.

```http
DELETE /users/1
```

---

# CRUD Mapping

|CRUD|HTTP|
|---|---|
|Create|POST|
|Read|GET|
|Update|PUT/PATCH|
|Delete|DELETE|

---

# URL Structure

Good

```text
/users

/users/10

/products

/orders
```

Bad

```text
/getUsers

/createUser

/deleteProduct
```

REST prefers nouns.

---

# Route Parameters

Example

```text
/users/25
```

Here

```text
25
```

is the route parameter.

Express

```javascript
req.params.id
```

---

# Query Parameters

Example

```text
/users?page=2&limit=10
```

Access

```javascript
req.query.page

req.query.limit
```

Used for

- Pagination
    
- Filtering
    
- Sorting
    
- Searching
    

---

# Headers

Headers contain additional request information.

Example

```http
Content-Type: application/json

Authorization: Bearer TOKEN
```

Common Headers

|Header|Purpose|
|---|---|
|Content-Type|Data format|
|Authorization|Authentication|
|Accept|Expected response format|

---

# Request Body

Contains data sent to the server.

Example

```json
{
    "name":"John",
    "age":22
}
```

Express

```javascript
req.body
```

---

# JSON

**JSON** = **JavaScript Object Notation**

Standard format for exchanging data.

Example

```json
{
    "id":1,
    "name":"Alice",
    "email":"alice@gmail.com"
}
```

---

# HTTP Status Codes ⭐⭐⭐⭐⭐

## 200 OK

Request successful.

---

## 201 Created

Resource created successfully.

---

## 204 No Content

Request successful.

No response body.

---

## 400 Bad Request

Invalid client request.

---

## 401 Unauthorized

Authentication required.

---

## 403 Forbidden

Authenticated but not allowed.

---

## 404 Not Found

Requested resource doesn't exist.

---

## 500 Internal Server Error

Unexpected server error.

---

# Authentication

Authentication verifies identity.

Example

```text
Login

↓

Username

Password

↓

Token
```

---

# Authorization

Authorization determines permissions.

Example

```text
Admin

↓

Can Delete

User

↓

Cannot Delete
```

---

# JWT

**JWT** = **JSON Web Token**

Used for authentication.

Flow

```text
Login

↓

JWT Generated

↓

Client Stores Token

↓

Every Request

↓

Authorization Header
```

Header

```http
Authorization: Bearer TOKEN
```

---

# Cookies

Alternative way to store session/authentication data.

Browser automatically sends cookies with requests.

---

# Stateless APIs ⭐⭐⭐⭐⭐

REST APIs are **stateless**.

Meaning

Every request contains everything needed.

Server does not remember previous requests.

Example

```text
Request 1

↓

Response

Request 2

↓

Response
```

Each request is independent.

---

# REST Constraints (Interview)

You only need to know:

- Client-Server
    
- Stateless
    
- Uniform Interface
    
- Cacheable (basic idea)
    

---

# Idempotent Methods

Calling multiple times gives the same result.

|Method|Idempotent|
|---|---|
|GET|✅|
|PUT|✅|
|DELETE|✅|
|POST|❌|

Example

Delete user twice

Still deleted.

POST twice

Creates two users.

---

# API Testing

Tools

- Postman
    
- Thunder Client
    
- Insomnia
    

---

# API Response Example

```json
{
    "success": true,
    "message": "User Created",
    "data": {
        "id": 1,
        "name": "John"
    }
}
```

---

# REST API Flow

```text
React

↓

Fetch API

↓

HTTP Request

↓

Express Route

↓

Controller

↓

Database

↓

JSON Response

↓

React Updates UI
```

---

# Important Differences

## PUT vs PATCH

|PUT|PATCH|
|---|---|
|Replace entire resource|Update selected fields|

---

## Authentication vs Authorization

|Authentication|Authorization|
|---|---|
|Who are you?|What can you access?|

---

## Route Params vs Query Params

|Route Params|Query Params|
|---|---|
|Required|Optional|
|`/users/10`|`/users?page=2`|

---

## Cookies vs JWT

|Cookies|JWT|
|---|---|
|Stored in browser|Stored by client (often local storage or cookies)|
|Automatic with requests|Sent manually in Authorization header (commonly)|

---

# Full Forms

REST → Representational State Transfer

API → Application Programming Interface

HTTP → HyperText Transfer Protocol

HTTPS → HyperText Transfer Protocol Secure

JSON → JavaScript Object Notation

JWT → JSON Web Token

CRUD → Create Read Update Delete

URL → Uniform Resource Locator

---

# Frequently Asked Interview Questions

1. What is REST?
    
2. What is an API?
    
3. Why use REST APIs?
    
4. Difference between PUT and PATCH?
    
5. Difference between Authentication and Authorization?
    
6. Difference between Cookies and JWT?
    
7. What are HTTP methods?
    
8. What are status codes?
    
9. What is JSON?
    
10. Why are REST APIs stateless?
    
11. What are route parameters?
    
12. What are query parameters?
    

---

# Revision Checklist ✅

-  REST
    
-  API
    
-  HTTP
    
-  Request-Response Cycle
    
-  CRUD
    
-  HTTP Methods
    
-  Resources
    
-  Route Parameters
    
-  Query Parameters
    
-  Headers
    
-  Request Body
    
-  JSON
    
-  Status Codes
    
-  Authentication
    
-  Authorization
    
-  JWT
    
-  Cookies
    
-  Stateless APIs
    
-  Idempotent Methods
    
-  API Testing