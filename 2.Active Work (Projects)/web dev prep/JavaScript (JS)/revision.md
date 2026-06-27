Now we reach the **most important frontend topic**.

> If HTML is the skeleton and CSS is the appearance, **JavaScript is the brain**.

Most frontend interviews spend **more time on JavaScript than on HTML + CSS combined**.

---

# JavaScript (JS)

**Full Form:** JavaScript

**Purpose:** Adds logic, interactivity, and dynamic behavior to web pages.

Examples:

- Login validation
    
- Buttons
    
- Image sliders
    
- API calls
    
- Games
    
- Forms
    
- Chat applications
    

---

# How JavaScript Works

When a webpage loads:

```text
HTML  → Creates Structure
CSS   → Styles the Structure
JS    → Adds Behavior
```

Example:

```html
<button id="btn">Click Me</button>
```

```javascript
document.getElementById("btn").addEventListener("click", () => {
    alert("Hello");
});
```

Without JavaScript:

- Button exists
    
- Nothing happens
    

With JavaScript:

- Button responds to clicks
    

---

# Variables ⭐⭐⭐⭐⭐

Store data.

## var

```javascript
var name = "John";
```

Old way.

Problems:

- Function scoped
    
- Can be redeclared
    
- Can cause bugs
    

Rarely used today.

---

## let ⭐⭐⭐⭐⭐

```javascript
let age = 20;
```

- Block scoped
    
- Can change value
    

```javascript
let x = 5;
x = 10;
```

---

## const ⭐⭐⭐⭐⭐

```javascript
const pi = 3.14;
```

- Block scoped
    
- Cannot be reassigned
    

```javascript
const name = "John";
```

---

## Interview

### var vs let vs const

|var|let|const|
|---|---|---|
|Function scope|Block scope|Block scope|
|Redeclare ✓|✗|✗|
|Reassign ✓|✓|✗|

Use `const` by default. Use `let` when the value changes.

---

# Data Types ⭐⭐⭐⭐⭐

Primitive:

```javascript
String
Number
Boolean
Undefined
Null
BigInt
Symbol
```

Example

```javascript
let name = "Alice";
let age = 21;
let isStudent = true;
let salary = null;
let x;
```

---

# Operators

Arithmetic

```javascript
+
-
*
/
%
**
```

Comparison

```javascript
==
===
!=
!==
<
>
<=
>=
```

Logical

```javascript
&&
||
!
```

---

# == vs === ⭐⭐⭐⭐⭐

```javascript
5 == "5"
```

Returns

```text
true
```

Because only value is compared.

---

```javascript
5 === "5"
```

Returns

```text
false
```

Because value **and type** are compared.

Always prefer `===`.

---

# Functions ⭐⭐⭐⭐⭐

## Normal Function

```javascript
function add(a, b) {
    return a + b;
}
```

---

## Arrow Function ⭐⭐⭐⭐⭐

```javascript
const add = (a, b) => {
    return a + b;
};
```

Short version:

```javascript
const square = x => x * x;
```

---

# Objects ⭐⭐⭐⭐⭐

Store related data.

```javascript
const user = {
    name: "John",
    age: 21
};
```

Access

```javascript
user.name
```

or

```javascript
user["name"]
```

---

# Arrays ⭐⭐⭐⭐⭐

Store multiple values.

```javascript
const nums = [1,2,3];
```

Access

```javascript
nums[0]
```

---

# Array Methods ⭐⭐⭐⭐⭐

## map()

Transforms every element.

```javascript
const nums = [1,2,3];

nums.map(x => x * 2);
```

Output

```text
2 4 6
```

---

## filter()

Keeps matching elements.

```javascript
nums.filter(x => x > 2);
```

---

## find()

Returns first match.

```javascript
nums.find(x => x == 3);
```

---

## forEach()

Loops through every element.

```javascript
nums.forEach(num => {
    console.log(num);
});
```

---

## reduce()

Combines values.

```javascript
nums.reduce((sum, x) => sum + x, 0);
```

---

# Loops

```javascript
for
while
do while
for...of
for...in
```

Know:

`for...of`

Iterates over array values.

`for...in`

Iterates over object keys.

---

# Conditionals

```javascript
if

else

switch

ternary operator
```

---

# DOM ⭐⭐⭐⭐⭐⭐⭐

**Full Form:** Document Object Model

The browser converts HTML into a tree of objects that JavaScript can manipulate.

Example HTML:

```html
<h1 id="title">Hello</h1>
```

JavaScript:

```javascript
const heading = document.getElementById("title");
heading.textContent = "Welcome";
```

---

# Selecting Elements

```javascript
document.getElementById()

document.querySelector()

document.querySelectorAll()
```

---

# Changing Content

```javascript
element.textContent

element.innerHTML
```

---

# Changing Style

```javascript
element.style.color = "red";
```

---

# Event Listeners ⭐⭐⭐⭐⭐

```javascript
button.addEventListener("click", function(){

});
```

Common Events

```text
click
submit
change
keydown
keyup
mouseover
mouseout
```

---

# Promises ⭐⭐⭐⭐⭐

A Promise represents a value that will be available **later**.

States:

```text
Pending

↓

Fulfilled

or

Rejected
```

Example:

```javascript
fetch("/users")
```

The data is not available immediately.

---

# Async / Await ⭐⭐⭐⭐⭐

Modern way to work with asynchronous code.

```javascript
async function getUsers() {
    const res = await fetch("/users");
    const data = await res.json();
}
```

Much cleaner than nested callbacks.

---

# Fetch API ⭐⭐⭐⭐⭐

Used to call backend APIs.

```javascript
fetch("/api/users")
```

Methods

```text
GET
POST
PUT
DELETE
```

---

# JSON ⭐⭐⭐⭐⭐

**Full Form:** JavaScript Object Notation

Used to exchange data between frontend and backend.

Example

```json
{
    "name":"John",
    "age":20
}
```

---

# Modules

```javascript
export

import
```

Example

```javascript
export default add;
```

```javascript
import add from "./math.js";
```

---

# Destructuring ⭐⭐⭐⭐☆

```javascript
const user = {
    name:"John",
    age:20
};

const {name, age} = user;
```

---

# Spread Operator ⭐⭐⭐⭐☆

```javascript
const arr = [...nums];
```

Copies arrays or objects.

---

# Rest Operator ⭐⭐⭐⭐☆

```javascript
function sum(...nums){

}
```

Collects multiple arguments into one array.

---

# Template Literals ⭐⭐⭐⭐☆

```javascript
const name = "John";

console.log(`Hello ${name}`);
```

---

# Closures ⭐⭐⭐⭐☆

Interview favorite.

A closure is a function that remembers variables from its outer scope even after the outer function has finished.

Basic example:

```javascript
function outer() {
    let count = 0;

    return function () {
        count++;
        return count;
    };
}

const counter = outer();

counter(); // 1
counter(); // 2
```

---

# Hoisting ⭐⭐⭐⭐☆

JavaScript moves declarations to the top of their scope before execution.

Important points:

- `var` is hoisted and initialized with `undefined`.
    
- `let` and `const` are hoisted but cannot be used before declaration (Temporal Dead Zone).
    

---

# Scope ⭐⭐⭐⭐☆

- Global Scope
    
- Function Scope
    
- Block Scope
    

`let` and `const` are block-scoped.

---

# Important Differences

## let vs const

|let|const|
|---|---|
|Can reassign|Cannot reassign|

---

## == vs ===

|==|===|
|---|---|
|Loose comparison|Strict comparison|
|Converts types|No type conversion|

---

## map vs forEach

|map|forEach|
|---|---|
|Returns new array|Returns undefined|
|Used for transformation|Used for side effects|

---

## null vs undefined

|null|undefined|
|---|---|
|Intentionally empty|Variable declared but not assigned|

---

# Full Forms

JS → JavaScript

DOM → Document Object Model

JSON → JavaScript Object Notation

API → Application Programming Interface

---

# Interview Questions

- Difference between `var`, `let`, and `const`?
    
- Difference between `==` and `===`?
    
- What is the DOM?
    
- What is an event listener?
    
- What is a Promise?
    
- What is `async/await`?
    
- What is JSON?
    
- Difference between `map()` and `forEach()`?
    
- What is a closure?
    
- What is hoisting?
    
- What is the Fetch API?
    
- Difference between `null` and `undefined`?
    

---

# 📌 Priority Order

1. Variables (`let`, `const`, `var`)
    
2. Data Types
    
3. Functions & Arrow Functions
    
4. Objects & Arrays
    
5. Array Methods (`map`, `filter`, `reduce`, `find`, `forEach`)
    
6. DOM Manipulation
    
7. Event Listeners
    
8. Promises
    
9. `async/await`
    
10. Fetch API & JSON
    
11. Modules (`import`/`export`)
    
12. Destructuring & Spread
    
13. Closures
    
14. Hoisting
    

---

This is the **JavaScript knowledge expected for a full-stack internship**. If you're comfortable with everything above, you're in a strong position to learn React because React is fundamentally built on these JavaScript concepts. The next topic will be **TypeScript**, where you'll see how JavaScript is extended with static typing to make larger applications easier to build and maintain.