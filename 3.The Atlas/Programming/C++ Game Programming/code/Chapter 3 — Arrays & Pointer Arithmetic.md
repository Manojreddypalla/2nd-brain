Excellent. This is one of the most important chapters in C++. Once this clicks, arrays, vectors, iterators, and even linked lists become much easier.

---

# Chapter 3 — Arrays & Pointer Arithmetic

> **Goal:** Understand how arrays are stored in memory and why `arr[i]` is actually `*(arr + i)`.

---

# The Big Idea

An array is **not** a collection of separate variables.

It is **one block of contiguous memory**.

For example:

```cpp
int arr[5] = {10, 20, 30, 40, 50};
```

The compiler does **not** create five unrelated integers.

Instead, it reserves one continuous block of memory.

```
Stack Memory

Address        Value

100 ----------> 10
104 ----------> 20
108 ----------> 30
112 ----------> 40
116 ----------> 50
```

Notice something.

Every integer is **4 bytes apart** because

```cpp
sizeof(int) == 4
```

---

# Why Contiguous Memory?

Suppose arrays were stored randomly.

```
10 → Address 100

20 → Address 500

30 → Address 900

40 → Address 200

50 → Address 700
```

Now imagine accessing the third element.

The CPU would first need to know

- where the first is
    
- then where the second is
    
- then where the third is
    

Very slow.

Instead, C++ stores arrays like this.

```
100
104
108
112
116
```

Now the compiler instantly knows

```
Address of element n

=

Base Address

+

n × sizeof(type)
```

This makes arrays **extremely fast**.

---

# What Does the Array Name Mean?

Consider

```cpp
int arr[5];
```

Many beginners think

```
arr

↓

Array
```

Not exactly.

The array **owns** the memory.

But the name `arr` behaves specially.

In most expressions,

```cpp
arr
```

becomes

```
Address of first element
```

Suppose

```
Address

100
```

Then

```cpp
arr
```

means

```
100
```

NOT

```
Entire array
```

This is called

# Array Decay

The array name automatically converts ("decays") into a pointer to its first element in most expressions.

---

# Visual Memory

```cpp
int arr[5] = {10,20,30,40,50};
```

```
           arr
            │
            ▼
      Address 100
            │
            ▼
+-----+-----+-----+-----+-----+
| 10  | 20  | 30  | 40  | 50  |
+-----+-----+-----+-----+-----+
100   104   108   112   116
```

Notice

`arr`

points to

```
First element
```

---

# Printing an Array Address

```cpp
std::cout << arr;
```

Possible output

```
0x61FF00
```

Exactly the same as

```cpp
std::cout << &arr[0];
```

Both print

```
Address of first element.
```

---

# Accessing Elements

Normally we write

```cpp
arr[2]
```

Output

```
30
```

Looks simple.

But internally something interesting happens.

---

# How `arr[i]` Works

Suppose

```
arr

↓

100
```

Now

```cpp
arr[2]
```

becomes

```
Address

100

+

2 × 4

=

108
```

Go there.

Read integer.

Result

```
30
```

That's literally how indexing works.

---

# Pointer Arithmetic

Let's create a pointer.

```cpp
int* p = arr;
```

Memory

```
p

↓

100
```

Now

```cpp
p + 1
```

does **NOT** become

```
101
```

Instead

```
104
```

Why?

Because

```
Pointer arithmetic

↓

Moves by object size
```

Since

```cpp
sizeof(int)=4
```

The compiler transforms

```
p + 1

↓

100 + 4

↓

104
```

---

# Example

```
Address

100

104

108

112

116
```

Then

```cpp
p
```

↓

```
100
```

```cpp
p+1
```

↓

```
104
```

```cpp
p+2
```

↓

```
108
```

```cpp
p+3
```

↓

```
112
```

---

# Dereferencing Again

Suppose

```cpp
int* p = arr;
```

Memory

```
100 → 10

104 → 20

108 → 30
```

Now

```cpp
*p
```

means

```
Go to address 100

↓

10
```

Output

```
10
```

---

Now

```cpp
*(p+1)
```

Compiler

```
p+1

↓

104

↓

Dereference

↓

20
```

Output

```
20
```

---

Similarly

```cpp
*(p+2)
```

becomes

```
30
```

---

# The Amazing Fact

Now compare

```cpp
arr[2]
```

and

```cpp
*(arr+2)
```

They produce

```
30
```

Exactly the same.

Because

The C++ language defines

```cpp
arr[i]
```

as

```cpp
*(arr+i)
```

These two are identical.

---

# Dry Run

```cpp
int arr[5]={10,20,30,40,50};

std::cout<<arr[3];
```

Step 1

```
arr

↓

100
```

Step 2

```
3 × sizeof(int)

↓

12
```

Step 3

```
100+12

↓

112
```

Step 4

Read

```
40
```

Output

```
40
```

---

# Modifying Elements

Suppose

```cpp
*(arr+2)=99;
```

Memory before

```
100 → 10

104 → 20

108 → 30

112 → 40
```

After

```
100 → 10

104 → 20

108 → 99

112 → 40
```

Now

```cpp
arr[2]
```

prints

```
99
```

---

# Traversing an Array

Method 1

```cpp
for(int i=0;i<5;i++)
{
    std::cout<<arr[i];
}
```

Method 2

```cpp
for(int i=0;i<5;i++)
{
    std::cout<<*(arr+i);
}
```

Both are exactly the same.

---

# `sizeof(arr)` vs `sizeof(pointer)`

This is a common interview question.

Suppose

```cpp
int arr[5];
```

Then

```cpp
sizeof(arr)
```

returns

```
20
```

Because

```
5 × 4
```

The compiler knows the complete array.

---

Now

```cpp
int* p=arr;
```

Then

```cpp
sizeof(p)
```

returns

```
8
```

(on a 64-bit machine)

Why?

Because

```
Pointer

↓

Stores only one address
```

It doesn't know how many elements exist.

---

# Array Initialization

Default

```cpp
int arr[5];
```

Memory

```
?

?

?

?

?
```

Garbage values (for local arrays).

---

Zero Initialization

```cpp
int arr[5]={};
```

Memory

```
0

0

0

0

0
```

---

Partial Initialization

```cpp
int arr[5]={1,2};
```

Memory

```
1

2

0

0

0
```

The remaining elements are automatically initialized to zero.

---

# Why Arrays Are Fast

Imagine searching through one million integers.

Because they are stored contiguously,

the CPU loads nearby values into its **cache** automatically.

```
CPU

↓

Cache

↓

RAM
```

When one element is read, the CPU often fetches several neighboring elements at the same time.

Since arrays store neighbors next to each other, traversing them is very fast. This is called **cache locality**, and it's one of the biggest reasons arrays outperform many pointer-based data structures for sequential access.

---

# Common Mistakes

### ❌ Thinking `arr` is a pointer

Not true.

`arr` is an array object.

It merely **decays into a pointer** in most expressions.

---

### ❌ Expecting `p+1` to increase by one byte

Wrong.

```
int*

↓

Moves 4 bytes
```

```
double*

↓

Moves 8 bytes
```

```
char*

↓

Moves 1 byte
```

Pointer arithmetic always advances by the size of the pointed-to type.

---

### ❌ Accessing Past the End

```cpp
arr[5]
```

is invalid for

```cpp
int arr[5];
```

Valid indices are

```
0

1

2

3

4
```

Reading or writing beyond those bounds results in **undefined behavior**.

---

# Mental Model

Think of an array as a train.

```
Engine

↓

Coach 1

↓

Coach 2

↓

Coach 3

↓

Coach 4
```

The engine is the first element.

The array name tells you where the train starts.

Pointer arithmetic simply walks from one coach to the next by the correct number of bytes.

---

# Key Takeaways

- An array is one contiguous block of memory.
    
- The array name usually decays to a pointer to its first element.
    
- Pointer arithmetic moves by the size of the pointed-to type, not by one byte.
    
- `arr[i]` is exactly equivalent to `*(arr + i)`.
    
- `sizeof(arr)` gives the size of the whole array, while `sizeof(pointer)` gives only the size of the address stored in the pointer.
    
- Contiguous memory makes arrays fast because of cache locality.
    

---

## Next Chapter

Now that you understand arrays and pointers, we'll answer another important question:

> **What if we don't know the array size until the program is running?**

That leads us to **dynamic memory**, the **heap**, `new`, `delete`, memory leaks, dangling pointers, and eventually **RAII**, which is one of the defining ideas of modern C++.