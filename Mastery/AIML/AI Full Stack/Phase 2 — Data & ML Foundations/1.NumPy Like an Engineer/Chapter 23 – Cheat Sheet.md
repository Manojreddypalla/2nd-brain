# NumPy Chapter 23 – Cheat Sheet

> **Goal:** Quick reference for the most useful NumPy operations.

---

# Import

```python
import numpy as np
```

---

# Create Arrays

```python
np.array([1,2,3])

np.zeros((3,3))

np.ones((3,3))

np.full((3,3),7)

np.eye(3)

np.arange(10)

np.linspace(0,1,5)
```

---

# Information

```python
arr.shape

arr.ndim

arr.size

arr.dtype

arr.itemsize

arr.nbytes
```

---

# Reshape

```python
arr.reshape(2,3)

arr.flatten()

arr.ravel()
```

---

# Indexing

```python
arr[0]

arr[-1]

arr[1,2]

arr[:,0]

arr[0,:]
```

---

# Slicing

```python
arr[1:4]

arr[:,1:3]

arr[::-1]

arr[:,::-1]
```

---

# Broadcasting

```python
arr + 10

arr * 5
```

---

# Math

```python
arr + arr

arr - arr

arr * arr

arr / arr

arr ** 2

np.sqrt(arr)
```

---

# Statistics

```python
arr.sum()

arr.mean()

arr.max()

arr.min()

arr.std()

arr.var()

arr.argmax()

arr.argmin()
```

---

# Boolean Mask

```python
arr[arr > 10]

arr[arr % 2 == 0]

arr[(arr > 5) & (arr < 20)]
```

---

# Sorting

```python
np.sort(arr)

np.argsort(arr)

np.unique(arr)
```

---

# Random

```python
np.random.rand()

np.random.randint()

np.random.choice()

np.random.shuffle()

np.random.seed(42)
```

---

# Linear Algebra

```python
A @ B

A.T

np.linalg.inv(A)

np.linalg.det(A)

np.linalg.norm(A)
```

---

# Joining

```python
np.concatenate()

np.stack()

np.vstack()

np.hstack()
```

---

# Splitting

```python
np.split()

np.vsplit()

np.hsplit()
```

---

# Images

```python
image[100:300,200:400]

np.rot90(image)

np.fliplr(image)

np.flipud(image)

255-image

np.clip(image+50,0,255)
```

---

# Performance Rules

✅ Use vectorization

✅ Avoid Python loops

✅ Use broadcasting

✅ Avoid unnecessary copy()

✅ Choose correct dtype

---

# Most Important APIs

```python
np.array()

reshape()

sum()

mean()

max()

min()

where()

sort()

unique()

random.randint()

@
```

---

# Most Important Concepts

⭐ Arrays

⭐ Memory

⭐ Views vs Copies

⭐ Broadcasting

⭐ Vectorization

⭐ Matrix Multiplication

⭐ Boolean Masking

⭐ Axis

---

# Remember

```
*  -> Element-wise

@  -> Matrix Multiplication

----------------------------

max()

↓

Value

argmax()

↓

Index

----------------------------

copy()

↓

New Memory

view()

↓

Shared Memory

----------------------------

flatten()

↓

Copy

ravel()

↓

Usually View
```