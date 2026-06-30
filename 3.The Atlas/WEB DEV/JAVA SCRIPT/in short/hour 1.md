These first 60 minutes build the **foundation of JavaScript**. Almost everything in React, Node.js, and frontend development depends on understanding these concepts. Don't just memorize syntax—understand **what JavaScript is storing in memory and how it executes code**.

---

# 1. Running JavaScript

JavaScript can run in:

- Browser (Chrome, Firefox, Edge)
    
- Node.js (outside browser)
    

Example:

```javascript
console.log("Hello World");
```

Output:

```
Hello World
```

`console.log()` prints data to the console.

---

# 2. Comments

Comments are ignored by JavaScript.

### Single Line

```javascript
// This is a comment
```

### Multi Line

```javascript
/*
This
is
a
comment
*/
```

---

# 3. Variables

A variable is simply **a named container that stores a value in memory**.

Think of memory like this:

```
Memory

name  ----------> "John"

age   ----------> 20
```

JavaScript gives names to memory locations.

---

## Declaring Variables

```javascript
var name;
```

This creates a variable.

Memory:

```
name

↓

undefined
```

Nothing is stored yet.

---

## Assigning Values

```javascript
var name;

name = "John";
```

Memory becomes

```
name

↓

"John"
```

---

## Declare + Assign Together

```javascript
var age = 20;
```

Equivalent to

```javascript
var age;

age = 20;
```

---

# 4. var vs let vs const

## var

Old way.

```javascript
var x = 10;
```

Can redeclare.

```javascript
var x = 10;
var x = 20;
```

Allowed.

Can also change value.

```javascript
x = 50;
```

---

## let

Modern.

```javascript
let x = 10;
```

Cannot redeclare.

```javascript
let x = 10;
let x = 20;
```

❌ Error

Can change value.

```javascript
x = 30;
```

---

## const

Constant.

```javascript
const PI = 3.14;
```

Cannot reassign.

```javascript
PI = 5;
```

❌ Error

---

### Interview

|Feature|var|let|const|
|---|---|---|---|
|Redeclare|✅|❌|❌|
|Reassign|✅|✅|❌|
|Block Scope|❌|✅|✅|

---

# 5. Undefined Variables

```javascript
let x;

console.log(x);
```

Output

```
undefined
```

Because memory exists.

No value yet.

---

Difference

```
undefined

↓

Variable exists
```

```
ReferenceError

↓

Variable doesn't exist
```

---

# 6. Case Sensitivity

```javascript
let age = 20;

let Age = 25;
```

These are different variables.

JavaScript is case-sensitive.

```
name

Name

NAME

naMe
```

All different.

---

# 7. Basic Math

Addition

```javascript
5 + 3
```

Subtraction

```javascript
10 - 2
```

Multiplication

```javascript
5 * 2
```

Division

```javascript
10 / 2
```

---

# 8. Increment

Instead of

```javascript
count = count + 1;
```

Use

```javascript
count++;
```

---

# 9. Decrement

Instead of

```javascript
count = count - 1;
```

Use

```javascript
count--;
```

---

# 10. Decimal Numbers

JavaScript has one Number type.

```javascript
let x = 5.6;

let y = 10;
```

Both are Number.

No separate float/int.

---

# 11. Multiply Decimals

```javascript
2.5 * 3
```

Output

```
7.5
```

---

# 12. Divide Decimals

```javascript
5.5 / 2
```

---

# 13. Modulus Operator (%)

Returns remainder.

```
10 % 3

↓

1
```

```
8 % 2

↓

0
```

Useful for checking even/odd.

```javascript
number % 2 == 0
```

Even

---

# 14. Augmented Assignment

Instead of

```javascript
x = x + 5;
```

Use

```javascript
x += 5;
```

Similarly

```javascript
x -= 2;

x *= 5;

x /= 4;
```

---

# 15. Strings

Strings are text.

```javascript
let name = "John";
```

---

Single Quotes

```javascript
'Hello'
```

Double Quotes

```javascript
"Hello"
```

Both work.

---

# 16. Escaping Quotes

Wrong

```javascript
"He said "Hi""
```

Correct

```javascript
"He said \"Hi\""
```

---

# 17. Escape Characters

```
\n

New line

\t

Tab

\\

Backslash

\"

Double Quote

\'

Single Quote
```

Example

```javascript
console.log("Hello\nWorld");
```

Output

```
Hello
World
```

---

# 18. String Concatenation

```javascript
"Hello" + "World"
```

↓

```
HelloWorld
```

Usually

```javascript
"Hello " + "World"
```

↓

```
Hello World
```

---

# 19. += with Strings

```javascript
let str = "Hello";

str += " World";
```

Output

```
Hello World
```

---

# 20. Variables Inside Strings

```javascript
let name = "Alex";

let msg = "Hello " + name;
```

Output

```
Hello Alex
```

---

# 21. String Length

```javascript
let name = "John";

name.length
```

↓

```
4
```

Counts characters.

```
H

e

l

l

o

↓

5
```

---

# 22. Bracket Notation

Strings behave like arrays.

```
H

e

l

l

o

0

1

2

3

4
```

```javascript
let word = "Hello";

word[0]
```

↓

```
H
```

---

# 23. Strings are Immutable

Cannot change individual characters.

```javascript
let str = "Hello";

str[0] = "Y";
```

Nothing happens.

Need

```javascript
str = "Yello";
```

---

# 24. Find nth Character

```javascript
let word = "JavaScript";

word[4]
```

↓

```
S
```

---

Last Character

```javascript
word[word.length - 1]
```

---

Second Last

```javascript
word[word.length - 2]
```

---

# 25. Arrays

Arrays store multiple values.

```javascript
let fruits = ["Apple", "Banana", "Orange"];
```

Memory

```
fruits

↓

["Apple","Banana","Orange"]
```

---

# 26. Nested Arrays

```javascript
[
 [1,2],
 [3,4],
 [5,6]
]
```

Useful later for matrices.

---

# 27. Access Array

```javascript
let arr = [10,20,30];

arr[0]
```

↓

```
10
```

---

# 28. Modify Array

```javascript
arr[1] = 50;
```

Now

```
[10,50,30]
```

Unlike strings, arrays are mutable.

---

# 29. Multi-Dimensional Arrays

```javascript
let arr = [
 [1,2],
 [3,4]
];

arr[0][1]
```

↓

```
2
```

---

# 30. Array Methods

### push()

Adds to end.

```javascript
arr.push(5);
```

Before

```
[1,2,3]
```

After

```
[1,2,3,5]
```

---

### pop()

Removes last.

```javascript
arr.pop();
```

---

### shift()

Removes first.

```javascript
arr.shift();
```

---

### unshift()

Adds at beginning.

```javascript
arr.unshift(100);
```

---

# 31. Functions

A function is reusable code.

Instead of

```javascript
console.log("Hello");

console.log("Hello");

console.log("Hello");
```

Use

```javascript
function greet() {
    console.log("Hello");
}
```

Call it

```javascript
greet();
```

---

# 32. Parameters

```javascript
function greet(name) {
    console.log(name);
}
```

Call

```javascript
greet("Alex");
```

Output

```
Alex
```

`name` is called a **parameter**.

`"Alex"` is the **argument**.

---

# 33. Global Scope

Declared outside every function.

```javascript
let x = 10;

function test() {
    console.log(x);
}
```

Works everywhere.

Memory:

```
Global

x → 10

test()
```

---

# 34. Local Scope

Declared inside a function.

```javascript
function test() {
    let y = 20;
}
```

Outside

```javascript
console.log(y);
```

❌ Error

`y` exists only while the function runs.

---

# 35. Global vs Local

```javascript
let x = 10;

function demo() {
    let y = 20;

    console.log(x);
    console.log(y);
}

demo();

console.log(x);

console.log(y);
```

Output:

```
10
20
10
ReferenceError
```

- `x` is global, so it's accessible everywhere.
    
- `y` is local to `demo()`, so it disappears after the function finishes.
    

---

# 🎯 Interview Questions from This Section

Be ready to answer:

- Difference between `var`, `let`, and `const`.
    
- What is `undefined`?
    
- What is the difference between `undefined` and `ReferenceError`?
    
- What does `%` (modulus) do?
    
- Difference between strings and arrays (immutability vs mutability).
    
- What is the difference between a parameter and an argument?
    
- What is variable scope?
    
- What is the difference between global scope and local scope?
    
- How do `push()`, `pop()`, `shift()`, and `unshift()` work?
    
- How do you find the last character of a string?
    

If you master these fundamentals, you'll have the base needed to understand more advanced topics like objects, closures, the DOM, asynchronous JavaScript, React hooks, and Node.js.