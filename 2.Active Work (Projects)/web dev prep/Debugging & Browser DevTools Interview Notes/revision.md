Now we're into the **last few topics**. These are much smaller than React or Express but interviewers still ask them.

---

# Debugging & Browser DevTools Interview Notes

# What is Debugging?

Debugging is the process of **finding, understanding, and fixing bugs (errors)** in your code.

A bug can be:

- Syntax Error
    
- Runtime Error
    
- Logical Error
    

---

# Types of Errors

## 1. Syntax Error

Breaking the language rules.

Example

```javascript
console.log("Hello"
```

Missing `)`.

Program won't run.

---

## 2. Runtime Error

Program starts but crashes while running.

Example

```javascript
let user = null;

console.log(user.name);
```

Output

```text
TypeError
```

---

## 3. Logical Error

Program runs but produces the wrong result.

Example

```javascript
function add(a,b){
    return a-b;
}
```

No crash.

Wrong answer.

---

# Console

Most commonly used debugging tool.

```javascript
console.log(data);

console.error(error);

console.warn("Warning");
```

---

# Browser DevTools

Shortcut

```
F12
```

or

```
Ctrl + Shift + I
```

---

# Important DevTools Tabs

## Elements

Purpose

Inspect HTML and CSS.

Use for

- Check CSS
    
- Modify styles
    
- Inspect DOM
    
- Find layout problems
    

---

## Console ⭐⭐⭐⭐⭐

Purpose

Run JavaScript.

View

- Errors
    
- Logs
    
- Warnings
    

---

## Network ⭐⭐⭐⭐⭐

Most important for Full Stack.

Shows

- API Requests
    
- Images
    
- CSS
    
- JS Files
    

Useful for

- 404 Errors
    
- Slow APIs
    
- Response Time
    

---

## Sources

Contains JavaScript files.

Can add Breakpoints.

---

## Application

Shows

- Cookies
    
- Local Storage
    
- Session Storage
    

Useful for authentication debugging.

---

# Breakpoints ⭐⭐⭐⭐☆

Pause execution at a specific line.

Flow

```
Program

↓

Stops

↓

Inspect Variables

↓

Continue
```

Much better than many `console.log()` statements.

---

# Call Stack ⭐⭐⭐⭐☆

Shows how functions were called.

Example

```
main()

↓

login()

↓

validate()

↓

Error
```

Helps locate where an error originated.

---

# Stack Trace

When an error occurs, JavaScript prints the sequence of function calls leading to it.

Example

```
login()

↓

validate()

↓

TypeError
```

---

# Network Status Codes

## 200

Success

---

## 201

Created

---

## 204

Success

No content

---

## 400

Bad Request

---

## 401

Unauthorized

---

## 403

Forbidden

---

## 404

Resource not found

---

## 500

Internal Server Error

---

# API Debugging

Check

1. URL
    

```
/api/users
```

2. Method
    

```
GET

POST
```

3. Headers
    

```
Authorization

Content-Type
```

4. Body
    

```
JSON
```

5. Response
    

```
200

404

500
```

---

# Common Problems

## CORS

Error

```
Access-Control-Allow-Origin
```

Fix

```javascript
app.use(cors());
```

---

## Undefined

```javascript
console.log(user.name);
```

When

```javascript
user == null
```

Always check before accessing properties.

---

## JSON Parse Error

Invalid JSON.

Always verify request and response bodies.

---

## Async Bugs

Forgot `await`.

Wrong

```javascript
const data = fetch("/users");
```

Correct

```javascript
const data = await fetch("/users");
```

---

# React Debugging

Check

- Props
    
- State
    
- useEffect dependencies
    
- Console errors
    

Useful tool

React Developer Tools (browser extension)

---

# Node.js Debugging

Check

- Server running?
    
- Correct port?
    
- Database connected?
    
- Environment variables loaded?
    
- Route exists?
    

---

# Postman / Thunder Client

Use them to test APIs.

Example

```
GET /users

POST /users

DELETE /users
```

If Postman works but React doesn't, the issue is likely in the frontend.

---

# Debugging Process

```
Bug

↓

Reproduce

↓

Locate

↓

Understand

↓

Fix

↓

Test Again
```

---

# Common Interview Questions

### How do you debug an application?

1. Reproduce the bug.
    
2. Read the error message.
    
3. Check logs.
    
4. Use DevTools or breakpoints.
    
5. Fix the issue.
    
6. Test again.
    

---

### What is the first thing you check when an API fails?

- Status code
    
- URL
    
- Request method
    
- Request body
    
- Server logs
    

---

### Difference between Syntax, Runtime, and Logical Errors?

|Syntax|Runtime|Logical|
|---|---|---|
|Code won't run|Crashes while running|Wrong output|

---

### What is a Breakpoint?

Pauses program execution so you can inspect variables and program state.

---

### What is the Network Tab?

Shows every HTTP request and response.

---

### What is the Console?

Displays logs, warnings, and JavaScript errors.

---

# Full Forms

API → Application Programming Interface

CORS → Cross-Origin Resource Sharing

HTTP → HyperText Transfer Protocol

JSON → JavaScript Object Notation

DOM → Document Object Model

---

# Interview Checklist ✅

-  Types of Errors
    
-  Console
    
-  Browser DevTools
    
-  Elements Tab
    
-  Console Tab
    
-  Network Tab
    
-  Sources Tab
    
-  Breakpoints
    
-  Call Stack
    
-  Stack Trace
    
-  Status Codes
    
-  API Debugging
    
-  React Debugging
    
-  Node Debugging
    
-  Postman
    
-  Debugging Workflow
    

---

## ⭐⭐⭐⭐⭐ High-Priority Topics

If you're short on time, focus on:

- Browser DevTools
    
- Console
    
- Network Tab
    
- Breakpoints
    
- API Debugging
    
- Status Codes
    
- Syntax vs Runtime vs Logical Errors
    

---

### 📌 Next Topic

Only **three small topics remain**:

1. **Deployment & Cloud Basics** (Vercel, Netlify, Render, AWS basics)
    
2. **Agile & Code Reviews**
    
3. **Performance Optimization**
    

After those, you'll have covered essentially **everything** mentioned in the internship job description.