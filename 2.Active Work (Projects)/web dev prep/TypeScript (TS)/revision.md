Excellent. Now we're entering **TypeScript**, which is much smaller than JavaScript.

The good news is that for **internship interviews**, companies usually don't expect deep TypeScript expertise. They want to know that you understand **why it exists** and can read/write basic TypeScript.

---

# TypeScript (TS)

**Full Form:** TypeScript

**Created By:** Microsoft

**Purpose:** JavaScript + Static Type Checking

Think of it like this:

```text
JavaScript
      +
Type Safety
      +
Better Tooling
-----------------
TypeScript
```

TypeScript is **not a new language**. It's a **superset of JavaScript**, which means every valid JavaScript program is also valid TypeScript.

---

# Why TypeScript?

In JavaScript, this is allowed:

```javascript
let age = 20;
age = "twenty";
```

No error until runtime.

In TypeScript:

```typescript
let age: number = 20;

age = "twenty";
```

❌ Compile-time error.

TypeScript catches mistakes **before your code runs**.

---

# How TypeScript Works

The browser **cannot understand TypeScript directly**.

Flow:

```text
TypeScript (.ts)
        ↓
TypeScript Compiler (tsc)
        ↓
JavaScript (.js)
        ↓
Browser
```

---

# Basic Types ⭐⭐⭐⭐⭐

## Number

```typescript
let age: number = 22;
```

---

## String

```typescript
let name: string = "John";
```

---

## Boolean

```typescript
let isLoggedIn: boolean = true;
```

---

## Array

```typescript
let nums: number[] = [1,2,3];
```

or

```typescript
let names: Array<string> = ["A","B"];
```

---

## Object

```typescript
let user: {
    name: string;
    age: number;
};
```

---

## Any ⭐⭐⭐⭐☆

```typescript
let value: any;
```

Can store anything.

Avoid using it unless necessary because it disables type checking.

---

## Unknown ⭐⭐⭐☆

```typescript
let value: unknown;
```

Safer than `any`.

You must check the type before using it.

---

## Void

Functions that return nothing.

```typescript
function print(): void {

}
```

---

## Never

Used for functions that never successfully return.

Example:

```typescript
function error(): never {
    throw new Error("Oops");
}
```

---

# Type Inference ⭐⭐⭐⭐⭐

TypeScript often figures out the type automatically.

```typescript
let age = 20;
```

It knows `age` is a `number`.

You don't always need to write:

```typescript
let age: number = 20;
```

---

# Functions ⭐⭐⭐⭐⭐

```typescript
function add(a: number, b: number): number {
    return a + b;
}
```

Explanation:

```text
a → number

b → number

returns → number
```

---

# Optional Parameters

```typescript
function greet(name?: string) {

}
```

The `?` means the parameter is optional.

---

# Default Parameters

```typescript
function greet(name = "Guest") {

}
```

---

# Interfaces ⭐⭐⭐⭐⭐

One of the most important TypeScript concepts.

Used to define the shape of an object.

```typescript
interface User {
    name: string;
    age: number;
}
```

Usage:

```typescript
const user: User = {
    name: "John",
    age: 22
};
```

---

# Type Alias ⭐⭐⭐⭐☆

Another way to define custom types.

```typescript
type User = {
    name: string;
    age: number;
};
```

---

# Interface vs Type

|Interface|Type|
|---|---|
|Mostly for object shapes|Can represent objects, unions, primitives, tuples|
|Can be extended easily|More flexible overall|

For React projects, you'll commonly see both.

---

# Union Types ⭐⭐⭐⭐☆

A variable can have multiple possible types.

```typescript
let id: string | number;
```

Valid:

```typescript
id = 10;

id = "abc";
```

---

# Literal Types

Restrict values.

```typescript
let status: "success" | "error";
```

Only these two values are allowed.

---

# Enums ⭐⭐⭐☆

Group related constants.

```typescript
enum Role {
    Admin,
    User,
    Guest
}
```

---

# Generics ⭐⭐⭐⭐☆

Allow functions or classes to work with different types while preserving type safety.

Without generics:

```typescript
function identity(value: any) {
    return value;
}
```

With generics:

```typescript
function identity<T>(value: T): T {
    return value;
}
```

`T` is a placeholder for a type.

---

# Classes

```typescript
class Person {
    name: string;

    constructor(name: string) {
        this.name = name;
    }
}
```

---

# Access Modifiers

```typescript
public

private

protected
```

### public

Accessible everywhere.

### private

Accessible only inside the class.

### protected

Accessible inside the class and subclasses.

---

# Readonly

```typescript
readonly id: number;
```

Can only be assigned once.

---

# Modules

Export

```typescript
export function add() {}
```

Import

```typescript
import { add } from "./math";
```

---

# Type Assertions

Tell TypeScript to treat a value as a specific type.

```typescript
const input = document.getElementById("name") as HTMLInputElement;
```

Useful when interacting with the DOM.

---

# Null & Undefined

```typescript
let value: string | null;
```

TypeScript encourages you to explicitly handle missing values.

---

# Benefits of TypeScript

- Catches errors early
    
- Better autocomplete in editors
    
- Easier refactoring
    
- Improves readability
    
- Great for large projects
    
- Widely used with React
    

---

# Important Differences

## JavaScript vs TypeScript

|JavaScript|TypeScript|
|---|---|
|Dynamically typed|Statically typed|
|Errors mostly at runtime|Many errors at compile time|
|No type annotations|Supports type annotations|
|Runs directly in browser|Must be compiled to JavaScript|

---

## any vs unknown

|any|unknown|
|---|---|
|Can do anything|Must check type first|
|Less safe|More safe|

---

## interface vs type

|interface|type|
|---|---|
|Best for object shapes|More flexible|
|Extendable|Supports unions, intersections, tuples|

---

# Full Forms

TS → TypeScript

JS → JavaScript

TSC → TypeScript Compiler

IDE → Integrated Development Environment

---

# Interview Questions

- What is TypeScript?
    
- Why use TypeScript instead of JavaScript?
    
- What is a superset?
    
- What are interfaces?
    
- Difference between `interface` and `type`?
    
- What are union types?
    
- What is `any`?
    
- Difference between `any` and `unknown`?
    
- What are generics?
    
- Why does TypeScript need compilation?
    

---

# 📌 Priority Order

1. Why TypeScript?
    
2. Basic Types (`string`, `number`, `boolean`, `array`)
    
3. Type Inference
    
4. Functions with typed parameters and return types
    
5. Interfaces
    
6. Type Aliases
    
7. Union Types
    
8. `any` vs `unknown`
    
9. Generics (basic understanding)
    
10. Modules (`import` / `export`)
    
11. Type Assertions
    
12. Classes & Access Modifiers (basic)
    

---

### For your internship

If the company says **"Good understanding of HTML, CSS, JavaScript, and TypeScript"**, they're usually looking for someone who can:

- Read and understand TypeScript code in a React project.
    
- Define interfaces for props and API responses.
    
- Catch simple type errors.
    
- Write basic typed functions.
    

You **do not** need advanced TypeScript features like decorators, namespaces, or complex conditional types for an entry-level full-stack internship.

**Next up is React.js**, which is arguably the most important technology in this entire job description. We'll cover it from the ground up to interview level.