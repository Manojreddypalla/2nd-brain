This is a great section because it covers **ES6 JavaScript**, which is what modern React and Node.js use. Since you want **Obsidian notes**, here's how I'd organize them. These are concise but interview-focused, so they're useful for quick revision.

---

# 📘 JavaScript ES6 & Control Flow

## 📂 1. Nested Objects

Objects can contain other objects.

```javascript
const user = {
    name: "Alex",
    address: {
        city: "Hyderabad",
        state: "Telangana"
    }
};

console.log(user.address.city);
```

Output:

```
Hyderabad
```

---

## 📂 2. Nested Arrays

```javascript
const arr = [
    [1,2],
    [3,4],
    [5,6]
];

console.log(arr[1][0]);
```

Output

```
3
```

---

# 🔁 Loops

## While Loop

Runs while condition is true.

```javascript
let i = 1;

while(i<=5){
    console.log(i);
    i++;
}
```

Use when number of iterations is unknown.

---

## For Loop

```javascript
for(let i=0;i<5;i++){
    console.log(i);
}
```

Best when number of iterations is known.

---

## Reverse Loop

```javascript
for(let i=10;i>=1;i--){
    console.log(i);
}
```

---

## Odd Numbers

```javascript
for(let i=1;i<=10;i+=2){
    console.log(i);
}
```

---

## Iterate Array

```javascript
const arr=[10,20,30];

for(let i=0;i<arr.length;i++){
    console.log(arr[i]);
}
```

---

## Nested Loops

```javascript
for(let i=0;i<3;i++){
    for(let j=0;j<3;j++){
        console.log(i,j);
    }
}
```

Used in:

- Matrices
    
- Pattern printing
    
- 2D Arrays
    

---

## do...while

Executes at least once.

```javascript
let i=1;

do{
    console.log(i);
    i++;
}while(i<=5);
```

Difference:

```
while
↓

Checks first

↓

Runs
```

```
do-while

↓

Runs first

↓

Checks
```

---

# 🎲 Random Numbers

Random decimal

```javascript
Math.random()
```

Range

```
0 ≤ value < 1
```

Random Integer

```javascript
Math.floor(Math.random()*10)
```

Produces

```
0-9
```

Random Between

```javascript
Math.floor(Math.random()*(max-min+1))+min
```

---

# 🔢 parseInt()

Converts String → Number

```javascript
parseInt("42")
```

↓

```
42
```

With radix

```javascript
parseInt("101",2)
```

↓

```
5
```

---

# ❓ Ternary Operator

Instead of

```javascript
if(age>=18)
    return "Adult";
else
    return "Minor";
```

Use

```javascript
age>=18 ? "Adult":"Minor";
```

Syntax

```
condition
?

true value

:

false value
```

---

## Multiple Ternary

```javascript
score>=90
?"A"
:score>=70
?"B"
:"C"
```

Avoid deep nesting.

---

# 📦 var vs let vs const

|Feature|var|let|const|
|---|---|---|---|
|Redeclare|✅|❌|❌|
|Reassign|✅|✅|❌|
|Scope|Function|Block|Block|

Use

```
const

↓

Default
```

```
let

↓

When value changes
```

Avoid

```
var
```

---

# const Arrays

```javascript
const arr=[1,2];

arr.push(3);
```

Works.

Because

```
Reference

↓

Constant

Not

Contents
```

Cannot

```javascript
arr=[4,5];
```

---

# Object.freeze()

Prevents mutation.

```javascript
const obj={
    name:"Alex"
};

Object.freeze(obj);
```

Now

```javascript
obj.name="Bob";
```

Will fail.

---

# 🚀 Arrow Functions

Old

```javascript
function add(a,b){
    return a+b;
}
```

Arrow

```javascript
const add=(a,b)=>{
    return a+b;
}
```

Short Form

```javascript
const add=(a,b)=>a+b;
```

Benefits

- Cleaner syntax
    
- Lexical `this`
    
- Used everywhere in React
    

---

# Default Parameters

Old

```javascript
function greet(name){
    if(name===undefined)
        name="Guest";
}
```

Modern

```javascript
function greet(name="Guest"){
    return name;
}
```

---

# Rest Operator (...)

Collects remaining arguments.

```javascript
function sum(...nums){
    console.log(nums);
}
```

Call

```javascript
sum(1,2,3,4);
```

Output

```
[1,2,3,4]
```

---

# Spread Operator (...)

Copies or expands.

```javascript
const arr=[1,2];

const copy=[...arr];
```

Merge

```javascript
const a=[1,2];

const b=[3,4];

const c=[...a,...b];
```

Output

```
[1,2,3,4]
```

---

# Destructuring

Old

```javascript
const person={
    name:"Alex",
    age:22
};

const name=person.name;
```

Modern

```javascript
const {name,age}=person;
```

Arrays

```javascript
const arr=[10,20];

const [a,b]=arr;
```

Skip values

```javascript
const [,,third]=arr;
```

---

# Template Literals

Old

```javascript
"Hello "+name
```

Modern

```javascript
`Hello ${name}`
```

Multi-line

```javascript
`Line1

Line2`
```

---

# Object Property Shorthand

Old

```javascript
const name="Alex";

const user={
    name:name
}
```

Modern

```javascript
const user={
    name
}
```

---

# Method Shorthand

Old

```javascript
const person={
    greet:function(){

    }
}
```

Modern

```javascript
const person={
    greet(){

    }
}
```

---

# Classes

Old constructor function

```javascript
function Person(name){
    this.name=name;
}
```

Modern

```javascript
class Person{

    constructor(name){
        this.name=name;
    }
}
```

Object

```javascript
const p=new Person("Alex");
```

---

# Getter

```javascript
class Rectangle{

    constructor(width){
        this.width=width;
    }

    get area(){
        return this.width*this.width;
    }
}
```

Access

```javascript
rect.area
```

No parentheses.

---

# Setter

```javascript
set width(value){
    this._width=value;
}
```

Used for validation.

---

# Modules

Export

```javascript
export const PI=3.14;
```

Default Export

```javascript
export default add;
```

Import

```javascript
import {PI} from "./math.js";
```

Default

```javascript
import add from "./math.js";
```

---

# ⭐ Frequently Asked Interview Questions

### Q1 Difference between while and do...while?

|while|do...while|
|---|---|
|Checks first|Executes first|
|May execute 0 times|Executes at least once|

---

### Q2 Difference between Rest and Spread?

**Rest (`...`)**

- Collects multiple values into one array (function parameters).
    

```javascript
function sum(...nums) {}
```

**Spread (`...`)**

- Expands an array or object into individual elements.
    

```javascript
const copy = [...arr];
```

---

### Q3 Difference between `let` and `const`?

- `let`: can be reassigned.
    
- `const`: cannot be reassigned.
    

---

### Q4 Can a `const` array be modified?

✅ Yes.

```javascript
const arr=[1];

arr.push(2);
```

Only the reference is constant.

---

### Q5 Why use Arrow Functions?

- Shorter syntax.
    
- Lexical `this`.
    
- Preferred in React.
    

---

### Q6 Difference between Template Literals and Strings?

Template literals use backticks (`` ` ``), support:

- String interpolation (`${}`)
    
- Multi-line strings
    

---

### Q7 Why use Destructuring?

To extract object or array values cleanly.

```javascript
const {name} = user;
```

instead of

```javascript
const name = user.name;
```

---

### Q8 Difference between `parseInt()` and `Number()`?

- `parseInt("10px")` → `10`
    
- `Number("10px")` → `NaN`
    

---

### ⭐ Most Important Topics for Interviews

1. `let` vs `const` vs `var`
    
2. Arrow Functions
    
3. Rest vs Spread
    
4. Destructuring
    
5. Template Literals
    
6. Classes
    
7. Getters & Setters (basic idea)
    
8. Import & Export (ES Modules)
    
9. `for`, `while`, `do...while`
    
10. Ternary Operator
    

These are the ES6+ features you'll use constantly in React, Node.js, and modern JavaScript codebases. They also appear frequently in internship coding tests and technical interviews.