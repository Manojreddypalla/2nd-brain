Perfect. This chapter is where C++ starts becoming different from languages like Java, Python, or C#. You'll understand **why `std::vector` exists** and why **RAII became one of the core ideas of modern C++**.

---

# Chapter 4 — Dynamic Memory (Heap), `new`, `delete`, and Memory Management

> **Goal:** Understand why stack memory isn't always enough, how the heap works, and why manual memory management is dangerous.

---

# The Problem with Stack Memory

Until now, every variable we've created looked like this:

```cpp
int a = 10;
int arr[5];
```

These variables are stored on the **stack**.

```text
Stack

+------------------+
| arr[5]           |
+------------------+
| a = 10           |
+------------------+
```

The stack is:

- Very fast
    
- Automatically managed
    
- Perfect for local variables
    

So why do we need anything else?

---

# Imagine This Problem

Suppose you're writing a program that asks the user:

```text
How many students are there?
```

The user types

```text
1000
```

Now you need an array.

Can you write

```cpp
int arr[userInput];
```

No.

The compiler must know the array size **when compiling the program**, not when it's running (standard C++).

So the stack cannot help here.

We need memory that can be created **while the program is running**.

That's called **Dynamic Memory**.

---

# Static vs Dynamic Memory

## Static (Compile Time)

Known before the program starts.

```cpp
int arr[10];
```

Compiler knows

```text
Need

10 integers

↓

40 bytes
```

Easy.

---

## Dynamic (Run Time)

Unknown until execution.

```cpp
int size;

std::cin >> size;
```

Only now do we know

```text
Need

size integers
```

The stack cannot magically grow based on runtime input.

Instead we ask the operating system.

---

# The Heap

Besides the stack, every program also has a large memory region called the **heap**.

Think of memory like this:

```text
+------------------------+
|        Stack           |
|  Local Variables       |
+------------------------+

        Free Space

+------------------------+
|         Heap           |
| Dynamic Allocation     |
+------------------------+
```

Stack

- Automatic
    

Heap

- Manual
    

---

# What is the Heap?

The heap is simply a large pool of memory managed by the operating system (through the runtime allocator).

Unlike the stack,

nothing is created automatically.

You must request memory.

Think of it like renting storage.

```text
Need memory?

↓

Ask OS

↓

OS gives address

↓

Use memory

↓

Return it later
```

---

# Allocating Memory with `new`

Suppose we need one integer.

```cpp
int* p = new int;
```

Let's see what happens.

---

### Step 1

Program asks

```text
Need

4 bytes
```

---

### Step 2

The runtime finds free heap memory.

Suppose

```text
Address

5000
```

---

### Step 3

It returns

```text
5000
```

---

### Step 4

The pointer stores it.

Memory becomes

```text
Stack

p

↓

5000



Heap

5000

↓

?

```

Notice

The pointer lives on the stack.

The actual integer lives on the heap.

---

# Assigning a Value

```cpp
int* p = new int;

*p = 42;
```

Memory

```text
Stack

p

↓

5000



Heap

5000

↓

42
```

---

# Dynamic Arrays

Instead of one integer

we can request many.

```cpp
int* arr = new int[5];
```

Memory

```text
Stack

arr

↓

7000



Heap

7000

↓

+----+----+----+----+----+
| ?  | ?  | ?  | ?  | ?  |
+----+----+----+----+----+
```

Each integer is still stored contiguously.

Exactly like a normal array.

The only difference is

its memory comes from the heap.

---

# Using the Array

```cpp
arr[0] = 10;
arr[1] = 20;
arr[2] = 30;
```

Memory

```text
7000 → 10

7004 → 20

7008 → 30

7012 → ?

7016 → ?
```

Notice

Pointer arithmetic still works.

```cpp
arr[2]
```

means

```cpp
*(arr+2)
```

Exactly like a stack array.

---

# The Problem

What happens when the function ends?

```cpp
void foo()
{
    int* p = new int;

    *p = 42;
}
```

When the function exits

the pointer disappears.

```text
Stack

p

↓

Destroyed
```

But

```text
Heap

42

Still Exists
```

Nobody remembers where it is.

You have just lost access to that memory.

This is called

# Memory Leak

---

# Memory Leak

A memory leak happens when

```text
Memory

↓

Allocated

↓

Never Released
```

The memory still exists.

But your program has no pointer to it anymore.

It's impossible to free.

Imagine renting a storage locker,

throwing away the key,

and continuing to pay rent forever.

---

# Cleaning Up

To return memory,

use

```cpp
delete p;
```

Example

```cpp
int* p = new int;

*p = 42;

delete p;
```

Memory

Before

```text
Heap

5000

↓

42
```

After

```text
Heap

Memory becomes available again.
```

The pointer still exists,

but the object does not.

---

# Arrays Need `delete[]`

Suppose

```cpp
int* arr = new int[10];
```

How was memory allocated?

As an array.

Therefore

```cpp
delete[] arr;
```

must be used.

Not

```cpp
delete arr;
```

Because the runtime needs to know it's releasing an array allocation.

---

# Why `delete[]`?

Imagine

```cpp
new Student[100];
```

Each student has a destructor.

The runtime must call

```text
Destructor

↓

100 times
```

before freeing memory.

`delete[]` tells the runtime:

> "This was an array."

---

# Dangling Pointer

Suppose

```cpp
int* p = new int;

delete p;
```

Now

```text
p

↓

Still contains

5000
```

But

```text
5000

↓

No longer belongs to you.
```

This is called a **dangling pointer**.

Using it is undefined behavior.

---

# Safe Practice

Immediately after deleting

```cpp
delete p;

p = nullptr;
```

Now

```text
p

↓

nullptr
```

You cannot accidentally use freed memory.

---

# Double Delete

Wrong

```cpp
delete p;

delete p;
```

Memory

First delete

✅ Correct

Second delete

❌ Memory was already released.

Undefined behavior.

---

# Memory Ownership

Whenever you use

```cpp
new
```

ask yourself

```text
Who owns this memory?
```

Some object

must eventually call

```cpp
delete
```

Otherwise

the memory leaks.

This idea of **ownership** is the foundation of RAII and smart pointers.

---

# Dry Run

```cpp
int* arr = new int[3];

arr[0]=5;

arr[1]=8;

arr[2]=12;

delete[] arr;
```

Step 1

Allocate

```text
Heap

3 integers
```

---

Step 2

Store values

```text
5

8

12
```

---

Step 3

Call

```cpp
delete[]
```

Heap memory returned to the allocator.

Program no longer owns it.

---

# Common Mistakes

## ❌ Forgetting `delete`

```cpp
int* p = new int;
```

Never deleted.

Memory leak.

---

## ❌ Using After Delete

```cpp
delete p;

std::cout<<*p;
```

Undefined behavior.

---

## ❌ Wrong Delete

```cpp
int* arr=new int[5];

delete arr;
```

Wrong.

Use

```cpp
delete[] arr;
```

---

## ❌ Losing the Pointer

```cpp
int* p=new int;

p=nullptr;
```

Now

```text
Heap Memory

↓

Still allocated

↓

Impossible to free
```

Memory leak.

---

# Stack vs Heap

|Stack|Heap|
|---|---|
|Automatic|Manual|
|Very Fast|Slower|
|Small|Much Larger|
|Freed Automatically|Must Be Freed Manually|
|Local Variables|Dynamic Objects|
|Compiler Manages|Programmer (or RAII/Smart Pointers) Manages|

---

# Mental Model

Think of the **stack** like your study desk.

You place books there while studying.

When you're done, you automatically clean the desk.

The **heap** is like renting a storage unit.

You decide:

- when to rent it (`new`)
    
- what to put inside
    
- when to empty it (`delete`)
    

If you forget to empty it, you keep paying for unused storage—that's a **memory leak**.

---

# Key Takeaways

- The stack is automatic; the heap is manual.
    
- Use `new` to allocate memory on the heap.
    
- `delete` frees a single object.
    
- `delete[]` frees an array.
    
- Forgetting to delete causes memory leaks.
    
- Using memory after deleting it causes dangling pointers.
    
- Every `new` should have a matching `delete` (or, better yet, be managed by RAII).
    

---

## What's Next?

Now comes the chapter that changes everything.

Instead of remembering to call `delete` ourselves, we'll design a class that **owns** its memory and automatically cleans it up. This is **RAII (Resource Acquisition Is Initialization)**—one of the most important principles in modern C++ and the foundation of classes like `std::vector`, `std::string`, and `std::unique_ptr`.
