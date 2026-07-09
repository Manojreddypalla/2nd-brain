# 📘 NumPy Like an Engineer

# Chapter 4 — Array Attributes

> **Goal:** Learn how to inspect an array.
> 
> Think of these attributes as an **ID card** for your NumPy array.

---

# Imagine You Download an Image

```python
import numpy as np

img = np.random.randint(0,255,(720,1280,3),dtype=np.uint8)
```

Before editing it, you'd probably ask:

- How big is it?
- How many dimensions?
- What type is each pixel?
- How much memory does it use?

That's exactly what array attributes answer.

---

# 1. `.shape`

The most used attribute.

```python
arr = np.array([
    [1,2,3],
    [4,5,6]
])

print(arr.shape)
```

Output

```python 
(2,3)
```

Meaning

```python
2 Rows

3 Columns
```

Think:

```
shape = structure
```

---

### Example

```python
img = np.zeros((1080,1920,3))
```

```python
Shape

(1080,1920,3)
```

Meaning

```
1080 pixels tall

1920 pixels wide

3 color channels
```

---

# 2. `.ndim`

How many dimensions?

```python 
print(arr.ndim)
```

Output

```
2
```

Because

```
[
 [1 2 3]
 [4 5 6]
]
```

is a **2D array**.

---

Examples

```
[1 2 3]

↓

1 Dimension
```

---

```
[
 [1 2]
 [3 4]
]

↓

2 Dimensions
```

---

```
10 Images

↓

64×64

↓

RGB

↓

Shape

(10,64,64,3)

↓

4 Dimensions
```

---

Think

```
ndim = Number of Axes
```

---

# 3. `.size`

Total number of elements.

```
arr = np.arange(12).reshape(3,4)

print(arr.size)
```

Output

```
12
```

Because

```
3 × 4 = 12
```

---

Another example

```
Shape

(100,200)

↓

100 × 200

↓

20,000 elements
```

---

Think

```
size = total values
```

---

# 4. `.dtype`

What type is every element?

```
arr = np.array([1,2,3])

print(arr.dtype)
```

Output

```
int64
```

---

Float

```
arr = np.array([1.5,2.5])

print(arr.dtype)
```

```
float64
```

---

Boolean

```
arr = np.array([True,False])

print(arr.dtype)
```

```
bool
```

---

Most common

```
int32

int64

float32

float64

uint8

bool
```

---

### Why it matters

AI models usually use

```
float32
```

because it uses **half the memory** of `float64`.

---

# 5. `.itemsize`

How much memory does ONE element use?

```
arr = np.array([1,2,3],dtype=np.int32)

print(arr.itemsize)
```

Output

```
4
```

Meaning

```
Each integer

↓

4 Bytes
```

---

If

```
dtype=int64
```

then

```
itemsize

↓

8 Bytes
```

---

Think

```
itemsize = size of one box
```

---

# 6. `.nbytes`

Total memory used.

Formula

```
itemsize × size
```

Example

```
arr = np.array([1,2,3],dtype=np.int32)

print(arr.nbytes)
```

Output

```
12
```

Because

```
3 Elements

×

4 Bytes

=

12 Bytes
```

---

Example

```
1000 float32 numbers

↓

1000 × 4

↓

4000 Bytes
```

---

Think

```
nbytes = total RAM used
```

---

# 7. `.strides`

Don't panic.

This looks scary.

It isn't.

```
arr = np.array([
    [1,2,3],
    [4,5,6]
],dtype=np.int32)

print(arr.strides)
```

Output

```
(12,4)
```

Meaning

```
Move 12 Bytes

↓

Next Row
```

```
Move 4 Bytes

↓

Next Column
```

Because

```
int32

↓

4 Bytes
```

Each row has

```
3 numbers

↓

3×4

↓

12 Bytes
```

---

We'll master this later.

For now just remember

```
Strides

↓

How NumPy walks through memory
```

---

# 8. `.T`

Transpose

Swap rows and columns.

```
arr = np.array([
    [1,2,3],
    [4,5,6]
])

print(arr.T)
```

Output

```
1 4

2 5

3 6
```

Rows become columns.

---

Very common in

- Machine Learning
- Linear Algebra

---

# 9. `.flags`

Rarely used.

```
print(arr.flags)
```

Shows

- writable
- contiguous
- ownership

Mostly useful for debugging.

---

# Quick Example

```
import numpy as np

arr = np.arange(12).reshape(3,4)

print(arr)

print(arr.shape)

print(arr.ndim)

print(arr.size)

print(arr.dtype)

print(arr.itemsize)

print(arr.nbytes)

print(arr.strides)
```

Try to predict each output before running it.

---

# Cheat Sheet

|Attribute|Meaning|
|---|---|
|`shape`|Rows, columns, dimensions|
|`ndim`|Number of dimensions|
|`size`|Total elements|
|`dtype`|Data type|
|`itemsize`|Bytes per element|
|`nbytes`|Total memory|
|`strides`|Memory jump|
|`T`|Transpose|

---

# Real AI Examples

### Dataset

```
images.shape
```

```
(50000,32,32,3)
```

Meaning

```
50,000 Images

32×32 Pixels

RGB
```

---

### Feature Matrix

```
X.shape
```

```
(1000,20)
```

Meaning

```
1000 Samples

20 Features
```

---

### Neural Network Output

```
predictions.shape
```

```
(64,10)
```

Meaning

```
64 Images

10 Classes
```

---

# Practice

```
arr = np.arange(24).reshape(2,3,4)
```

Without running the code, answer:

1. What is `arr.shape`?
2. What is `arr.ndim`?
3. What is `arr.size`?
4. What is `arr.dtype`?
5. If the dtype is `int64`, what is `arr.itemsize`?
6. What is `arr.nbytes`?

---

# 🧠 Mental Model

Every NumPy array has an invisible **information card** attached to it:

```
Array
│
├── shape     → What does it look like?
├── ndim      → How many axes?
├── size      → How many values?
├── dtype     → What type?
├── itemsize  → Size of one value
├── nbytes    → Total memory
└── strides   → How to move through memory
```

Notice how much shorter this chapter is while still covering the important details. I think this style will make the rest of the handbook much easier to study.

**Next chapter (Indexing)** is one of the most important in NumPy. I'll explain it visually so that expressions like `arr[:, 1]`, `arr[1:3, 2:]`, and even 3D indexing become almost second nature.