# JavaScript Master Notes

# Chapter 3 — Objects (Part 2)

---

# Topics Covered

- switch Statement
    
- Objects
    
- Object Memory Model
    
- Dot Notation
    
- Bracket Notation
    
- Variables as Property Names
    
- Updating Properties
    
- Adding Properties
    
- Deleting Properties
    
- Object Lookups
    
- hasOwnProperty()
    

---

# 1. switch Statement

## Why do we need switch?

Imagine a menu.

```text
1 → Home
2 → Profile
3 → Settings
4 → Logout
```

Using many `else if` statements works, but becomes messy.

```javascript
if(choice===1){

}
else if(choice===2){

}
else if(choice===3){

}
else{

}
```

`switch` makes code cleaner when comparing **one variable against many fixed values**.

---

## Syntax

```javascript
switch(value){

    case 1:
        console.log("Home");
        break;

    case 2:
        console.log("Profile");
        break;

    default:
        console.log("Unknown");
}
```

---

## Flow

```text
          value

             │
      ┌──────┼──────┐
      │      │      │
   case1  case2  case3
      │      │      │
      └──────┼──────┘
             │
         default
```

---

## Example

```javascript
let day = 3;

switch(day){

case 1:
    console.log("Monday");
    break;

case 2:
    console.log("Tuesday");
    break;

case 3:
    console.log("Wednesday");
    break;

default:
    console.log("Invalid");

}
```

Output

```text
Wednesday
```

---

# Why break?

Without it

```javascript
switch(day){

case 1:
    console.log("Monday");

case 2:
    console.log("Tuesday");

case 3:
    console.log("Wednesday");

}
```

If

```javascript
day = 1
```

Output

```text
Monday
Tuesday
Wednesday
```

Execution "falls through."

Always use `break` unless you intentionally want fall-through.

---

# Multiple Cases

```javascript
switch(letter){

case 'a':
case 'e':
case 'i':
case 'o':
case 'u':

    console.log("Vowel");
    break;

default:

    console.log("Consonant");

}
```

---

# switch vs if

Use **if**

- Ranges
    
- Complex conditions
    

```javascript
if(score>90)
```

Use **switch**

- One variable
    
- Fixed values
    

```javascript
switch(color)
```

---

# 2. Objects

## The Most Important JavaScript Data Structure

Think about a student.

A student has

- name
    
- age
    
- branch
    
- CGPA
    

Using variables

```javascript
let name="Alex";
let age=21;
let branch="CSE";
```

These values are related.

Objects group related data.

```javascript
let student={

name:"Alex",

age:21,

branch:"CSE",

cgpa:8.9

};
```

---

# Mental Model

Object

```text
student

↓

+----------------------+
| name   → "Alex"      |
| age    → 21          |
| branch → "CSE"       |
| cgpa   → 8.9         |
+----------------------+
```

An object is a **collection of key-value pairs**.

---

# Keys and Values

```javascript
let car={

brand:"BMW",

year:2025

};
```

Keys

```text
brand

year
```

Values

```text
BMW

2025
```

---

# Objects vs Arrays

Array

```javascript
["Alex",21,"CSE"]
```

Need indexes.

```javascript
student[1]
```

What is index 1?

Not obvious.

Objects

```javascript
student.age
```

Much more readable.

---

# Memory Model

Primitive

```javascript
let x=10;
```

Memory

```text
x

↓

10
```

Object

```javascript
let student={

name:"Alex"

};
```

Memory

```text
student

↓

Address 1000

↓

Object

↓

name

↓

Alex
```

Variables store a **reference** to the object, not the object itself.

This is one of the biggest differences from primitive values.

---

# 3. Dot Notation

Access properties.

```javascript
console.log(student.name);
```

Output

```text
Alex
```

Visual

```text
student

↓

name

↓

Alex
```

---

# More Examples

```javascript
student.age

student.cgpa
```

---

# 4. Bracket Notation

Alternative syntax.

```javascript
student["name"]
```

Output

```text
Alex
```

Equivalent to

```javascript
student.name
```

---

# Why Bracket Notation Exists

Sometimes the property name isn't known until runtime.

Example

```javascript
let key="age";

student[key]
```

Internally

```text
key

↓

"age"

↓

student["age"]

↓

21
```

Dot notation cannot do this.

---

# Property Names with Spaces

```javascript
let person={

"first name":"Alex"

};
```

Wrong

```javascript
person.first name
```

Correct

```javascript
person["first name"]
```

---

# Dot vs Bracket

Use dot when property names are known.

```javascript
student.age
```

Use brackets

- variables
    
- spaces
    
- special characters
    

```javascript
student[key]

student["first name"]
```

---

# 5. Updating Properties

Objects are mutable.

```javascript
student.age=22;
```

Before

```text
age

↓

21
```

After

```text
age

↓

22
```

---

# 6. Adding New Properties

Objects grow dynamically.

```javascript
student.city="Hyderabad";
```

Memory

Before

```text
name

age
```

After

```text
name

age

city
```

---

# Another Example

```javascript
student["email"]="abc@gmail.com";
```

Works exactly the same.

---

# 7. Delete Properties

```javascript
delete student.cgpa;
```

Before

```text
name

age

cgpa
```

After

```text
name

age
```

---

# 8. Variables as Keys

Suppose

```javascript
let prop="branch";
```

Then

```javascript
student[prop]
```

Internally

```text
prop

↓

"branch"

↓

student["branch"]

↓

"CSE"
```

Very common in APIs.

---

# 9. Object Lookup

Instead of

```javascript
if(card=="A")

return 11;

if(card=="K")

return 10;

if(card=="Q")

return 10;
```

Use an object.

```javascript
const values={

A:11,

K:10,

Q:10,

J:10

};
```

Lookup

```javascript
values["A"]
```

Output

```text
11
```

This technique is faster and cleaner.

---

# 10. hasOwnProperty()

Sometimes a property may not exist.

Example

```javascript
student.phone
```

Returns

```text
undefined
```

Better

```javascript
student.hasOwnProperty("phone")
```

Output

```text
false
```

---

Example

```javascript
student.hasOwnProperty("age")
```

Output

```text
true
```

---

# Why not compare with undefined?

Bad

```javascript
if(student.phone===undefined)
```

Better

```javascript
if(student.hasOwnProperty("phone"))
```

This clearly expresses your intent: you're checking whether the property exists.

---

# Dry Run

```javascript
let student={

name:"Alex",

age:20

};

student.age=21;

student.city="Delhi";

delete student.name;
```

Step 1

```text
{
name:"Alex",
age:20
}
```

Step 2

```text
{
name:"Alex",
age:21
}
```

Step 3

```text
{
name:"Alex",
age:21,
city:"Delhi"
}
```

Step 4

```text
{
age:21,
city:"Delhi"
}
```

---

# Common Mistakes

### Using dot notation with variables

Wrong

```javascript
let key="age";

student.key
```

Looks for a property literally named `"key"`.

Correct

```javascript
student[key]
```

---

### Forgetting Quotes

Wrong

```javascript
student[name]
```

JavaScript treats `name` as a variable.

Correct

```javascript
student["name"]
```

or

```javascript
student.name
```

---

### Assuming Objects Are Copied

```javascript
let a={

x:1

};

let b=a;
```

Memory

```text
a ─────┐
       │
       ▼
     {x:1}
       ▲
       │
b ─────┘
```

Changing

```javascript
b.x=10;
```

Also changes

```javascript
a.x
```

because both variables reference the **same object**.

---

# C++ Comparison

|JavaScript|C++|
|---|---|
|Object|`struct` / `class`|
|Property|Member variable|
|Dot notation|`obj.member`|
|Reference semantics|Pointer/reference behavior|
|Dynamic properties|Not available in normal C++ classes|

---

# Quick Revision

```javascript
let obj={}
```

Access

```javascript
obj.name
```

or

```javascript
obj["name"]
```

Update

```javascript
obj.age=22
```

Add

```javascript
obj.city="Hyderabad"
```

Delete

```javascript
delete obj.city
```

Exists?

```javascript
obj.hasOwnProperty("age")
```

---

# Interview Questions

1. What is an object in JavaScript?
    
2. Why are objects called reference types?
    
3. What is the difference between dot notation and bracket notation?
    
4. When should you use bracket notation?
    
5. What does `delete` do?
    
6. What is `hasOwnProperty()`?
    
7. Why are objects preferred over long `if-else` chains for lookups?
    
8. What happens when you assign one object variable to another?