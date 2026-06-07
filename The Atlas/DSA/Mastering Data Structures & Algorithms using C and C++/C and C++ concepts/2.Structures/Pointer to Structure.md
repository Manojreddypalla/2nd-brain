# Pointer to Structure Notes (C and C++)

---

# 1. What is a Pointer to Structure?

A pointer to structure stores the **address of a structure variable/object**.

```
structRectangle {
int length;
int breadth;
};
```

---

# 2. Static Structure Creation

## C / C++

```
structRectangler = {10,5};
```

### Memory:

Stored in **Stack**

---

# 3. Declaring Pointer to Structure

## C / C++

```
structRectangle *p;
p = &r;
```

- `p` stores address of `r`

---

# 4. Accessing Structure Members

---

## Using Normal Variable

```
r.length
r.breadth
```

Use **dot operator (`.`)**

---

## Using Pointer

### Method 1:

```
(*p).length
```

### Method 2 (Preferred):

```
p->length
```

---

# 5. Why Arrow Operator?

Because:

```
p->length
```

is shorthand for:

```
(*p).length
```

---

# 6. Why Parentheses Needed?

Wrong:

```
*p.length
```

Compiler reads as:

```
*(p.length)
```

Because **`.` has higher precedence than `*`**

Correct:

```
(*p).length
```

---

# 7. Dynamic Structure Allocation in C

---

## Syntax

```
structRectangle*p;

p= (structRectangle*)malloc(sizeof(structRectangle));
```

---

## Access Members

```
p->length=10;
p->breadth=5;
```

---

## Free Memory

```
free(p);
```

---

# 8. Dynamic Structure Allocation in C++

---

## Syntax

```
Rectangle *p =newRectangle;
```

---

## Access Members

```
p->length =10;
p->breadth =5;
```

---

## Delete Memory

```
deletep;
```

---

# 9. Full C Program Example

```
#include <stdio.h>
#include <stdlib.h>

structRectangle {
intlength;
intbreadth;
};

intmain() {
structRectangle*p;

p= (structRectangle*)malloc(sizeof(structRectangle));

p->length=10;
p->breadth=5;

printf("%d %d",p->length,p->breadth);

free(p);

return0;
}
```

---

# 10. Full C++ Program Example

```
#include<iostream>
usingnamespacestd;

structRectangle {
int length;
int breadth;
};

intmain() {
Rectangle *p =newRectangle;

p->length =10;
p->breadth =5;

cout<<p->length<<" "<<p->breadth;

deletep;

return0;
}
```

---

# 11. Memory Diagram

```
Stack:                     Heap:

p -----------+            [ length | breadth ]
             |              10        5
             +-----------> Heap Object
```

---

# 12. C vs C++ Difference Table

|Feature|C|C++|
|---|---|---|
|Dynamic Allocation|`malloc()`|`new`|
|Deallocation|`free()`|`delete`|
|Typecasting Needed|Yes|No|
|Constructor Called|No|Yes|

---

# 13. Interview/Exam Important Points

---

### Pointer stores:

> Address of structure, NOT the structure itself

---

### Arrow Operator:

> Used to access structure members through pointer

---

### Heap Allocation:

> Dynamic memory remains until manually freed/deleted

---

# 14. Internal Working of Arrow Operator

Compiler converts:

```
p->length
```

Into:

```
(*p).length
```

Then:

1. Dereference pointer
2. Access member from resulting structure

---

# 15. Common Mistakes

---

## Forgetting Parentheses

```
*p.length// Wrong
```

---

## Forgetting Memory Deallocation

```
newRectangle;// Memory leak if not deleted
```

---

# 16. Real-Life Analogy

Think of:

- Structure = House
- Pointer = House Address

Arrow operator means:

> "Go to this address and access that room."

Example:

```
p->length
```

Means:

> Go to address in `p`, then access `length`.