# Chapter 5 — RAII (Resource Acquisition Is Initialization)

> **Goal:** Learn how C++ manages resources automatically using constructors and destructors, eliminating memory leaks.

---

# Before RAII

Suppose we allocate memory manually.

```cpp
int* p = new int;

*p = 10;
```

Memory

```
Stack

p
│
▼
5000

Heap

5000
│
▼
10
```

Everything looks fine.

But now imagine the program ends.

```cpp
delete p;
```

If we forget this line...

```
Heap Memory

↓

Still Allocated

↓

Memory Leak
```

The computer doesn't magically know you're finished with that memory.

---

# The Real Problem

Imagine writing this.

```cpp
void process()
{
    int* data = new int[1000];

    // lots of code...
}
```

Looks harmless.

But after the function returns

```
Stack

data

↓

Destroyed
```

while

```
Heap

1000 Integers

↓

Still Exist
```

Nobody owns them anymore.

That memory is permanently lost until the program exits.

---

# Why Humans Are Bad at Manual Memory

Imagine this.

```cpp
void foo()
{
    int* arr = new int[100];

    if(error)
        return;

    delete[] arr;
}
```

What happens if

```cpp
error == true
```

The function returns before

```cpp
delete[]
```

Memory leak.

Programmers forget cleanup all the time.

C++ needed a better solution.

---

# The Big Idea

Instead of saying

> "Remember to delete memory later."

C++ says

> "Let's make the object responsible for cleaning itself."

That's RAII.

---

# What is RAII?

RAII stands for

```
Resource

Acquisition

Is

Initialization
```

Let's break that down.

## Resource

Anything that must be acquired and later released.

Examples

- Heap memory
    
- Files
    
- Network sockets
    
- Database connections
    
- Mutexes
    
- Threads
    
- GPU memory
    

---

## Acquisition

Getting the resource.

Example

```cpp
new int[10]
```

or

```cpp
std::fstream file("notes.txt");
```

---

## Initialization

Acquire the resource while creating the object.

Instead of

```cpp
IntArray arr;

arr.allocate();
```

We do

```cpp
IntArray arr(10);
```

Construction

↓

Resource acquired immediately.

---

# The RAII Rule

Every resource follows this lifecycle.

```
Constructor

↓

Acquire Resource

↓

Use Resource

↓

Destructor

↓

Release Resource
```

This is the heart of modern C++.

---

# Constructors

A constructor runs automatically when an object is created.

Example

```cpp
class Test
{
public:

    Test()
    {
        std::cout << "Created";
    }
};
```

Usage

```cpp
Test t;
```

Output

```
Created
```

The constructor runs automatically.

---

# Destructors

A destructor runs automatically when an object dies.

Syntax

```cpp
~ClassName()
{
}
```

Example

```cpp
class Test
{
public:

    Test()
    {
        std::cout << "Created\n";
    }

    ~Test()
    {
        std::cout << "Destroyed\n";
    }
};
```

Program

```cpp
int main()
{
    Test t;
}
```

Output

```
Created

Destroyed
```

Notice

You never called

```cpp
t.~Test();
```

The compiler did it automatically.

---

# Building Our Own RAII Class

Let's build something useful.

```cpp
class IntArray
{
private:

    int* m_Data;
    size_t m_Size;
};
```

Why these members?

```
Pointer

↓

Where data lives

Size

↓

How many elements exist
```

Without the size

we wouldn't know how many elements belong to the array.

---

# Constructor

```cpp
IntArray(size_t size)
{
    m_Size = size;
    m_Data = new int[size];
}
```

Suppose

```cpp
IntArray arr(5);
```

Step 1

Create object

```
Stack

arr
```

---

Step 2

Allocate heap memory.

```
Stack

arr

↓

Pointer

↓

Heap

+----+
| ? |
+----+
| ? |
+----+
| ? |
+----+
| ? |
+----+
| ? |
+----+
```

Now the object owns that memory.

---

# Destructor

When the object dies

```cpp
~IntArray()
{
    delete[] m_Data;
}
```

Memory

Before

```
Stack

arr

↓

Heap Array
```

After destruction

```
Stack

Destroyed

Heap

Memory Returned
```

No leak.

---

# Why This Is Beautiful

Look at user code.

```cpp
void foo()
{
    IntArray arr(1000);

}
```

That's it.

No

```cpp
delete[]
```

No cleanup.

When

```cpp
foo()
```

ends

```
Destructor

↓

Runs Automatically

↓

Memory Freed
```

---

# Lifetime

Every object has a lifetime.

```
Created

↓

Alive

↓

Destroyed
```

RAII ties

```
Resource Lifetime

=

Object Lifetime
```

This is why it works so well.

---

# Dry Run

```cpp
int main()
{
    IntArray arr(3);
}
```

Step 1

Compiler reserves space for

```
arr
```

---

Step 2

Constructor runs.

```
new int[3]
```

Memory

```
Stack

arr

↓

7000

Heap

7000

↓

+----+
| ? |
+----+
| ? |
+----+
| ? |
+----+
```

---

Step 3

Program reaches end of main.

Destructor runs automatically.

```
delete[]

↓

Heap Memory Freed
```

Program ends cleanly.

---

# What If an Exception Happens?

Without RAII

```cpp
int* p = new int[100];

throw std::runtime_error("Error");

delete[] p;
```

The exception skips

```cpp
delete[]
```

Memory leak.

---

With RAII

```cpp
IntArray arr(100);

throw std::runtime_error("Error");
```

Exception occurs.

Stack begins unwinding.

Every object is destroyed automatically.

```
Destructor

↓

delete[]

↓

Memory Released
```

This is one of the biggest reasons RAII exists.

It makes code **exception-safe**.

---

# Why STL Uses RAII

Suppose

```cpp
std::vector<int> numbers;
```

Internally

```
vector

↓

Pointer

↓

Heap Array
```

When the vector dies

```
Destructor

↓

delete[]

↓

Memory Freed
```

Exactly the same idea as our `IntArray`.

The standard library simply implements it much more robustly.

---

# Resources Beyond Memory

RAII isn't just for memory.

Example

```cpp
std::ifstream file("data.txt");
```

Constructor

```
Open File
```

Destructor

```
Close File
```

You never call

```cpp
file.close();
```

in most cases.

The destructor handles it.

---

Another example

```cpp
std::lock_guard<std::mutex> lock(m);
```

Constructor

```
Lock Mutex
```

Destructor

```
Unlock Mutex
```

Again

Automatic cleanup.

---

# Common Mistakes

## ❌ Forgetting a Destructor

```cpp
class IntArray
{
    int* m_Data;
};
```

Constructor allocates memory.

No destructor.

Memory leak.

---

## ❌ Manual Cleanup + RAII

Wrong

```cpp
delete[] arr.m_Data;
```

If the destructor later executes

```
delete[]

Again

↓

Double Free
```

Never manually free resources owned by an RAII object.

---

## ❌ Copying Raw Pointers (Preview)

```cpp
IntArray a(10);

IntArray b = a;
```

Now both objects point to the same heap memory.

```
a

↓

5000

b

↓

5000
```

When both destructors run

```
delete[]

↓

delete[]

↓

💥 Double Free
```

This is why we need the **Rule of Three/Five**, which we'll learn later.

---

# Mental Model

Imagine hiring a housekeeper.

Without RAII

```
Cook

↓

Eat

↓

Remember to Clean

↓

Sometimes Forget
```

With RAII

```
Hire Housekeeper

↓

They Clean Automatically

↓

Nothing Forgotten
```

The object takes responsibility for its own cleanup.

---

# Why Modern C++ Loves RAII

Almost every standard library class follows this pattern:

|Class|Resource Managed|
|---|---|
|`std::vector`|Dynamic memory|
|`std::string`|Dynamic memory|
|`std::fstream`|File handles|
|`std::unique_ptr`|Heap object|
|`std::shared_ptr`|Shared heap object|
|`std::thread`|Operating system thread|
|`std::lock_guard`|Mutex lock|

Once you understand RAII, you'll start seeing it everywhere.

---

# Key Takeaways

- **RAII = Resource Acquisition Is Initialization.**
    
- Resources are acquired in the **constructor** and released in the **destructor**.
    
- Cleanup becomes automatic, even if exceptions occur.
    
- The lifetime of the resource is tied to the lifetime of the object.
    
- RAII eliminates most manual `delete` calls and is one of the foundations of modern C++.
    

---

## Next Chapter

We'll **finish building our `IntArray`** by making it behave like a real container:

- encapsulation (`private` members)
    
- `size_t`
    
- `operator[]`
    
- bounds checking discussion
    
- `print()` function
    
- converting `IntArray` into a reusable class
    

This is where your class starts looking like a simplified version of `std::vector`.
