# 📘 NumPy Like an Engineer

# Chapter 2 — Arrays & Memory (The Heart of NumPy)

> **Goal:** Understand **how NumPy stores data in memory** and why it's so fast.

---

# Learning Objectives

By the end of this chapter, you'll understand:

- What happens in RAM
    
- Why Python lists are slow
    
- How NumPy arrays are stored
    
- What contiguous memory means
    
- Why homogeneous data matters
    
- What pointers are
    
- Cache locality
    
- `itemsize`, `nbytes`, `dtype`
    
- Memory addresses
    
- C-order vs Fortran-order
    
- Strides (intro)
    

---

# Before We Begin...

Most people think this:

```python
arr = np.array([1,2,3,4])
```

creates "an array."

Not exactly.

It creates **a block of memory** with a very specific layout.

Understanding that layout explains almost every NumPy feature.

---

# 1. What is RAM?

RAM (Random Access Memory) is where your program stores data while it's running.

Imagine RAM as a long street of numbered houses.

```text
Address

1000
1001
1002
1003
1004
1005
...
```

Each address stores **1 byte**.

---

Suppose we store

```python
10
```

An integer might occupy several bytes (for example, 4 or 8 bytes depending on the type).

So memory could look like

```text
1000
1001
1002
1003
```

Those bytes together represent one integer.

---

# 2. Python List Memory

Let's create

```python
numbers = [10,20,30]
```

Many beginners imagine this:

```text
+----+----+----+
|10  |20  |30  |
+----+----+----+
```

**Wrong.**

---

Python lists actually store **references (pointers)** to Python objects.

```text
numbers

+--------+
|   •----|------+
+--------+      |
|   •-----------|------+
+--------+      |      |
|   •------------------|------+
+--------+      |      |      |
                ↓      ↓      ↓

              10     20     30
```

The list contains **addresses**, not the integers themselves.

Each integer is a separate Python object somewhere else in memory.

---

## Why?

Because Python allows

```python
data = [
    10,
    3.14,
    "Hello",
    True
]
```

Different types need different internal representations.

Python solves this by storing references to objects.

---

# 3. Why is This Slower?

Suppose you write

```python
for x in numbers:
```

The CPU has to:

1. Read a pointer.
    
2. Jump to another memory location.
    
3. Read the object.
    
4. Check its type.
    
5. Get its value.
    
6. Perform the operation.
    
7. Repeat.
    

This involves many memory lookups.

Memory access is often slower than arithmetic itself.

---

# 4. NumPy Memory

Now create

```python
arr = np.array([10,20,30])
```

Memory looks more like this:

```text
Address

1000
1008
1016

+----+----+----+
|10  |20  |30  |
+----+----+----+
```

Everything is stored **next to each other**.

No scattered objects.

No type checks for every element.

This is called **contiguous memory**.

---

# 5. Contiguous Memory

Contiguous means

> Stored continuously without gaps.

Example

```text
1000 → 10

1008 → 20

1016 → 30

1024 → 40
```

Notice the addresses increase by the same amount because every element has the same size.

The CPU can efficiently read one element after another.

---

# 6. Homogeneous Data

NumPy arrays contain **one data type**.

Good:

```python
np.array([1,2,3,4])
```

Good:

```python
np.array([1.2,3.4,5.6])
```

Bad (mixed types):

```python
np.array([1,"hello",3])
```

NumPy will convert everything to a common type if possible:

```python
arr = np.array([1,2.5,3])
print(arr)
print(arr.dtype)
```

Output:

```text
[1.  2.5 3. ]
float64
```

It promotes integers to floats to keep the array homogeneous.

---

# 7. Why Homogeneous Data Helps

Imagine shelves in a warehouse.

Mixed sizes:

```text
📦 📚 🧸 🍎
```

Hard to organize.

Same size boxes:

```text
📦 📦 📦 📦
```

Easy to stack and locate.

That's exactly what NumPy does with memory.

---

# 8. dtype

Every NumPy array has a single data type.

```python
arr = np.array([1,2,3])

print(arr.dtype)
```

Output:

```text
int64
```

Examples:

```python
np.int8

np.int16

np.int32

np.int64

np.float32

np.float64

np.bool_

np.complex64
```

Choosing smaller types can save memory when appropriate.

---

# 9. itemsize

How many bytes does each element occupy?

```python
arr = np.array([1,2,3], dtype=np.int32)

print(arr.itemsize)
```

Output:

```text
4
```

Each `int32` uses 4 bytes.

For `int64`:

```text
8
```

---

# 10. nbytes

Total memory used by the array.

```python
arr = np.array([1,2,3], dtype=np.int32)

print(arr.nbytes)
```

Output:

```text
12
```

Because:

```text
3 elements × 4 bytes = 12 bytes
```

---

# 11. shape

```python
arr = np.zeros((3,4))

print(arr.shape)
```

Output:

```text
(3,4)
```

Meaning:

```text
3 rows

4 columns
```

---

# 12. ndim

```python
print(arr.ndim)
```

Output

```text
2
```

Because it's a 2D array.

---

# 13. size

```python
print(arr.size)
```

Output

```text
12
```

3 × 4 = 12 elements.

---

# 14. Memory Address

You can inspect the memory address of the array's data buffer:

```python
arr = np.array([10,20,30])

print(arr.ctypes.data)
```

You'll get a large integer representing the starting address in memory.

---

# 15. C-order vs Fortran-order

Consider:

```python
arr = np.array([
    [1,2,3],
    [4,5,6]
])
```

## C-order (Row-major)

Stored row by row:

```text
1 2 3 4 5 6
```

This is NumPy's default.

---

## Fortran-order (Column-major)

Stored column by column:

```text
1 4 2 5 3 6
```

Some scientific libraries use this layout.

---

# 16. Strides (Introduction)

Strides tell NumPy:

> **"How many bytes should I move in memory to reach the next element?"**

Example:

```python
arr = np.array([[1,2,3],
                [4,5,6]], dtype=np.int32)

print(arr.strides)
```

Typical output:

```text
(12, 4)
```

How do we read this?

- Move **12 bytes** to go to the next row.
    
- Move **4 bytes** to go to the next column.
    

Because:

```text
int32 = 4 bytes

Row has 3 elements

3 × 4 = 12 bytes
```

We'll revisit strides in more detail later because they're the reason slicing can often avoid copying data.

---

# 17. Why CPU Cache Loves NumPy

Modern CPUs read memory in chunks called **cache lines**.

When values are contiguous:

```text
10 20 30 40 50 60
```

The CPU can fetch many values at once.

If values are scattered:

```text
10 ...... 20 ......... 30
```

It has to perform many more memory accesses.

This is one reason NumPy operations are so fast.

---

# 18. Summary

- RAM is a sequence of memory addresses.
    
- Python lists store **references to objects**.
    
- NumPy arrays store **raw values contiguously**.
    
- Homogeneous data enables efficient storage and computation.
    
- `dtype` defines the type of every element.
    
- `itemsize` tells you the size of one element.
    
- `nbytes` is the total memory usage.
    
- `shape`, `ndim`, and `size` describe the array's structure.
    
- NumPy uses **row-major (C-order)** by default.
    
- Strides describe how to move through memory.
    
- Contiguous memory + CPU cache + compiled code = NumPy's speed.
    

---

# Practice

```python
import numpy as np

arr = np.arange(12).reshape(3,4)
```

Try to answer before running the code:

1. What is `arr.shape`?
    
2. What is `arr.ndim`?
    
3. What is `arr.size`?
    
4. What is `arr.dtype`?
    
5. What is `arr.itemsize`?
    
6. What is `arr.nbytes`?
    
7. What does `arr.strides` mean?
    
8. Print the starting memory address with `arr.ctypes.data`.
    

---

# Key Takeaway

The biggest mindset shift is this:

> **A NumPy array is not just a collection of numbers. It is a carefully organized block of memory.**

Once you see NumPy this way, concepts like slicing, broadcasting, views, reshaping, and vectorization stop feeling like magic—they become logical consequences of how the data is laid out in memory.

➡️ **Next:** **Chapter 3 – Creating Arrays**, where we'll explore every major way to construct arrays and when to use each one.