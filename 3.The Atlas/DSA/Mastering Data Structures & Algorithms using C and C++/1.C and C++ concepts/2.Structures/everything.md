# Structures in C and C++ — Full Deep Notes

Think of a `struct` as:

> A custom data type made by grouping multiple related variables together into one memory block.

Real-world analogy:

A student has:

- name
- age
- marks

Instead of creating:

```cpp
string name;
int age;
float marks;
```

again and again for every student…

we create ONE blueprint:

```cpp
struct Student
{
    string name;
    int age;
    float marks;
};
```

Now:

```cpp
Student s1;
Student s2;
```

Each object gets its own grouped memory.

---

# WHY STRUCT EXISTS

Without struct:

```cpp
string name1;
int age1;

string name2;
int age2;
```

Messy.

With struct:

```cpp
Student s1;
Student s2;
```

Much cleaner.

---

# CORE MENTAL MODEL

A structure is:

```
ONE BIG MEMORY BOX
containing smaller variables inside it
```

Example:

```cpp
struct Student
{
    int age;
    char grade;
};
```

Memory layout:

```
Student Object:

+--------+--------+
|  age   | grade  |
+--------+--------+
```

---

# STRUCTURE IN C

---

# BASIC SYNTAX

```c
struct Student
{
    int age;
    float marks;
};
```

Here:

- `Student` = custom data type
- age and marks = members/data members

---

# CREATING VARIABLES

```c
struct Student s1;
```

Now memory is allocated for:

- age
- marks

inside `s1`.

---

# ACCESSING MEMBERS — DOT OPERATOR

```c
s1.age = 20;
s1.marks = 95.5;
```

Dot means:

> go inside the object

---

# FULL EXAMPLE (C)

```c
#include <stdio.h>

struct Student
{
    int age;
    float marks;
};

int main()
{
    struct Student s1;

    s1.age = 20;
    s1.marks = 95.5;

    printf("%d\\n", s1.age);
    printf("%.2f\\n", s1.marks);
}
```

---

# MEMORY UNDERSTANDING

Suppose:

```c
struct Student
{
    int age;
    float marks;
};
```

Memory:

```
s1:

Address 100 -> age
Address 104 -> marks
```

The struct object is ONE continuous memory block.

---

# WHAT DATA TYPES CAN STRUCT STORE?

Almost EVERYTHING.

---

# 1. Primitive Types

```c
struct Test
{
    int a;
    float b;
    char c;
};
```

---

# 2. Arrays

```c
struct Student
{
    char name[50];
};
```

---

# 3. Pointers

```c
struct Node
{
    int data;
    struct Node* next;
};
```

VERY IMPORTANT.

This creates:

- linked lists
- trees
- graphs

---

# 4. Another Structure

```c
struct Address
{
    int pin;
};

struct Student
{
    struct Address addr;
};
```

Nested structure.

---

# 5. Functions? (NO in C)

C structs cannot contain functions.

Only data.

---

# STRUCT MEMORY SIZE

Example:

```c
struct Test
{
    int a;
    char b;
};
```

You may think:

```
4 + 1 = 5 bytes
```

But actual size may become:

```
8 bytes
```

WHY?

Because of:

# MEMORY ALIGNMENT + PADDING

CPU accesses memory faster when aligned properly.

Compiler adds empty spaces.

---

# CHECKING SIZE

```c
printf("%lu", sizeof(struct Test));
```

---

# POINTER TO STRUCTURE

VERY IMPORTANT.

---

# WHY POINTERS?

Instead of copying entire object:

- we use address
- more efficient

---

# DECLARATION

```c
struct Student s1;

struct Student* ptr = &s1;
```

Here:

- ptr stores address of s1

---

# ACCESS USING POINTER

Two ways.

---

# METHOD 1

```c
(*ptr).age = 20;
```

Meaning:

1. go to object using `ptr`
2. access age using dot

---

# METHOD 2 — ARROW OPERATOR

Shortcut:

```c
ptr->age = 20;
```

This means:

```c
(*ptr).age
```

Arrow is just shorthand.

---

# FULL POINTER EXAMPLE

```c
#include <stdio.h>

struct Student
{
    int age;
};

int main()
{
    struct Student s1;

    struct Student* ptr = &s1;

    ptr->age = 25;

    printf("%d", ptr->age);
}
```

---

# STRUCTURE AND FUNCTIONS

---

# PASS BY VALUE

```c
void show(struct Student s)
{
    printf("%d", s.age);
}
```

Entire struct copied.

Can be expensive.

---

# PASS BY POINTER (BETTER)

```c
void show(struct Student* s)
{
    printf("%d", s->age);
}
```

Efficient.

Only address copied.

---

# STRUCTURES IN HEAP MEMORY

VERY IMPORTANT.

---

# STACK STRUCT

```c
struct Student s1;
```

Stored in stack memory.

Automatic cleanup.

---

# HEAP STRUCT

```c
struct Student* s1 =
    malloc(sizeof(struct Student));
```

Now:

- object lives in heap
- pointer lives in stack

---

# MEMORY VISUALIZATION

```
STACK:
s1 -> address 500

HEAP:
Address 500:
+------+
| age  |
+------+
```

---

# FULL HEAP EXAMPLE IN C

```c
#include <stdio.h>
#include <stdlib.h>

struct Student
{
    int age;
};

int main()
{
    struct Student* s1 =
        malloc(sizeof(struct Student));

    s1->age = 21;

    printf("%d", s1->age);

    free(s1);
}
```

---

# WHY USE HEAP?

Because:

- dynamic size
- survives function scope
- needed for linked lists/trees

---

# SELF-REFERENTIAL STRUCTURE

MOST IMPORTANT DSA CONCEPT.

```c
struct Node
{
    int data;
    struct Node* next;
};
```

This means:

- one node points to another node

Foundation of:

- linked lists
- trees
- graphs

---

# VISUALIZATION

```
Node1 -> Node2 -> Node3
```

Each node:

```
[data][next address]
```

---

# STRUCT VS CLASS

---

# IN C

Only structs exist.

---

# IN C++

Both exist.

Main difference:

|Struct|Class|
|---|---|
|members public by default|private by default|

---

# C++ STRUCT

```cpp
struct Student
{
    int age;

    void show()
    {
        cout << age;
    }
};
```

C++ structs CAN contain:

- functions
- constructors
- inheritance

---

# STRUCT OBJECT IN C++

```cpp
Student s1;
s1.age = 20;
s1.show();
```

---

# STRUCT WITH CONSTRUCTOR

```cpp
struct Student
{
    int age;

    Student(int a)
    {
        age = a;
    }
};
```

Usage:

```cpp
Student s1(20);
```

---

# HEAP MEMORY IN C++

Instead of malloc:

```cpp
Student* s1 = new Student();
```

Access:

```cpp
s1->age = 20;
```

Cleanup:

```cpp
delete s1;
```

---

# malloc VS new

|malloc|new|
|---|---|
|C|C++|
|no constructor|constructor called|
|returns void*|returns proper type|
|manual setup|object-oriented|

---

# IMPORTANT MEMORY CONCEPTS

---

# 1. Struct Variable = Entire Object

```cpp
Student s1;
```

Allocates full memory block.

---

# 2. Pointer Stores Address Only

```cpp
Student* ptr;
```

Pointer itself is small.

Usually:

```
8 bytes in 64-bit systems
```

---

# 3. Dot vs Arrow

Dot:

```cpp
s1.age
```

Direct object.

Arrow:

```cpp
ptr->age
```

Pointer to object.

---

# 4. Heap vs Stack

Stack:

```cpp
Student s1;
```

Fast.

Automatic cleanup.

Heap:

```cpp
Student* s1 = new Student();
```

Dynamic.

Manual cleanup.

---

# COMMON INTERVIEW QUESTIONS

---

# Q1. Can struct contain pointer to itself?

YES.

```c
struct Node
{
    struct Node* next;
};
```

---

# Q2. Why not direct object inside itself?

INVALID:

```c
struct Node
{
    struct Node next;
};
```

Infinite memory size problem.

Because:

- next contains next
- next contains next again endlessly

---

# Q3. Why use pointer then?

Pointer size is fixed.

So compiler knows memory size.

---

# MOST IMPORTANT CONNECTION

STRUCTURES ARE THE FOUNDATION OF:

- Linked Lists
- Trees
- Graphs
- Hash Tables
- Queues
- Stacks
- OS Kernels
- Network Packets
- File Systems

Everything starts from:

```
custom memory organization
```

That is what a struct really is.

---

# FINAL CORE UNDERSTANDING

A structure is:

```
A programmer-designed memory layout
```

You are literally designing how memory should look.

That’s the deep systems-level understanding behind structs.