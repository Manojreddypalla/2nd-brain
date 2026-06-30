# 📘 JavaScript Master Notes

# Chapter 1 — JavaScript Fundamentals

**Topics Covered**

- Introduction
- Running JavaScript
- Comments
- Variables
- Assignment
- Data Types
- Operators
- Strings

---

# 1. What is JavaScript?

## Intuition

Imagine a web page.

HTML builds the skeleton.

CSS paints it.

JavaScript gives it a brain.

Without JavaScript

```
Button
```

With JavaScript

```
Click Button↓Open Menu↓Send Request↓Update Screen
```

JavaScript makes websites **interactive**.

---

## Where JavaScript Runs

Originally

```
BrowserChromeFirefoxEdgeSafari
```

Today

It runs almost everywhere.

```
Browser↓Node.js↓Servers↓Desktop Apps↓Mobile Apps↓IoT
```

---

# JavaScript Engine

Every browser contains an engine.

Chrome

```
V8 Engine
```

Firefox

```
SpiderMonkey
```

Safari

```
JavaScriptCore
```

The engine converts your code into machine instructions.

```
Your Code↓Parser↓AST↓Compiler↓Machine Code↓CPU
```

You don't compile manually like C++.

---

# C++ vs JavaScript

C++

```
Source Code↓Compile↓Executable↓Run
```

JavaScript

```
Source Code↓Engine↓Run Immediately
```

---

# Running JavaScript

### Browser Console

Press

```
F12↓Console
```

```
console.log("Hello");
```

Output

```
Hello
```

---

### HTML

```
<script>console.log("Hello");</script>
```

---

### External File

```
<script src="app.js"></script>
```

---

### Node.js

```
node app.js
```

---

# Comments

Single Line

```
// this is comment
```

Multi Line

```
/*HelloWorld*/
```

Why?

Comments are ignored by the engine.

They're for humans.

---

# Variables

## Mental Model

Think of RAM.

```
RAM+----------------+|                ||                ||                |+----------------+
```

A variable reserves space.

```
let age = 22;
```

Memory

```
age↓22
```

The variable is simply a **name attached to a value**.

---

## Why Variables Exist

Without variables

```
console.log(22);console.log(22);console.log(22);
```

With variables

```
let age = 22;console.log(age);
```

Much cleaner.

---

# Declaration

```
let age;
```

JavaScript creates

```
age↓undefined
```

Nothing assigned yet.

---

# Assignment

```
age = 22;
```

Memory becomes

```
age↓22
```

---

# Declaration + Assignment

```
let age = 22;
```

Happens together.

---

# Reassignment

```
let age = 22;age = 23;
```

Memory

Before

```
age↓22
```

After

```
age↓23
```

---

# Variable Naming Rules

Allowed

```
let myAge;let age2;let _name;let $value;
```

Not allowed

```
let 2age;let my-name;let let;
```

---

# Case Sensitive

```
let age = 20;let Age = 40;
```

Different variables.

```
age↓20Age↓40
```

---

# Data Types

JavaScript stores different kinds of values.

## Number

```
let x = 10;
```

```
103.14-25
```

No separate

```
intfloatdouble
```

Everything is Number.

---

## String

```
let name = "John";
```

Collection of characters.

---

## Boolean

```
truefalse
```

Represents yes/no.

---

## Undefined

```
let x;
```

```
undefined
```

Means

> declared but not assigned.

---

## Null

```
let x = null;
```

Means

"I intentionally made it empty."

---

## Object

```
let student = {name:"Alex",age:22}
```

Stores multiple values.

---

## Symbol

Unique identifier.

Mostly advanced JavaScript.

---

## BigInt

Very large integers.

```
12345678901234567890n
```

---

# typeof

Know the type.

```
typeof 10
```

Returns

```
number
```

```
typeof "Hello"
```

```
string
```

```
typeof true
```

```
boolean
```

---

# Math Operators

Addition

```
5+2
```

```
7
```

Subtraction

```
5-2
```

```
3
```

Multiplication

```
5*2
```

```
10
```

Division

```
6/2
```

```
3
```

---

# Modulus %

Returns remainder.

```
10%3
```

```
1
```

Visual

```
3 |10331 left
```

Used for

Even

```
n%2==0
```

Odd

```
n%2!=0
```

---

# Increment

```
let x=5;x++;
```

Equivalent

```
x=x+1;
```

---

# Decrement

```
x--;
```

Equivalent

```
x=x-1;
```

---

# Compound Assignment

Instead of

```
x=x+5;
```

Use

```
x+=5;
```

Similarly

```
x-=5;x*=5;x/=5;
```

---

# Strings

A string is simply text.

```
let city="Hyderabad";
```

Memory

```
city↓"Hyderabad"
```

---

# Quotes

Double

```
"Hello"
```

Single

```
'Hello'
```

Both valid.

---

# Escaping Quotes

Wrong

```
"He said "Hello""
```

Correct

```
"He said \"Hello\""
```

---

# Escape Sequences

New Line

```
"\n"
```

Tab

```
"\t"
```

Backslash

```
"\\"
```

Quote

```
"\""
```

---

# String Concatenation

```
let first="John";let last="Doe";first+" "+last
```

Output

```
John Doe
```

---

# += with Strings

```
let msg="Hello";msg+=" World";
```

```
Hello World
```

---

# Variables in Strings

```
let name="Alex";let age=22;"My name is "+name
```

Output

```
My name is Alex
```

---

# String Length

```
let s="Hello";s.length
```

Returns

```
5
```

Memory

```
H e l l o0 1 2 3 4
```

---

# Indexing

```
let s="Hello";s[0]
```

```
H
```

```
s[1]
```

```
e
```

Last character

```
s[s.length-1]
```

```
o
```

---

# Strings are Immutable

This surprises C++ programmers.

```
let s="Hello";s[0]="Y";
```

Nothing happens.

Strings cannot be modified character by character.

Instead

```
s="Yello";
```

A **new string** is created, and `s` now refers to that new string.

---

# Memory Model

```
let a = "Hello";let b = a;
```

```
a ──► "Hello"b ──► "Hello"
```

Now:

```
b = "World";
```

```
a ──► "Hello"b ──► "World"
```

`a` is unchanged because strings are immutable.

---

# Mini Cheat Sheet

|Concept|Example|
|---|---|
|Variable|`let age = 22;`|
|Assignment|`age = 25;`|
|Number|`10`, `3.14`|
|String|`"Hello"`|
|Boolean|`true`|
|Undefined|`let x;`|
|Null|`let x = null;`|
|Addition|`a + b`|
|Modulus|`10 % 3`|
|Increment|`x++`|
|Length|`str.length`|
|First Character|`str[0]`|
|Last Character|`str[str.length - 1]`|

---

# Common Interview Questions

1. What is the difference between `undefined` and `null`?
2. Why are JavaScript strings immutable?
3. Explain how `%` works.
4. What is the difference between declaration and assignment?
5. Why is JavaScript called a dynamically typed language?
6. What does `typeof null` return, and why is it considered a historical quirk? _(Answer: `"object"`.)_

---

### What's next?

The next chapter will cover:

- Arrays
- Nested Arrays
- `push()`, `pop()`, `shift()`, `unshift()`
- Functions
- Parameters vs Arguments
- Return values
- Scope (`global` vs `local`)
- Execution context
- The call stack (what happens internally when functions are called)

This is where JavaScript starts becoming much more powerful, and we'll connect it to concepts you already