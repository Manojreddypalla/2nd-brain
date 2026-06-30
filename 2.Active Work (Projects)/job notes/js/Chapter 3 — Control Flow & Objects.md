# JavaScript Master Notes

# Chapter 3 — Control Flow & Objects (Part 1)

---

# Topics Covered

- Boolean
    
- Truthy & Falsy
    
- Comparison Operators
    
- if Statement
    
- else
    
- else if
    
- Logical Operators
    
- Nested Conditions
    
- Returning Booleans
    
- Early Return Pattern
    

---

# 1. Why Do We Need Conditions?

Until now, our programs executed every line in order.

```javascript
console.log("Step 1");
console.log("Step 2");
console.log("Step 3");
```

Output

```
Step 1
Step 2
Step 3
```

But real programs make **decisions**.

Examples:

- Is the user logged in?
    
- Is the password correct?
    
- Is the player dead?
    
- Is the bank balance enough?
    
- Did the API return success?
    

A program needs to choose different paths.

Think of it like a road.

```
                Start
                  |
             Is it raining?
              /          \
           Yes            No
           |              |
     Take Umbrella     Go Outside
```

This branching is called **Control Flow**.

---

# 2. Boolean Values

A Boolean represents only **two possible states**.

```javascript
true
false
```

Nothing else.

Imagine a light switch.

```
OFF

↓

false

ON

↓

true
```

---

## Examples

```javascript
let isLoggedIn = true;
let isAdmin = false;
```

---

## Memory

```
isLoggedIn

↓

true

isAdmin

↓

false
```

---

# 3. Why Boolean Exists

Imagine checking login.

Without Boolean

```javascript
let login = "yes";
```

What about

```
YES
Yes
true
1
okay
```

Confusing.

Boolean removes ambiguity.

```
true

or

false
```

---

# 4. Comparison Operators

Comparison operators compare two values.

Result?

Always

```
true

or

false
```

---

## Equal

```javascript
5 == 5
```

Output

```
true
```

---

## Not Equal

```javascript
5 != 4
```

```
true
```

---

## Greater Than

```javascript
10 > 3
```

```
true
```

---

## Less Than

```javascript
3 < 10
```

```
true
```

---

## Greater Than or Equal

```javascript
10 >= 10
```

```
true
```

---

## Less Than or Equal

```javascript
5 <= 2
```

```
false
```

---

# 5. == vs ===

One of the most common interview questions.

There are **two equality operators**.

```
==

===

```

They are NOT the same.

---

## == (Loose Equality)

`==` tries to convert values before comparing.

Example

```javascript
5 == "5"
```

JavaScript thinks

```
Number

↓

String

↓

Convert String to Number

↓

5 == 5

↓

true
```

Output

```javascript
true
```

Another example

```javascript
true == 1
```

Internally

```
true

↓

1

↓

1 == 1

↓

true
```

---

## === (Strict Equality)

Strict equality checks

1. Value
    
2. Type
    

Both must match.

```javascript
5 === "5"
```

Types

```
Number

vs

String
```

Different.

Output

```javascript
false
```

---

## Visual Comparison

```
==

Convert Types

↓

Compare


===

No Conversion

↓

Compare Directly
```

---

## Interview Rule

Always prefer

```javascript
===
```

It avoids unexpected conversions.

---

# 6. !==

Strict not equal.

```javascript
5 !== "5"
```

Output

```javascript
true
```

Because

```
Number

≠

String
```

---

# 7. if Statement

Imagine

```
If it rains

↓

Take umbrella
```

Syntax

```javascript
if(condition){

    // code
}
```

Example

```javascript
let age = 20;

if(age >= 18){

    console.log("Adult");
}
```

Execution

```
age >=18 ?

↓

true

↓

Run Code
```

Output

```
Adult
```

---

## False Condition

```javascript
let age = 15;

if(age >=18){

    console.log("Adult");
}
```

Execution

```
15>=18

↓

false

↓

Skip Block
```

Nothing prints.

---

# 8. else

Sometimes we want another path.

```
Rain?

↓

Yes

↓

Umbrella

No

↓

No Umbrella
```

Code

```javascript
if(age>=18){

    console.log("Adult");

}else{

    console.log("Minor");
}
```

Only ONE block runs.

---

# Dry Run

```javascript
let age=16;

if(age>=18){

    console.log("Adult");

}else{

    console.log("Minor");

}
```

Step 1

```
16>=18

↓

false
```

Step 2

```
Skip if
```

Step 3

```
Run else
```

Output

```
Minor
```

---

# 9. else if

Used when there are multiple possibilities.

Example

```javascript
let marks = 75;

if(marks>=90){

    console.log("A");

}else if(marks>=75){

    console.log("B");

}else if(marks>=50){

    console.log("C");

}else{

    console.log("Fail");

}
```

Execution

```
75>=90 ?

↓

No

↓

75>=75 ?

↓

Yes

↓

Print B

↓

Stop
```

Output

```
B
```

Only the **first matching condition** executes.

---

# 10. Nested if

Conditions can exist inside another condition.

```javascript
let loggedIn=true;

let admin=false;

if(loggedIn){

    if(admin){

        console.log("Admin Panel");

    }

}
```

Visual

```
Logged In?

↓

Yes

↓

Admin?

↓

Yes

↓

Panel
```

---

# 11. Logical Operators

Sometimes multiple conditions must be combined.

---

## AND (&&)

Meaning

```
Condition A

AND

Condition B
```

Both must be true.

Example

```javascript
let age=22;

let citizen=true;

if(age>=18 && citizen){

    console.log("Eligible");

}
```

Truth Table

|A|B|Result|
|---|---|---|
|T|T|T|
|T|F|F|
|F|T|F|
|F|F|F|

---

## OR (||)

Only ONE needs to be true.

```javascript
if(isAdmin || isModerator){

}
```

Truth Table

|A|B|Result|
|---|---|---|
|T|T|T|
|T|F|T|
|F|T|T|
|F|F|F|

---

## NOT (!)

Reverses a Boolean.

```javascript
true
```

becomes

```javascript
false
```

Example

```javascript
let loggedIn=false;

if(!loggedIn){

    console.log("Login First");

}
```

---

# 12. Truthy and Falsy

A very important JavaScript concept.

Some values automatically behave like `true`.

Others behave like `false`.

Falsy values are only:

```javascript
false
0
-0
0n
""
null
undefined
NaN
```

Everything else is **truthy**.

Examples

```javascript
if("Hello"){
    console.log("Runs");
}
```

Output

```
Runs
```

Because `"Hello"` is truthy.

---

Example

```javascript
if(""){

    console.log("Runs");

}
```

Nothing prints.

Empty string is falsy.

---

# 13. Returning Boolean Values

Instead of

```javascript
function isAdult(age){

    if(age>=18){

        return true;

    }else{

        return false;

    }

}
```

Simply write

```javascript
function isAdult(age){

    return age>=18;

}
```

Because

```
age>=18

↓

Already produces

↓

true

or

false
```

Cleaner and faster.

---

# 14. Early Return Pattern

Instead of

```javascript
function divide(a,b){

    if(b!=0){

        return a/b;

    }

}
```

Write

```javascript
function divide(a,b){

    if(b===0){

        return "Cannot Divide";

    }

    return a/b;

}
```

Why?

The invalid case exits immediately.

The rest of the function becomes easier to read.

This pattern is used heavily in professional code.

---

# Memory Example

```javascript
let age=20;
```

```
Global Memory

age

↓

20
```

Call

```javascript
isAdult(age);
```

New Execution Context

```
Parameter

↓

20

↓

20>=18

↓

true

↓

Return
```

Execution context is destroyed.

Global memory remains.

---

# Common Mistakes

### Assignment instead of Comparison

Wrong

```javascript
if(age=18)
```

This assigns `18` to `age`.

Correct

```javascript
if(age===18)
```

---

### Using ==

Avoid

```javascript
if(value=="5")
```

Prefer

```javascript
if(value==="5")
```

---

### Forgetting Braces

Wrong

```javascript
if(age>=18)

console.log("Adult");

console.log("Vote");
```

Only the first statement belongs to the `if`.

Always use braces.

---

# C++ Comparison

|JavaScript|C++|
|---|---|
|`if`|`if`|
|`else`|`else`|
|`else if`|`else if`|
|`&&`|`&&`|
|`||
|`!`|`!`|
|`true`|`true`|
|`false`|`false`|

The syntax is almost identical.

The biggest difference is JavaScript's **truthy and falsy** behavior.

---

# Quick Revision

```javascript
if(condition){

}
```

```javascript
if(){

}else{

}
```

```javascript
if(){

}else if(){

}else{

}
```

```javascript
&&
```

Both conditions must be true.

```javascript
||
```

At least one condition must be true.

```javascript
!
```

Negates a Boolean.

Prefer

```javascript
===
```

instead of

```javascript
==
```

Use **early returns** to reduce nesting and improve readability.

---

# Interview Questions

1. What is the difference between `==` and `===`?
    
2. What are truthy and falsy values?
    
3. Why is `===` preferred over `==`?
    
4. What does the `!` operator do?
    
5. What is short-circuit evaluation in `&&` and `||`?
    
6. What is the early return pattern?
    
7. Why should you always use braces with `if` statements?
    
8. What happens if you write `if(age = 18)`?