# Lecture 3 — Memory Management, RAII & Smart Pointers

> **Goal:** Learn how C++ manages memory safely using RAII and Smart Pointers while minimizing manual memory management.

---

# Memory Allocation

There are two primary places where objects can be stored.

## Stack Memory

- Automatically managed
- Very fast allocation/deallocation
- Stores local variables
- Memory is released automatically when the variable goes out of scope
- Limited size

Example:

```cpp
int main()
{
    int x = 10;
}
```

When `main()` finishes, `x` is automatically destroyed.

---

## Heap Memory

- Dynamically allocated
- Much larger than stack
- Exists until explicitly released
- Managed using `new` and `delete` (or preferably Smart Pointers)

Example

```cpp
int* ptr = new int(5);

delete ptr;
```

If `delete` is forgotten, memory leaks occur.

---

# The Problem with Heap Memory

Manual memory management causes problems:

- Memory leaks
- Double deletion
- Dangling pointers
- Forgetting cleanup during exceptions
- Difficult ownership tracking

Example

```cpp
void foo()
{
    int* data = new int[100];

    if(someError)
        return;

    delete[] data;
}
```

If the function returns early, memory is never released.

This is exactly the problem RAII solves.

---

# RAII

## Resource Acquisition Is Initialization

RAII is one of the most important concepts in C++.

### Core Idea

Bind the lifetime of a resource to the lifetime of an object.

Instead of remembering:

- allocate
- use
- delete

You create an object.

When the object dies, its destructor automatically cleans everything.

---

## RAII Workflow

```

Constructor
↓
Acquire Resource

↓

Use Resource

↓

Object goes out of scope

↓

Destructor

↓

Release Resource

```

No manual cleanup required.

---

# RAII Principles

## 1. Encapsulate every resource inside a class

Instead of exposing raw pointers, wrap them inside a class.

```
Resource
↓

Class

↓

Object owns resource

```

---

## 2. Constructor acquires the resource

The constructor performs allocation.

Example

```cpp
array = new int[size];
```

---

## 3. Destructor releases the resource

Destructor performs cleanup.

```cpp
delete[] array;
```

No matter how the function exits, cleanup always happens.

---

## 4. Resource lifetime == Object lifetime

When object exists

→ Resource exists

When object dies

→ Resource is released

---

# RAII Example

```cpp
class IntArray
{
    int* array;

public:

    IntArray(size_t size)
    {
        array = new int[size];
    }

    ~IntArray()
    {
        delete[] array;
    }

    int& operator[](size_t i)
    {
        return array[i];
    }
};
```

---

## Constructor

```cpp
IntArray(size_t size)
{
    array = new int[size];
}
```

Allocates heap memory.

---

## Destructor

```cpp
~IntArray()
{
    delete[] array;
}
```

Automatically frees memory.

---

## Operator[]

```cpp
int& operator[](size_t i)
{
    return array[i];
}
```

Allows usage like

```cpp
arr[5] = 20;
```

instead of

```cpp
arr.operator[](5);
```

---

# RAII Usage

```cpp
int main()
{
    IntArray arr(10);

    arr[5] = 21;

}
```

Execution:

```
Create arr
        ↓
Constructor runs
        ↓
Memory allocated
        ↓
Use array
        ↓
main ends
        ↓
Destructor runs
        ↓
Memory released
```

No manual delete required.

---

# Why RAII Matters

Without RAII

```
new

↓

Work

↓

Remember delete
```

Easy to forget.

With RAII

```
Create Object

↓

Use Object

↓

Leave Scope

↓

Automatic cleanup
```

Safer and simpler.

---

# Smart Pointers

Writing a custom RAII class for every resource is tedious.

Instead, C++ Standard Library provides Smart Pointers.

Smart pointers automatically implement RAII.

---

## Benefits

- Automatic cleanup
- Prevent memory leaks
- Exception safe
- Ownership tracking
- No manual delete

---

# std::shared_ptr

A shared_ptr is a smart pointer that allows multiple owners of the same object.

Also called

> Reference Counted Pointer

---

## Internal Reference Counter

Every shared_ptr owns a hidden counter.

Example

```cpp
auto p = std::make_shared<MyClass>();
```

```
Object

Counter = 1
```

---

## Copying

```cpp
auto p2 = p;
```

```
Counter = 2
```

Both pointers own the object.

---

## Destruction

If one pointer disappears

```
Counter--

```

Object still exists.

---

When counter reaches zero

```
Counter = 0

↓

Delete object
```

Automatic cleanup.

---

# shared_ptr Example

```cpp
void func(std::shared_ptr<MyClass> p)
{
    p->doSomething();
}

int main()
{
    auto p = std::make_shared<MyClass>();

    func(p);

}
```

Execution

```
make_shared()

Counter = 1

↓

Function receives copy

Counter = 2

↓

Function exits

Counter = 1

↓

main ends

Counter = 0

↓

Delete object
```

---

# make_shared()

Preferred way to create shared pointers.

Instead of

```cpp
std::shared_ptr<MyClass> p(new MyClass());
```

Use

```cpp
auto p = std::make_shared<MyClass>();
```

Advantages

- Faster
- Safer
- Single allocation
- Cleaner syntax

---

# Memory Allocation Guidelines

## 1. Prefer Stack

Whenever possible

```cpp
Player player;
```

Advantages

- Faster
- Automatic cleanup
- Cache friendly

---

## 2. Use Smart Pointers for Heap

```cpp
auto enemy = std::make_shared<Enemy>();
```

Avoid raw new/delete.

---

## 3. Use Raw Pointers Only When Necessary

Usually for

- Non-owning references
- Legacy APIs
- Performance-critical systems
- Low-level engine code

Never use raw pointers for ownership.

---

# Templates

Templates allow writing generic code.

Instead of writing

```cpp
VectorInt
VectorFloat
VectorDouble
```

write one generic class.

Example

```cpp
template<typename T>
class Vector
{

};
```

---

## STL Example

```cpp
std::vector<int>

std::vector<float>

std::vector<Player>
```

Same implementation.

Different data types.

Compiler generates specialized versions automatically.

---

# Pass by const Reference

Large objects should not be copied.

Instead of

```cpp
void Draw(Texture texture);
```

Use

```cpp
void Draw(const Texture& texture);
```

---

## Why?

Copying

```
Entire object copied

↓

More memory

↓

Slower
```

Reference

```
Only 8-byte address passed

↓

No copy

↓

Much faster
```

---

## const

```cpp
const Texture&
```

Means

- Function cannot modify object
- No copying
- Better performance
- Safer API

---

# Summary

## RAII

- Constructor acquires resource
- Destructor releases resource
- Resource lifetime = Object lifetime

---

## Smart Pointers

- Implement RAII automatically
- Prefer `std::shared_ptr`
- Use `std::make_shared()`

---

## Heap vs Stack

Prefer:

```
Stack
        ↓
Smart Pointer
        ↓
Raw Pointer (last resort)
```

---

## Templates

- Generic programming
- One implementation
- Multiple data types

---

## Pass by const Reference

Use for large objects.

Benefits

- No copying
- Faster
- Read-only
- Standard C++ practice

---

# Key Takeaways

- Prefer automatic memory management.
- Use RAII to tie resource lifetime to object lifetime.
- Prefer stack allocation whenever possible.
- Use `std::shared_ptr` for shared ownership of heap objects.
- Use `std::make_shared()` instead of `new`.
- Pass large objects by `const&`.
- Use templates to write reusable, type-independent code.