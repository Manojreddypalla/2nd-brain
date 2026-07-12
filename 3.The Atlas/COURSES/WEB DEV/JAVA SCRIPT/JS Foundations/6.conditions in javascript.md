# JavaScript Conditionals (`if`, `else`)

> [!abstract] Core Idea
> A program becomes **intelligent** when it can make decisions instead of executing every instruction sequentially.

---

# Why Do We Need Conditionals?

Without conditionals, a program simply executes statements one after another.

```text
Start
 ↓
Statement 1
 ↓
Statement 2
 ↓
Statement 3
 ↓
End
```

Every statement runs.

Real-world applications rarely work like this.

Examples:

- Login **only if** the password is correct.
- Show the admin panel **only if** the user is an admin.
- Send an OTP **only if** the phone number exists.
- Apply a discount **only if** the total exceeds ₹1000.
- Enable dark mode **only if** the user selected it.

Programs need the ability to **choose**.

---

# Mental Model

Imagine a road.

```text
           Condition
               │
         true / \ false
             /   \
        Path A   Path B
```

The program asks:

> Is this condition true?

If yes → execute the true path.

If no → execute the false path.

Only **one path executes.**

This is called **Control Flow**.

---

# Every Condition Returns a Boolean

Every condition eventually becomes one of two values.

```javascript
true
```

or

```javascript
false
```

Examples

```javascript
5 > 3
```

↓

```javascript
true
```

---

```javascript
10 < 4
```

↓

```javascript
false
```

---

```javascript
username === "John"
```

↓

```javascript
true
```

or

```javascript
false
```

---

# Syntax of `if`

```javascript
if (condition) {
    // code
}
```

Think of it as

> IF this condition is true  
> THEN execute this block.

Example

```javascript
let age = 20;

if (age >= 18) {
    console.log("Adult");
}
```

Execution

```text
age >= 18

20 >= 18

↓

true

↓

Print "Adult"
```

---

# Curly Braces `{}`

JavaScript uses `{}` in two different contexts.

## 1. Object Literal

```javascript
const user = {
    name: "Alex"
};
```

Creates an object.

---

## 2. Code Block

```javascript
if (age >= 18) {
    console.log("Adult");
}
```

Groups statements that execute together.

Context determines the meaning.

---

# Flow of Execution

```javascript
console.log("Start");

if (5 > 10) {
    console.log("Inside");
}

console.log("End");
```

Execution

```text
Start

↓

Condition

↓

false

↓

Skip block

↓

End
```

Output

```text
Start
End
```

> [!tip]
> A false condition **does not stop the program**. It only skips that block.

---

# Comparison Operators

| Operator | Meaning               | Example       |
| -------- | --------------------- | ------------- |
| `>`      | Greater Than          | `10 > 5`      |
| `<`      | Less Than             | `5 < 2`       |
| `>=`     | Greater Than or Equal | `age >= 18`   |
| `<=`     | Less Than or Equal    | `marks <= 35` |
| `==`     | Loose Equality        | `5 == "5"`    |
| `===`    | Strict Equality       | `5 === "5"`   |
| `!=`     | Loose Not Equal       | `5 != "5"`    |
| `!==`    | Strict Not Equal      | `5 !== "5"`   |

---

# Loose Equality (`==`)

JavaScript converts data types before comparing.

Example

```javascript
5 == "5"
```

Result

```javascript
true
```

Because JavaScript performs type coercion.

```text
"5"

↓

5
```

---

# Strict Equality (`===`)

JavaScript does **not** convert data types.

Example

```javascript
5 === "5"
```

Result

```javascript
false
```

Because

```text
Number
≠
String
```

> [!important]
> Always prefer **`===`** unless you intentionally want type coercion.

---

# `if...else`

```javascript
if (condition) {

} else {

}
```

Flow

```text
             Condition
             /       \
         true        false
           |            |
      if block     else block
```

Example

```javascript
let marks = 40;

if (marks >= 35) {
    console.log("Pass");
} else {
    console.log("Fail");
}
```

Only one block executes.

---

# String Comparison

```javascript
let username = "chai";
let anotherUsername = "chai";

if (username === anotherUsername) {
    console.log("Same");
}
```

Output

```text
Same
```

Applications

- Login
- Password verification
- Username validation
- OTP verification

---

# Assignment vs Comparison

## Assignment

Operator

```javascript
=
```

Stores a value.

```javascript
let x = 10;
```

---

## Loose Comparison

Operator

```javascript
==
```

Checks value only.

---

## Strict Comparison

Operator

```javascript
===
```

Checks value **and** type.

---

Wrong

```javascript
if (username = anotherUsername) {
    console.log("Same");
}
```

Correct

```javascript
if (username === anotherUsername) {
    console.log("Same");
}
```

---

# `typeof` Operator

Returns the type of a value.

Example

```javascript
let score = 44;

console.log(typeof score);
```

Output

```text
number
```

---

```javascript
let score = "44";

console.log(typeof score);
```

Output

```text
string
```

---

Checking the type

```javascript
if (typeof score === "number") {
    console.log("Number");
}
```

---

## Common Results

| Value | `typeof` |
|---------|----------|
| `10` | `"number"` |
| `"Hello"` | `"string"` |
| `true` | `"boolean"` |
| `{}` | `"object"` |
| `[]` | `"object"` ⚠️ |
| `undefined` | `"undefined"` |
| `function(){}` | `"function"` |

> [!warning]
> Arrays return `"object"` because of a historical JavaScript design decision.

---

# Boolean Variables

Instead of

```javascript
if (isTeaReady === true)
```

Write

```javascript
if (isTeaReady)
```

Because the variable already contains either `true` or `false`.

Example

```javascript
let isTeaReady = false;

if (isTeaReady) {
    console.log("Tea Ready");
} else {
    console.log("Tea Not Ready");
}
```

---

# Checking an Empty Array

Arrays have a property called `length`.

```javascript
let items = [];
```

Checking

```javascript
if (items.length === 0) {
    console.log("Empty");
}
```

If

```javascript
let items = ["Apple"];
```

Then

```javascript
items.length // 1
```

Common use cases

- Database results
- Shopping cart
- Search results
- Notifications
- Messages

---

# Dry Run

```javascript
let score = 95;

if (score > 90) {
    console.log("Excellent");
} else {
    console.log("Average");
}
```

Execution

```text
score

↓

95

↓

95 > 90

↓

true

↓

Execute if block

↓

Excellent
```

---

# Common Beginner Mistakes

## ❌ Using Assignment Instead of Comparison

Wrong

```javascript
if (a = b)
```

Correct

```javascript
if (a === b)
```

---

## ❌ Forgetting Braces

```javascript
if (x > 5)
    console.log(x);

console.log("Done");
```

Only the first statement belongs to the `if`.

Preferred

```javascript
if (x > 5) {
    console.log(x);
    console.log("Done");
}
```

---

## ❌ Comparing Booleans Unnecessarily

Instead of

```javascript
if (isReady === true)
```

Write

```javascript
if (isReady)
```

---

# Pattern to Remember

Whenever solving a problem, ask yourself:

1. What decision does my program need to make?
2. What condition returns `true` or `false`?
3. What happens if it's true?
4. What happens if it's false?

That naturally becomes

```javascript
if (condition) {
    // True path
} else {
    // False path
}
```

---

> [!success] Key Takeaway
> Conditionals are the foundation of decision-making in JavaScript.
>
> You'll use them everywhere:
>
> - Authentication
> - Authorization
> - Form validation
> - API responses
> - Error handling
> - React rendering
> - Backend logic
> - Games
> - Database operations

---
