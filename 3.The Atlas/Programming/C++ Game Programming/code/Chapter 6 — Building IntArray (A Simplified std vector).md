# Chapter 6 — Building `IntArray` (A Simplified `std::vector`)

> **Goal:** Build a class that safely manages a dynamic array using RAII and understand every line of code.

---

# Why Create `IntArray`?

Until now we've been writing code like this:

```cpp
int* arr = new int[10];

// Use the array

delete[] arr;
```

This works.

But imagine writing this in **100 different places**.

Problems:

- Easy to forget `delete[]`
    
- Memory leaks
    
- Hard to maintain
    
- Error-prone
    

Instead, we want one class that handles all of this.

```cpp
IntArray arr(10);
```

That's it.

The user of the class shouldn't care about `new` or `delete`.

---

# Designing the Class

Let's think before writing code.

What information must an array remember?

### 1. Where is the data stored?

```cpp
int* m_Data;
```

This pointer stores the address of the heap memory.

Example

```text
Stack

IntArray
│
▼
m_Data
│
▼
7000

Heap

7000
│
▼
+----+----+----+----+
| ?  | ?  | ?  | ?  |
+----+----+----+----+
```

Without this pointer...

there's no way to find the array.

---

### 2. How many elements exist?

```cpp
size_t m_Size;
```

Suppose

```cpp
IntArray arr(100);
```

How does the object know

```text
There are 100 integers.
```

It must store that information.

Otherwise

```cpp
arr.print();
```

wouldn't know where to stop.

---

# Why `size_t` Instead of `int`?

You'll see this everywhere in C++.

```cpp
size_t
```

is an unsigned integer type used for

```text
Sizes

Memory

Indexes
```

Why not `int`?

Because sizes cannot be negative.

```text
Array Size

-5

❌ Makes no sense.
```

---

### Internally

On most systems

```text
64-bit computer

↓

size_t

↓

8 bytes
```

On 32-bit systems

```text
size_t

↓

4 bytes
```

The compiler chooses the correct type automatically.

That's why STL containers use

```cpp
size_t
```

instead of

```cpp
int
```

---

# Member Naming (`m_`)

You'll notice

```cpp
m_Data
m_Size
```

The prefix

```text
m_
```

means

```text
Member Variable
```

This helps distinguish

```cpp
class IntArray
{
    size_t m_Size;

public:

    IntArray(size_t size)
    {
        m_Size = size;
    }
};
```

Without `m_`

```cpp
size = size;
```

Which one is the parameter?

Which one is the member?

Confusing.

With

```cpp
m_Size = size;
```

It's obvious.

---

# The Class Skeleton

```cpp
class IntArray
{
private:

    int* m_Data;
    size_t m_Size;

public:

};
```

Notice

Everything is

```cpp
private
```

Why?

Because users shouldn't directly modify

```cpp
m_Data
```

Imagine someone writes

```cpp
arr.m_Data = nullptr;
```

Now

```text
Original Heap Memory

↓

Lost Forever

↓

Memory Leak
```

Private protects the object's internal state.

---

# Constructor

Now let's allocate memory.

```cpp
IntArray(size_t size)
{
    m_Size = size;
    m_Data = new int[size];
}
```

Let's execute

```cpp
IntArray arr(5);
```

---

## Step 1

Object created on stack.

```text
Stack

arr
```

---

## Step 2

Constructor begins.

Parameter

```cpp
size
```

contains

```text
5
```

---

## Step 3

```cpp
m_Size = size;
```

Object now stores

```text
m_Size

↓

5
```

---

## Step 4

```cpp
m_Data = new int[size];
```

Heap

```text
7000

↓

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

The returned address

```text
7000
```

is stored inside

```text
m_Data
```

---

Final Memory

```text
Stack

arr

m_Size = 5

m_Data
│
▼
7000

Heap

7000

↓

+----+----+----+----+----+
| ?  | ?  | ?  | ?  | ?  |
+----+----+----+----+----+
```

The object now **owns** that heap memory.

---

# Better Constructor (Initializer List)

Instead of

```cpp
IntArray(size_t size)
{
    m_Size = size;
    m_Data = new int[size];
}
```

Modern C++ prefers

```cpp
IntArray(size_t size)
    : m_Size(size),
      m_Data(new int[size])
{
}
```

Why?

Because member variables are initialized **directly**, rather than being default-constructed and then assigned.

This is more efficient and is required for some types (like `const` members and references).

---

# Destructor

Now we clean up.

```cpp
~IntArray()
{
    delete[] m_Data;
}
```

When

```cpp
arr
```

dies

Memory becomes

Before

```text
Stack

arr

↓

Heap Array
```

After

```text
Stack

Destroyed

Heap

Memory Returned
```

No memory leak.

---

# Complete RAII Flow

```cpp
IntArray arr(5);
```

Internally

```text
Constructor

↓

new int[5]

↓

Object Uses Memory

↓

Destructor

↓

delete[]
```

The programmer never writes

```cpp
delete[]
```

outside the class.

---

# Adding a `size()` Function

Suppose we want to know

how many elements exist.

```cpp
size_t size() const
{
    return m_Size;
}
```

Why not

```cpp
arr.m_Size
```

Because

```cpp
m_Size
```

is private.

Instead

```cpp
arr.size()
```

returns it safely.

---

# Adding a Print Function

```cpp
void print() const
{
    for(size_t i = 0; i < m_Size; i++)
    {
        std::cout << m_Data[i] << " ";
    }

    std::cout << '\n';
}
```

Dry Run

Suppose

```text
Heap

10

20

30
```

Loop

```text
i = 0

↓

10

i = 1

↓

20

i = 2

↓

30
```

Output

```text
10 20 30
```

---

# Why `const`?

Notice

```cpp
size_t size() const

void print() const
```

The final

```cpp
const
```

means

```text
This function promises

NOT

to modify the object.
```

Compiler enforces this promise.

If you accidentally write

```cpp
m_Size = 50;
```

inside a `const` member function,

the compiler reports an error.

---

# Dry Run of the Whole Class

```cpp
int main()
{
    IntArray arr(3);

    arr.print();
}
```

### Step 1

Create object.

```text
Stack

arr
```

---

### Step 2

Constructor runs.

```text
Heap

3 integers
```

---

### Step 3

`print()`

Reads

```text
m_Data

↓

Print each element
```

---

### Step 4

Program exits.

Destructor runs automatically.

```text
delete[]

↓

Heap Freed
```

---

# Encapsulation

Our class hides all implementation details.

The user only sees

```cpp
IntArray arr(10);

arr.print();

arr.size();
```

The user doesn't know

- where memory is stored
    
- how it's allocated
    
- when it's deleted
    

This is called **Encapsulation**.

The class exposes a simple interface while hiding the complex implementation.

---

# Common Mistakes

## ❌ Forgetting the Destructor

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

## ❌ Making Members Public

```cpp
public:

int* m_Data;
```

Now anyone can write

```cpp
arr.m_Data = nullptr;
```

The object loses ownership.

Bad design.

---

## ❌ Using `int` for Sizes

```cpp
int m_Size;
```

Works most of the time,

but the C++ standard library consistently uses

```cpp
size_t
```

for sizes and indexes because it's the correct type for memory-related quantities.

---

# Mental Model

Think of `IntArray` as a warehouse manager.

Without the manager:

```text
You rent a warehouse.

↓

You must remember

where it is

when to clean it

when to return it
```

With the manager:

```text
Hire Manager

↓

Manager rents warehouse

↓

Manager keeps records

↓

Manager returns warehouse

↓

You simply use it
```

That's exactly what the `IntArray` object is doing.

---

# Key Takeaways

- `IntArray` owns a dynamically allocated array.
    
- `m_Data` stores the address of the heap memory.
    
- `m_Size` stores the number of elements.
    
- The constructor allocates memory.
    
- The destructor releases memory.
    
- `private` protects the object's internal state.
    
- Member initializer lists are the preferred way to initialize members.
    
- `const` member functions promise not to modify the object.
    
- This design is a simplified version of how `std::vector` manages memory.
    

---

# Up Next

Now we'll make the class feel like a **real C++ container** by adding:

- `operator[]`
    
- returning references (`T&`)
    
- const overloads
    
- templates (`template<typename T>`)
    
- turning `IntArray` into a generic `Array<T>`
    

This is where the class starts to resemble the real implementation style of the C++ Standard Library.