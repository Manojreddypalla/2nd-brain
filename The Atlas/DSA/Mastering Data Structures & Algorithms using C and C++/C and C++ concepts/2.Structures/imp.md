# Structures + Pointers + Heap vs Stack — Most Important Notes

This is the REAL important part for DSA, systems, linked lists, trees, and OOP.

Forget theory overload.

The core thing is:

```
Object = actual memory block
Pointer = variable storing address of memory block
```

Everything comes from this.

---

# 1. STRUCTURE BASICS

```cpp
struct Student
{
    int age;
    float marks;
};
```

This is just:

> a custom memory layout

---

# OBJECT CREATION

```cpp
Student s1;
```

Now memory is created.

---

# MEMORY VISUALIZATION

```
s1

+--------+--------+
| age    | marks  |
+--------+--------+
```

Example:

```cpp
s1.age = 20;
s1.marks = 95.5;
```

---

# ACCESSING MEMBERS — DOT OPERATOR

```cpp
s1.age
```

Meaning:

> go inside object and access age

Dot is used when:

```
you have DIRECT object
```

---

# 2. POINTER TO OBJECT

MOST IMPORTANT CONCEPT.

---

# NORMAL OBJECT

```cpp
Student s1;
```

Memory:

```
STACK:

s1:
+--------+--------+
| age    | marks  |
+--------+--------+
```

---

# POINTER OBJECT

```cpp
Student* ptr = &s1;
```

Now:

```
ptr stores ADDRESS of s1
```

---

# MEMORY VISUALIZATION

```
STACK:

s1:
Address 100
+--------+--------+
| age    | marks  |
+--------+--------+

ptr:
stores 100
```

---

# POINTER ACCESS

Two ways.

---

# METHOD 1

```cpp
(*ptr).age
```

Meaning:

1. go to address using `ptr`
2. access age using dot

---

# METHOD 2 — ARROW OPERATOR

Shortcut:

```cpp
ptr->age
```

This is SAME as:

```cpp
(*ptr).age
```

---

# IMPORTANT RULE

|Situation|Operator|
|---|---|
|direct object|`.`|
|pointer to object|`->`|

---

# COMPLETE EXAMPLE

```cpp
#include <iostream>
using namespace std;

struct Student
{
    int age;
};

int main()
{
    Student s1;

    s1.age = 20;

    Student* ptr = &s1;

    cout << ptr->age;
}
```

---

# THINKING VISUALLY

```
ptr
 ↓
[address of s1]

s1
+------+
| age  |
+------+
```

Arrow means:

```
follow pointer
then access member
```

---

# 3. STACK VS HEAP

SUPER IMPORTANT.

---

# STACK MEMORY

```cpp
Student s1;
```

Created inside stack.

Automatic cleanup.

Fast.

---

# MEMORY

```
STACK:

s1:
+------+
| age  |
+------+
```

Destroyed automatically after function ends.

---

# HEAP MEMORY

Dynamic memory.

Created using:

```cpp
Student* s1 = new Student();
```

Now object is NOT in stack.

Object is in HEAP.

---

# MEMORY VISUALIZATION

```
STACK:
s1 -> 500

HEAP:
Address 500:
+------+
| age  |
+------+
```

Pointer lives in stack.

Actual object lives in heap.

---

# ACCESSING HEAP OBJECT

```cpp
s1->age = 20;
```

Because:

```
s1 is pointer
```

NOT direct object.

---

# DELETE MEMORY

```cpp
delete s1;
```

Important.

Otherwise:

```
memory leak
```

---

# COMPLETE HEAP EXAMPLE

```cpp
#include <iostream>
using namespace std;

struct Student
{
    int age;
};

int main()
{
    Student* s1 = new Student();

    s1->age = 25;

    cout << s1->age;

    delete s1;
}
```

---

# STACK VS HEAP DIFFERENCE

|Stack|Heap|
|---|---|
|automatic memory|manual memory|
|fast|slower|
|limited size|large memory|
|auto destroyed|must delete|
|temporary|dynamic lifetime|

---

# WHY HEAP IS IMPORTANT

Because stack objects disappear after function ends.

But heap memory survives until deleted.

Needed for:

- linked lists
- trees
- graphs
- dynamic structures

---

# 4. STRUCTURE INSIDE HEAP

```cpp
Student* s1 = new Student();
```

Internally:

STEP 1:

```
heap memory allocated
```

STEP 2:

```
address returned
```

STEP 3:

```
pointer stores address
```

---

# 5. SELF-REFERENTIAL STRUCTURE

MOST IMPORTANT FOR DSA.

```cpp
struct Node
{
    int data;
    Node* next;
};
```

This creates linked lists.

---

# MEMORY VISUALIZATION

```
Node1:
+------+--------+
| data | next   |
+------+--------+

next stores address of another node
```

---

# LINKING NODES

```cpp
Node* first = new Node();
Node* second = new Node();

first->next = second;
```

---

# VISUALIZATION

```
first -----> second
```

---

# WHY POINTER IS USED

THIS IS VERY IMPORTANT.

Wrong:

```cpp
struct Node
{
    Node next;
};
```

Impossible.

Because:

```
next contains next
next contains next
next contains next
...
```

Infinite size.

---

# POINTER FIXES IT

```cpp
Node* next;
```

Pointer has fixed size:

- usually 8 bytes

So compiler knows total struct size.

---

# 6. OBJECT VS POINTER OBJECT

---

# DIRECT OBJECT

```cpp
Student s1;
```

Memory created immediately.

Access:

```cpp
s1.age
```

---

# POINTER OBJECT

```cpp
Student* s1 = new Student();
```

Pointer stores address.

Access:

```cpp
s1->age
```

---

# CORE UNDERSTANDING

```
Object = actual house
Pointer = address to house
```

Dot:

```
go inside house directly
```

Arrow:

```
go to address
then enter house
```

---

# 7. PASSING STRUCTURES TO FUNCTIONS

---

# BAD (COPYING)

```cpp
void show(Student s)
```

Entire object copied.

---

# BETTER

```cpp
void show(Student* s)
{
    cout << s->age;
}
```

Only address copied.

Efficient.

---

# CALL

```cpp
show(&s1);
```

---

# 8. MOST IMPORTANT INTERVIEW UNDERSTANDING

---

# STACK OBJECT

```cpp
Student s1;
```

Destroyed automatically.

---

# HEAP OBJECT

```cpp
Student* s1 = new Student();
```

Lives until:

```cpp
delete s1;
```

---

# FINAL CORE IDEA

Everything in advanced DSA comes from this:

```
Custom memory blocks
+
Pointers connecting memory blocks
```

That is:

- linked list
- tree
- graph
- hash table
- OS structures

all fundamentally.