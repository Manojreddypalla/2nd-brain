# 📘 NumPy Like an Engineer

# Chapter 3 — Creating Arrays

> **Goal:** Learn every important way to create NumPy arrays and understand **when to use each one**.

---

# Learning Objectives

By the end of this chapter, you'll know:

- `np.array()`
    
- `zeros()`
    
- `ones()`
    
- `empty()`
    
- `full()`
    
- `eye()`
    
- `identity()`
    
- `diag()`
    
- `arange()`
    
- `linspace()`
    
- `logspace()`
    
- `reshape()`
    
- `dtype`
    
- `copy`
    
- `astype()`
    

---

# The Big Picture

There are only **three ways arrays are created**:

```
             Create Arrays
                   │
        ┌──────────┼──────────┐
        │          │          │
 Existing Data  Fill Pattern  Generate Values
        │          │          │
     array()    zeros()    arange()
                ones()     linspace()
                full()     random()
```

---

# 1. `np.array()` — Convert Existing Data

This is the most common way.

```python
import numpy as np

arr = np.array([1,2,3,4])

print(arr)
```

Output

```
[1 2 3 4]
```

---

## 2D Array

```python
arr = np.array([
    [1,2,3],
    [4,5,6]
])

print(arr)
```

Output

```
[[1 2 3]
 [4 5 6]]
```

Think of it like converting a nested Python list into a matrix.

---

# Specify Data Type

```python
arr = np.array([1,2,3], dtype=np.float32)

print(arr)
print(arr.dtype)
```

Output

```
[1. 2. 3.]

float32
```

Useful when you want to save memory or match ML model requirements.

---

# 2. `np.zeros()`

Creates an array filled with **0**.

```python
arr = np.zeros((3,4))

print(arr)
```

Output

```
[[0. 0. 0. 0.]
 [0. 0. 0. 0.]
 [0. 0. 0. 0.]]
```

### Uses

- Initialize matrices
    
- Image buffers
    
- Neural network weights (sometimes)
    
- Counters
    

---

# Integer Zeros

```python
arr = np.zeros((2,3), dtype=np.int32)

print(arr)
```

Output

```
[[0 0 0]
 [0 0 0]]
```

---

# 3. `np.ones()`

Creates an array filled with **1**.

```python
arr = np.ones((2,4))

print(arr)
```

Output

```
[[1. 1. 1. 1.]
 [1. 1. 1. 1.]]
```

Useful for masks, scaling, and testing.

---

# 4. `np.full()`

Fill with **any value**.

```python
arr = np.full((3,3),7)

print(arr)
```

Output

```
[[7 7 7]
 [7 7 7]
 [7 7 7]]
```

---

Another example

```python
np.full((2,5),100)
```

Output

```
100 100 100 100 100
100 100 100 100 100
```

---

# 5. `np.empty()`

One of the most misunderstood functions.

```python
arr = np.empty((3,3))

print(arr)
```

Output (example)

```
[[5.12e-310 ...]
 [...        ...]]
```

Why?

Because **NumPy allocates memory but doesn't initialize it**.

It simply gives you whatever data was already in RAM.

### Use Case

When you're going to overwrite every element anyway, `empty()` can be faster than `zeros()`.

---

# 6. `np.eye()`

Creates an **identity matrix**.

```python
arr = np.eye(4)

print(arr)
```

Output

```
1 0 0 0
0 1 0 0
0 0 1 0
0 0 0 1
```

Diagonal = 1

Everything else = 0

Used heavily in linear algebra.

---

# 7. `np.identity()`

Very similar.

```python
np.identity(4)
```

Produces the same result as `eye(4)`.

Difference:

- `eye()` can create rectangular matrices and offset diagonals.
    
- `identity()` only creates square identity matrices.
    

---

# 8. `np.diag()`

Create a diagonal matrix.

```python
arr = np.diag([1,2,3,4])

print(arr)
```

Output

```
1 0 0 0

0 2 0 0

0 0 3 0

0 0 0 4
```

---

Extract diagonal

```python
matrix = np.array([
    [1,2],
    [3,4]
])

print(np.diag(matrix))
```

Output

```
[1 4]
```

---

# 9. `np.arange()`

Works like Python's `range()`, but returns a NumPy array.

```python
np.arange(10)
```

Output

```
0 1 2 3 4 5 6 7 8 9
```

---

Start and stop

```python
np.arange(5,15)
```

```
5 6 7 8 9 10 11 12 13 14
```

---

Step

```python
np.arange(0,20,2)
```

Output

```
0 2 4 6 8 10 12 14 16 18
```

---

Negative step

```python
np.arange(10,0,-1)
```

Output

```
10 9 8 7 6 5 4 3 2 1
```

---

# 10. `np.linspace()`

Instead of specifying the step size, you specify **how many values you want**.

```python
np.linspace(0,10,5)
```

Output

```
0
2.5
5
7.5
10
```

Think:

> Divide the interval into equal pieces.

---

Common in:

- Graph plotting
    
- Simulations
    
- Machine learning
    

---

# 11. `np.logspace()`

Like `linspace()`, but on a logarithmic scale.

```python
np.logspace(0,3,4)
```

Output

```
1

10

100

1000
```

Used in signal processing and scientific computing.

---

# 12. `reshape()`

Convert

```python
arr = np.arange(12)
```

```
0 1 2 3 4 5 6 7 8 9 10 11
```

into

```python
arr.reshape(3,4)
```

Output

```
0 1 2 3

4 5 6 7

8 9 10 11
```

The number of elements must stay the same.

---

# Automatic Dimension

```python
arr.reshape(-1,4)
```

NumPy calculates the missing dimension automatically.

Very useful.

---

# 13. `astype()`

Convert data type.

```python
arr = np.array([1,2,3])

float_arr = arr.astype(np.float32)

print(float_arr)
```

Output

```
[1. 2. 3.]
```

---

Convert to integer

```python
arr.astype(np.int32)
```

---

# 14. Copy During Creation

```python
arr = np.array([1,2,3], copy=True)
```

Normally not needed, but useful if you explicitly want an independent copy.

---

# Summary Table

|Function|Purpose|
|---|---|
|`array()`|Convert existing data|
|`zeros()`|Fill with 0|
|`ones()`|Fill with 1|
|`full()`|Fill with any value|
|`empty()`|Allocate without initialization|
|`eye()`|Identity matrix|
|`identity()`|Square identity matrix|
|`diag()`|Create/extract diagonal|
|`arange()`|Evenly spaced values (step size)|
|`linspace()`|Evenly spaced values (number of points)|
|`logspace()`|Logarithmic spacing|
|`reshape()`|Change dimensions|
|`astype()`|Change data type|

---

# Real-World Examples

## Create a 5×5 black image

```python
img = np.zeros((5,5), dtype=np.uint8)
```

---

## Create a white image

```python
img = np.full((5,5),255,dtype=np.uint8)
```

---

## Student marks

```python
marks = np.array([85,90,72,65,88])
```

---

## Chessboard size

```python
board = np.zeros((8,8))
```

---

## Identity matrix

```python
weights = np.eye(3)
```

---

# Common Interview Questions

### Q1. Difference between `arange()` and `linspace()`?

**`arange(start, stop, step)`**

- You specify the **step size**.
    
- The stop value is usually **excluded**.
    

```python
np.arange(0, 10, 2)
# [0 2 4 6 8]
```

**`linspace(start, stop, num)`**

- You specify the **number of values**.
    
- The stop value is **included by default**.
    

```python
np.linspace(0, 10, 5)
# [0.  2.5 5.  7.5 10.]
```

---

### Q2. Why is `empty()` faster than `zeros()`?

Because `empty()` **only allocates memory**. It doesn't spend time writing zeros into every element.

---

### Q3. When should you specify `dtype`?

- To save memory (`int32` vs `int64`)
    
- To match model requirements (`float32` is common in deep learning)
    
- To avoid unexpected type conversions
    

---

# Practice

1. Create a `4 × 4` matrix of zeros.
    
2. Create a `3 × 5` matrix of ones.
    
3. Create a `5 × 5` matrix filled with `9`.
    
4. Generate numbers from `10` to `100` with a step of `10`.
    
5. Generate **6** equally spaced numbers between `0` and `1`.
    
6. Create a `3 × 3` identity matrix.
    
7. Create a diagonal matrix using `[5, 10, 15]`.
    
8. Reshape an array of `24` elements into a `4 × 6` matrix.
    
9. Convert an integer array to `float32`.
    
10. Predict the output of `np.empty((2,2))` and explain why it looks that way.
    

---

# Chapter Wrap-Up

Creating arrays isn't just about syntax—it's about **choosing the right initialization strategy** for your problem. Once you're comfortable creating arrays, the next skill is learning how to **inspect** them.

➡️ **Next Chapter:** **Chapter 4 – Array Attributes**, where we'll explore `shape`, `ndim`, `size`, `dtype`, `itemsize`, `nbytes`, `strides`, and how NumPy keeps track of your data internally.