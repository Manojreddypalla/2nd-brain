# Node.js

**Full Form:** Node.js (Not an acronym)

**Created By:** Ryan Dahl (2009)

**Purpose:** Allows JavaScript to run **outside the browser**.

Normally:

```
JavaScript     
 ↓
 
 Browser
```

With Node.js

JavaScript
      ↓
Node.js Runtime
      ↓
Computer / Server```

---

# Why Node.js?

Before Node.js

```
Frontend → JavaScript

Backend → Java
         Python
         PHP
         C#
```

After Node.js

Frontend → JavaScript

Backend → JavaScript```

One language everywhere.

---

# Runtime

Interview question:

> What is a Runtime?

A runtime is the environment that executes your code.

Examples

```
Java → JVM

Python → Python Interpreter

JavaScript → Browser

Node.js → V8 Engine + Node APIs
```

---

# V8 Engine

Created by Google.

Purpose

Converts JavaScript into machine code.

```
JavaScript

↓

V8 Engine

↓

Machine Code

↓

CPU
```

---

# Node.js Architecture

Very important interview topic.

Node.js is

- Single Threaded
- Event Driven
- Non Blocking
- Asynchronous

---

# Single Thread

One main thread handles JavaScript execution.

But...

Node.js delegates slow tasks (like file I/O or network requests) to the underlying system, allowing it to continue processing other work.

---

# Blocking Example

```
Read huge file

↓

Wait

↓

Continue
```

Everything stops until the file finishes.

---

# Non-Blocking Example ⭐⭐⭐⭐⭐

```
Read huge file

↓

Continue doing other work

↓

File finishes

↓

Handle result
```

This is why Node.js is good for web servers.

---

# Event Loop ⭐⭐⭐⭐⭐

One of the most common Node interview questions.

```
Request

↓

Event Loop

↓

Worker / OS handles slow task

↓

Callback queued

↓

Event Loop executes callback
```

The Event Loop checks if asynchronous work has completed and runs the associated callbacks when the main thread is free.

---

# Modules

Instead of one huge file

```
app.js

↓

Split into

auth.js

user.js

db.js

routes.js
```

---

## Export

```js
export function add(){}
```

---

## Import

```js
import {add} from "./math.js";
```

---

# CommonJS

Older Node syntax

```js
const fs = require("fs");
```

Modern projects typically use ES Modules (`import`/`export`).

---

# npm

**Full Form:** Node Package Manager

Used to install packages.

```
npm install express
```

---

# package.json ⭐⭐⭐⭐⭐

Most important Node file.

Contains

- Project name
- Version
- Dependencies
- Scripts

Example

```js
{"name":"project","version":"1.0.0"}
```

---

# node_modules

Contains installed packages.

Usually never edited manually.

---

# package-lock.json

Locks dependency versions.

Ensures everyone installs the same versions.

---

# File System Module (fs)

Used to read/write files.

```js
import fs from "fs";
```

Example

```js
fs.readFile()
```

---

# Path Module

Works with file paths.

```js
import path from "path";
```

---

# OS Module

Information about the operating system.

```js
import os from "os";
```

---

# HTTP Module

Create web servers without Express.

```js
import http from "http";
```

---

# Process

Represents the running Node.js application.

Example

```
process.env
```

Used for

Environment Variables

---

# Environment Variables ⭐⭐⭐⭐⭐

Store sensitive data.

```
PORT=5000DB_PASSWORD=******
```

Access

```
process.env.PORT
```

Never hardcode secrets in your source code.

---

# .env File

```
PORT=5000SECRET_KEY=abc123
```

Usually loaded using the `dotenv` package.

---

# JSON

**Full Form**

JavaScript Object Notation

Backend and frontend communicate using JSON.

Example

```js
{"name":"John","age":20}
```

---

# Creating a Server

```js
import http from "http";

const server = http.createServer((req,res)=>{

res.end("Hello");

});

server.listen(3000);
```

Express simplifies this process significantly.

---

# Asynchronous Programming

Instead of

```
Do A

↓

Wait

↓

Do B
```

Node

```
Do A

↓

Start B

↓

Continue C

↓

Finish B
```

---

# Promise

Represents future completion.

---

# Async Await

Modern way to write asynchronous code.

```js
const users=await fetch(...)
```

---

# Streams (Basic)

Useful for large files.

Instead of loading an entire file into memory, process it in chunks.

---

# Buffers (Basic)

Used for handling binary data.

Examples:

- Images
- Videos
- PDFs

---

# REPL

**Full Form:** Read Evaluate Print Loop

Interactive Node.js shell.

Run

```
node
```

Then execute JavaScript directly.

---

# Global Objects

Examples

```
consoleprocesssetTimeoutsetInterval
```

Available without importing.

---

# Important Differences

## Node.js vs Browser

|Browser|Node.js|
|---|---|
|Runs in browser|Runs on server|
|Has DOM|No DOM|
|Access to HTML/CSS|No direct DOM access|
|Limited file access|Full file system access (subject to permissions)|

---

## Blocking vs Non-Blocking

|Blocking|Non-Blocking|
|---|---|
|Waits for task|Continues executing other tasks|
|Slower for many concurrent requests|Better concurrency|

---

## CommonJS vs ES Modules

|CommonJS|ES Modules|
|---|---|
|`require()`|`import`|
|`module.exports`|`export`|
|Older syntax|Modern standard|

---

# Full Forms

Node.js → (Name, not an acronym)

V8 → JavaScript engine by Google

npm → Node Package Manager

JSON → JavaScript Object Notation

REPL → Read Evaluate Print Loop

API → Application Programming Interface

---

# Interview Questions

- What is Node.js?
- Why use Node.js?
- Is Node.js a programming language?
- What is the V8 Engine?
- What is the Event Loop?
- What is asynchronous programming?
- What is non-blocking I/O?
- What is `package.json`?
- What is `node_modules`?
- What are environment variables?
- Why use a `.env` file?
- Difference between CommonJS and ES Modules?

---

# 📌 Priority Order

1. What is Node.js? ⭐⭐⭐⭐⭐
2. Runtime & V8 Engine ⭐⭐⭐⭐⭐
3. Event Loop ⭐⭐⭐⭐⭐
4. Asynchronous Programming ⭐⭐⭐⭐⭐
5. npm ⭐⭐⭐⭐⭐
6. `package.json` ⭐⭐⭐⭐⭐
7. Modules (`import`/`export`) ⭐⭐⭐⭐☆
8. Environment Variables & `.env` ⭐⭐⭐⭐⭐
9. Built-in Modules (`fs`, `path`, `http`) ⭐⭐⭐☆
10. Streams & Buffers (basic awareness) ⭐⭐☆☆☆

---

## 🎯 For your internship

Based on the job description, interviewers are much more likely to ask:

- "What is Node.js?"
- "Why is Node.js good for backend development?"
- "What is the Event Loop?"
- "What is `package.json`?"
- "How does React communicate with a Node backend?"
- "How do you store API keys securely?"

You **do not** need to know the internals of V8 garbage collection or advanced stream implementations for an entry-level full-stack role.

➡️ **Next:** **Express.js**, where we'll build actual REST APIs, handle requests and responses, use middleware, and connect Node.js to databases. This is the framework you'll use most often with Node in these internships.