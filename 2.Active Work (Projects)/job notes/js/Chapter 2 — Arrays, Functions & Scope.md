# JavaScript Master Notes

# Chapter 2 — Arrays, Functions & Scope

---

# Topics Covered

- Arrays
    
- Nested Arrays
    
- Accessing & Modifying Arrays
    
- `push()`
    
- `pop()`
    
- `shift()`
    
- `unshift()`
    
- Functions
    
- Parameters vs Arguments
    
- Return Values
    
- Global Scope
    
- Local Scope
    
- Global vs Local Variables
    
- Execution Context
    
- Call Stack
    

---

# 1. Arrays

## Intuition

Imagine you have multiple related values.

Without an array:

```javascript
let student1 = "Alex";
let student2 = "John";
let student3 = "Emma";
```

This quickly becomes difficult to manage.

Instead, JavaScript provides an **Array**, which stores multiple values together.

```javascript
let students = ["Alex", "John", "Emma"];
```

Think of an array as a row of numbered boxes.

```
Index

 0        1        2
+------+-------+------+
| Alex | John  | Emma |
+------+-------+------+
```

---

# 2. Array Index

Arrays start at **0**, not 1.

```javascript
let colors = ["Red", "Green", "Blue"];
```

```
Index

0 → Red
1 → Green
2 → Blue
```

Accessing values:

```javascript
console.log(colors[0]);
```

Output

```
Red
```

---

# 3. Modifying an Array

```javascript
let fruits = ["Apple", "Banana", "Orange"];

fruits[1] = "Mango";
```

Memory before

```
0 Apple
1 Banana
2 Orange
```

Memory after

```
0 Apple
1 Mango
2 Orange
```

Unlike strings, arrays are **mutable**.

---

# 4. Nested Arrays

Arrays can contain arrays.

```javascript
let matrix = [
    [1,2,3],
    [4,5,6],
    [7,8,9]
];
```

Visual

```
matrix

↓

[
 [1 2 3]
 [4 5 6]
 [7 8 9]
]
```

Access

```javascript
matrix[1][2]
```

Explanation

```
matrix[1]

↓

[4,5,6]

↓

index 2

↓

6
```

---

# 5. push()

Adds an element to the **end**.

```javascript
let nums = [1,2,3];

nums.push(4);
```

Before

```
1 2 3
```

After

```
1 2 3 4
```

Time Complexity

```
O(1)
```

---

# 6. pop()

Removes the last element.

```javascript
nums.pop();
```

Before

```
1 2 3 4
```

After

```
1 2 3
```

It also returns the removed value.

```javascript
let x = nums.pop();
```

---

# 7. shift()

Removes the first element.

```javascript
let arr = [10,20,30];

arr.shift();
```

Result

```
20 30
```

Internally every remaining element shifts left.

```
10 20 30

↓

20 30
```

Time Complexity

```
O(n)
```

---

# 8. unshift()

Adds an element to the beginning.

```javascript
arr.unshift(5);
```

Before

```
20 30
```

After

```
5 20 30
```

Time Complexity

```
O(n)
```

Every element moves one position.

---

# Array Method Summary

|Method|Purpose|
|---|---|
|push()|Add at end|
|pop()|Remove from end|
|shift()|Remove first|
|unshift()|Add first|

---

# 9. Functions

## Intuition

Functions are reusable blocks of code.

Without functions:

```javascript
console.log("Hello");
console.log("Hello");
console.log("Hello");
```

With functions

```javascript
function greet(){
    console.log("Hello");
}

greet();
greet();
greet();
```

Think of a function as a machine.

```
Input

↓

Machine

↓

Output
```

---

# 10. Declaring a Function

```javascript
function greet(){
    console.log("Hello");
}
```

Calling it

```javascript
greet();
```

Output

```
Hello
```

---

# 11. Parameters vs Arguments

Function

```javascript
function square(number){
    return number * number;
}
```

Here

```
number

↓

Parameter
```

Calling

```javascript
square(5);
```

```
5

↓

Argument
```

Easy rule

- Parameter → variable in function definition
    
- Argument → value passed during call
    

---

# 12. Multiple Parameters

```javascript
function add(a,b){
    return a+b;
}

add(10,20);
```

Execution

```
a = 10

b = 20

↓

30
```

---

# 13. Return

Imagine a vending machine.

```
Money

↓

Machine

↓

Snack
```

Functions return values similarly.

```javascript
function cube(x){
    return x*x*x;
}
```

```javascript
let ans = cube(3);
```

Memory

```
ans

↓

27
```

---

# 14. return Stops Execution

```javascript
function test(){

    return 5;

    console.log("Hello");
}
```

The `console.log()` never runs.

Execution ends immediately after `return`.

---

# 15. Functions Without Return

```javascript
function greet(){

    console.log("Hi");
}
```

```javascript
let x = greet();
```

Output

```
Hi
```

Value stored in x

```
undefined
```

Because every function returns something.

If you don't specify it, JavaScript returns

```
undefined
```

---

# 16. Execution Context

This is one of the most important concepts.

When JavaScript calls a function, it creates a **new execution context**.

Imagine giving every function its own private desk.

```
Function

↓

Private Workspace

↓

Variables

↓

Execute

↓

Destroy Workspace
```

Each call gets a fresh workspace.

---

# 17. Call Stack

Every function call goes onto the **Call Stack**.

Example

```javascript
function A(){
    B();
}

function B(){
    C();
}

function C(){
    console.log("Done");
}

A();
```

Stack

```
Top

C()

↓

B()

↓

A()

↓

Global

Bottom
```

After C finishes

```
B()

↓

A()

↓

Global
```

Then

```
A()

↓

Global
```

Finally

```
Global
```

This **Last In, First Out (LIFO)** behavior is exactly like recursive function calls in C++.

---

# 18. Global Scope

Variables declared outside every function are global.

```javascript
let country = "India";

function show(){

    console.log(country);
}
```

Output

```
India
```

Every function can access global variables.

---

# 19. Local Scope

```javascript
function test(){

    let age = 20;
}
```

Outside

```javascript
console.log(age);
```

Error

```
ReferenceError
```

The variable is destroyed when the function finishes.

---

# 20. Global vs Local

```javascript
let x = 10;

function demo(){

    let x = 20;

    console.log(x);
}

demo();

console.log(x);
```

Output

```
20

10
```

The local variable shadows the global variable.

---

# 21. Variable Lookup

JavaScript searches for variables in this order:

```
Current Function

↓

Outer Scope

↓

Global Scope

↓

Not Found

↓

ReferenceError
```

This is the foundation of the **scope chain**.

---

# Dry Run

```javascript
let total = 100;

function discount(price){

    let result = total - price;

    return result;
}

console.log(discount(20));
```

Step 1

```
Global

total = 100
```

Step 2

```
Call discount(20)

New Execution Context

price = 20
```

Step 3

```
result = 100 - 20

↓

80
```

Step 4

```
return 80
```

Step 5

Execution context is destroyed.

Only the global context remains.

---

# Common Mistakes

### Forgetting `return`

```javascript
function add(a,b){

    a+b;
}
```

Returns

```
undefined
```

Correct

```javascript
function add(a,b){

    return a+b;
}
```

---

### Accessing Local Variables

```javascript
function test(){

    let x = 10;
}

console.log(x);
```

Produces a `ReferenceError`.

---

### Thinking Arrays Are Immutable

```javascript
const arr = [1,2,3];

arr.push(4);
```

This works.

`const` prevents reassignment of the **reference**, not mutation of the array's contents.

---

# C++ Comparison

|C++|JavaScript|
|---|---|
|`vector<int>`|`Array`|
|Function call stack|Call Stack|
|Local variables|Local Scope|
|Global variables|Global Scope|
|`return`|`return`|
|Parameters|Parameters|

---

# Quick Revision

## Arrays

```javascript
let arr = [1,2,3];
```

## Access

```javascript
arr[0]
```

## Modify

```javascript
arr[1] = 10;
```

## Push

```javascript
arr.push(5);
```

## Pop

```javascript
arr.pop();
```

## Shift

```javascript
arr.shift();
```

## Unshift

```javascript
arr.unshift(100);
```

## Function

```javascript
function add(a,b){

    return a+b;
}
```

## Global

```javascript
let x = 10;
```

## Local

```javascript
function test(){

    let y = 20;
}
```

---

# Interview Questions

1. Why do arrays start at index 0?
    
2. What is the difference between `push()` and `unshift()`?
    
3. Why is `shift()` slower than `pop()`?
    
4. What is the difference between a parameter and an argument?
    
5. What happens internally when a function is called?
    
6. What is an execution context?
    
7. What is the call stack?
    
8. What is the difference between global and local scope?
    
9. Why does a function without `return` return `undefined`?
    
10. Why can a `const` array still be modified?