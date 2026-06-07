# CORE IDEA

Structures are user-defined grouped data.

---

# Example

``` cpp
structRectangle
{
    int length;
    int breadth;
};
```

---

# WHY STRUCTURES MATTER

Instead of passing many variables:

```
area(length,breadth);
```

We can pass:

```
area(rectangle);
```

Cleaner.

More scalable.

---

# 1. STRUCTURE PASS BY VALUE

---

# Example

```cpp
struct Rectangle
{
    int length;
    int breadth;
};

int area(Rectangle r)
{
    return r.length * r.breadth;
}
```

---

# IMPORTANT

Entire structure copied.

---

# MEMORY MODEL

Original:

```
r:
length = 10
breadth = 5
```

Function copy:

```
r1:
length = 10
breadth = 5
```

---

# MODIFICATIONS DO NOT AFFECT ORIGINAL

Because copy created.

---

# 2. STRUCTURE PASS BY ADDRESS

---

# Example

```
voidchangeLength(Rectangle *p)
{
p->length =20;
}
```

Call:

```
changeLength(&r);
```

---

# VERY IMPORTANT

Arrow operator:

```
p->length
```

means:

```
(*p).length
```

---

# WHY POINTERS USED HERE?

Because function directly modifies original structure.

---

# MEMORY MODEL

```
p stores address of r
```

So function accesses original structure indirectly.

---

# 3. STRUCTURE PASS BY REFERENCE

(C++)

---

# Example

```
voidchange(Rectangle &r)
{
r.length =50;
}
```

---

# WHY POWERFUL?

No copying.

Cleaner syntax.

Original modified.

---

# HUGE INTERVIEW CONCEPT

Large structures are expensive to copy.

So often use:

```
constRectangle &r
```

Why?

```
No copying
Read-only
Efficient
```

---

# STRUCTURE WITH ARRAY

VERY IMPORTANT CONCEPT.

---

# Example

```
structTest
{
int A[5];
int n;
};
```

---

# AMAZING THING

Even though arrays alone cannot be passed by value:

```
Structure containing array CAN be passed by value
```

---

# WHY?

Compiler copies entire structure member-by-member.

Including array elements.

---

# MEMORY VISUALIZATION

Original:

```
A = {1,2,3,4,5}
```

Copy:

```
A = {1,2,3,4,5}
```

Entire array duplicated.

---

# WHY IMPORTANT FOR DSA?

Structures become foundation of:

```
Nodes
Linked Lists
Trees
Graphs
Stacks
Queues
```

---

# MASTER CONNECTION OF ALL 3 TOPICS

```
Functions
   ↓
Parameter Passing
   ↓
Pointers
   ↓
Arrays
   ↓
Structures
   ↓
Dynamic Structures
   ↓
Linked Lists
   ↓
Trees / Graphs
```

---

# FINAL GOLDEN INTUITION

---

# PASS BY VALUE

```
"Take a photocopy"
```

---

# PASS BY ADDRESS

```
"Take my house address"
```

---

# PASS BY REFERENCE

```
"Use my nickname"
```

---

# ARRAYS

```
Array name = base address
```

---

# STRUCTURES

```
Group related data into one object
```

---

# BIGGEST TAKEAWAY

Almost all advanced DSA internally depends on:

```
Functions manipulating memory through addresses.
```

That is literally:

- linked lists
- trees
- graphs
- recursion
- heap structures
- dynamic programming tables
- STL internals

These topics are not “small basics”.

They are the foundation layer of systems thinking in C/C++.