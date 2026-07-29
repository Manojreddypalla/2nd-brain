# NumPy Chapter 17 – Linear Algebra

> **Goal:** Learn the matrix operations used in AI, graphics, physics, and machine learning.

---

# Mental Model

Linear Algebra = **Math with matrices.**

```
Numbers

↓

Arrays

↓

Matrices

↓

Machine Learning
```

---

# Example

```python
import numpy as np

A = np.array([
    [1,2],
    [3,4]
])

B = np.array([
    [5,6],
    [7,8]
])
```

---

# Matrix Addition

```python
A + B
```

Output

```python
[[ 6  8]
 [10 12]]
```

Rule

```
Same position

↓

Add
```

---

# Matrix Subtraction

```python
A - B
```

---

# Element-wise Multiplication

```python
A * B
```

Output

```python
[[ 5 12]
 [21 32]]
```

⚠️ This is **NOT** matrix multiplication.

Each element multiplies its matching element.

---

# Matrix Multiplication ⭐

```python
A @ B
```

or

```python
np.matmul(A,B)
```

or

```python
np.dot(A,B)
```

Output

```python
[[19 22]
 [43 50]]
```

Remember

```
*

↓

Element-wise

-----------------

@

↓

Real Matrix Multiplication
```

---

# Transpose

Swap rows and columns.

```python
A.T
```

Output

```python
[[1 3]
 [2 4]]
```

---

# Determinant

```python
np.linalg.det(A)
```

Used to determine if a matrix is invertible.

---

# Inverse

```python
np.linalg.inv(A)
```

Think

```
Division

for matrices
```

Only works if the matrix is invertible.

---

# Rank

```python
np.linalg.matrix_rank(A)
```

Measures

```
How much independent information

the matrix contains.
```

---

# Eigenvalues

```python
values, vectors = np.linalg.eig(A)
```

Very important in

- PCA
- Computer Vision
- Machine Learning

Don't worry about the math yet.

---

# Norm

```python
np.linalg.norm(A)
```

Think

```
Length

or

Magnitude
```

Used everywhere in AI.

---

# Cheat Sheet

| Function | Purpose |
|----------|---------|
| `+` | Matrix addition |
| `-` | Matrix subtraction |
| `*` | Element-wise multiplication |
| `@` | Matrix multiplication |
| `A.T` | Transpose |
| `np.linalg.det()` | Determinant |
| `np.linalg.inv()` | Inverse |
| `np.linalg.matrix_rank()` | Rank |
| `np.linalg.eig()` | Eigenvalues |
| `np.linalg.norm()` | Magnitude |

---

# Practice

```python
A = np.array([
    [1,2],
    [3,4]
])

B = np.array([
    [5,6],
    [7,8]
])
```

Try

```python
A+B

A-B

A*B

A@B

A.T

np.linalg.det(A)

np.linalg.inv(A)

np.linalg.norm(A)
```

---

# Key Takeaways

- `*` → Element-wise multiplication.
- `@` → Matrix multiplication.
- `A.T` → Transpose.
- Linear algebra powers almost every machine learning algorithm.