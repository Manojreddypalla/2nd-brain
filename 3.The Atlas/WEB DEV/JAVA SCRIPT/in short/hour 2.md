This section (1:00–2:00) moves from **basic syntax** to **decision making and objects**. These topics are **extremely common** in interviews because React, Node.js, Express, and almost all JavaScript applications rely on them.

---

# ⭐⭐⭐ Priority 1 (Must Know)

## 1. Functions

### Return Statement

```javascript
function add(a, b) {
    return a + b;
}

console.log(add(5, 3));
```

Output

```
8
```

### Questions

**Q. What is `return`?**

> `return` sends a value back to the function caller and immediately stops function execution.

---

## Undefined Return

```javascript
function hello() {
    console.log("Hello");
}

console.log(hello());
```

Output

```
Hello
undefined
```

Because no value is returned.

---

## Return Early Pattern

```javascript
function checkAge(age) {
    if (age < 18)
        return "Minor";

    return "Adult";
}
```

Used frequently to avoid unnecessary nesting.

---

# ⭐⭐⭐⭐⭐ 2. Boolean

Boolean has only two values.

```javascript
true
false
```

Examples

```javascript
5 > 2
```

Returns

```
true
```

---

# ⭐⭐⭐⭐⭐ 3. If Statement

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

---

# ⭐⭐⭐⭐⭐ 4. Comparison Operators

|Operator|Meaning|
|---|---|
|==|Loose equality|
|===|Strict equality|
|!=|Loose not equal|
|!==|Strict not equal|
|>|Greater|
|<|Less|
|>=|Greater or equal|
|<=|Less or equal|

---

## == vs ===

Very common interview question.

```javascript
5 == "5"
```

↓

```
true
```

Because type conversion happens.

---

```javascript
5 === "5"
```

↓

```
false
```

Different types.

Always prefer

```javascript
===
```

---

# ⭐⭐⭐⭐ 5. Logical Operators

## AND

```javascript
&&
```

Both conditions must be true.

```javascript
age > 18 && marks > 60
```

---

## OR

```javascript
||
```

Only one condition needs to be true.

---

## NOT

```javascript
!
```

Flips boolean.

---

# ⭐⭐⭐⭐⭐ 6. Else

```javascript
if(age >=18){

}
else{

}
```

---

# ⭐⭐⭐⭐⭐ 7. Else If

```javascript
if(score>90){

}
else if(score>70){

}
else{

}
```

Checks conditions one by one.

---

# ⭐⭐⭐⭐ 8. If Else Chain

```javascript
if(){

}
else if(){

}
else if(){

}
else{

}
```

Only one block executes.

---

# ⭐⭐⭐⭐ 9. Switch

Alternative to many else-if blocks.

```javascript
switch(day){

case 1:

break;

case 2:

break;

default:

}
```

Need

```
break
```

Otherwise fall-through occurs.

---

Interview

When should switch be used?

Answer:

When comparing one variable against many fixed values.

---

# ⭐⭐⭐⭐ 10. Objects

Most important topic after arrays.

Object stores

```
key → value
```

Example

```javascript
const person={
    name:"Alex",
    age:22,
    city:"Hyderabad"
}
```

Memory

```
person

↓

{

name:"Alex"

age:22

city:"Hyderabad"

}
```

---

# ⭐⭐⭐⭐⭐ 11. Dot Notation

```javascript
person.name
```

Output

```
Alex
```

---

# ⭐⭐⭐⭐⭐ 12. Bracket Notation

```javascript
person["name"]
```

Same output.

---

Interview

Difference?

Dot

```javascript
person.name
```

Bracket

```javascript
person["name"]
```

Bracket is useful when key comes from a variable.

```javascript
let key="name";

person[key]
```

---

# ⭐⭐⭐⭐ 13. Update Property

```javascript
person.age=25;
```

---

# ⭐⭐⭐⭐ 14. Add Property

```javascript
person.country="India";
```

---

# ⭐⭐⭐⭐ 15. Delete Property

```javascript
delete person.age;
```

---

# ⭐⭐⭐⭐ 16. Object Lookup

Instead of

```javascript
switch(card){

}
```

Use

```javascript
const lookup={
A:"Apple",
B:"Ball"
};

lookup["A"];
```

Very common interview concept.

---

# ⭐⭐⭐⭐ 17. Checking Property

```javascript
person.hasOwnProperty("age")
```

Returns

```
true
```

or

```
false
```

---

# ⭐⭐⭐⭐ 18. Complex Objects

Objects inside arrays

```javascript
const users=[
{
name:"Alex",
age:22
},
{
name:"Bob",
age:25
}
];
```

Access

```javascript
users[0].name
```

↓

```
Alex
```

---

# ⭐⭐⭐ Queue Example (Stand in Line)

This teaches Queue.

```javascript
arr.push(item);
```

Add to back.

```javascript
arr.shift();
```

Remove from front.

FIFO

```
First In

↓

First Out
```

---

# ⭐⭐⭐⭐ Counting Cards

Introduces

- switch
    
- condition
    
- return
    

Not important for interview.

Understand logic.

---

# Frequently Asked Interview Questions

### Q1 Difference between == and ===

**Answer**

```
==

Checks value only

Type conversion happens
```

```
===

Checks value and type

No type conversion
```

---

### Q2 What does return do?

Returns a value and exits the function immediately.

---

### Q3 What happens if a function has no return?

Returns

```
undefined
```

---

### Q4 Difference between && and ||

```
&&

Both conditions true
```

```
||

At least one condition true
```

---

### Q5 What is a Boolean?

Data type having only

```
true

false
```

---

### Q6 Difference between if and switch?

**if**

- Best for ranges or complex conditions (`score > 90`).
    

**switch**

- Best for checking one variable against many exact values.
    

---

### Q7 What is an object?

A collection of key-value pairs.

---

### Q8 Difference between array and object?

|Array|Object|
|---|---|
|Indexed|Key-value|
|Ordered|Named properties|
|`arr[0]`|`obj.name`|

---

### Q9 Dot notation vs bracket notation?

```javascript
obj.name
```

vs

```javascript
obj["name"]
```

Use brackets when the property name is stored in a variable or contains spaces/special characters.

---

### Q10 How do you add a property?

```javascript
obj.city="Delhi";
```

---

### Q11 How do you delete a property?

```javascript
delete obj.city;
```

---

### Q12 How do you check whether a property exists?

```javascript
obj.hasOwnProperty("name")
```

---

### Q13 Output?

```javascript
console.log(5=="5");
```

**Answer**

```
true
```

---

### Q14 Output?

```javascript
console.log(5==="5");
```

**Answer**

```
false
```

---

### Q15 Output?

```javascript
let person={
name:"Alex"
};

console.log(person.name);
```

**Answer**

```
Alex
```

---

### Q16 Output?

```javascript
let person={
age:22
};

delete person.age;

console.log(person.age);
```

**Answer**

```
undefined
```

---

## 🎯 Highest-Priority Topics from This Hour

If you're short on time, master these first:

1. ✅ `return` statement
    
2. ✅ `if`, `else`, `else if`
    
3. ✅ `==` vs `===`
    
4. ✅ `&&`, `||`, `!`
    
5. ✅ `switch` and `break`
    
6. ✅ Objects
    
7. ✅ Dot vs bracket notation
    
8. ✅ Add, update, delete object properties
    
9. ✅ `hasOwnProperty()`
    
10. ✅ Arrays of objects (`users[0].name`)
    

These concepts appear very frequently in JavaScript MCQs and technical interviews, especially for full-stack internship roles.