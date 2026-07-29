Good catch. The previous version looked like a code block because of the markdown fencing.

Below is a **proper Obsidian note**. Just copy and paste it into a `.md` file. It uses normal Markdown headings, tables, callouts, and code blocks only where they're useful.

---

# 📐 Mathematics Cheat Sheet for NumPy, AI & Machine Learning

> **Purpose:** Quick revision of the math concepts you'll repeatedly encounter in NumPy, AI, Data Science, Computer Vision, and Graphics.

---

# 🧱 1. Scalars, Vectors, Matrices & Tensors

|Type|Example|Dimensions|
|---|---|---|
|Scalar|`5`|0D|
|Vector|`[1, 2, 3]`|1D|
|Matrix|`[[1,2],[3,4]]`|2D|
|Tensor|Multi-dimensional array|3D+|

**NumPy Connection**

```python
scalar = 5

vector = np.array([1,2,3])

matrix = np.array([[1,2],[3,4]])

tensor = np.random.rand(2,3,4)
```

---

# 📏 2. Matrix Shape

A matrix always has

```
Rows × Columns
```

Example

```
[
 [1 2 3]
 [4 5 6]
]
```

Shape

```
2 × 3
```

NumPy

```python
arr.shape
```

---

# ➕ 3. Matrix Addition

Requirements

- Same shape
    

Example

```
1 2

3 4

+

5 6

7 8

=

6 8

10 12
```

NumPy

```python
A + B
```

---

# ✖️ 4. Matrix Multiplication

Rule

```
(m × n)

×

(n × p)

↓

(m × p)
```

Example

```
(2×3)

×

(3×4)

↓

(2×4)
```

NumPy

```python
A @ B
```

> **Remember**
> 
> `*` = Element-wise multiplication
> 
> `@` = Matrix multiplication

---

# 🔄 5. Transpose

Swap rows and columns.

Before

```
1 2

3 4
```

After

```
1 3

2 4
```

NumPy

```python
A.T
```

---

# 🪞 6. Identity Matrix

```
1 0 0

0 1 0

0 0 1
```

Acts like **1** in multiplication.

NumPy

```python
np.eye(3)
```

---

# 🔢 7. Determinant

A number describing a square matrix.

```
det(A)
```

If

```
det(A) = 0
```

then

- Matrix cannot be inverted.
    

NumPy

```python
np.linalg.det(A)
```

---

# 🔄 8. Matrix Inverse

Think of it as

> Division for matrices.

NumPy

```python
np.linalg.inv(A)
```

Requirement

```
det(A) ≠ 0
```

---

# 🎯 9. Dot Product

Formula

```
a · b

=

Σ(ai × bi)
```

Example

```
[1 2 3]

•

[4 5 6]

=

1×4

+

2×5

+

3×6

=

32
```

NumPy

```python
np.dot(a,b)
```

---

# 📐 10. Vector Norm

Norm means

> Length (Magnitude)

Formula

```
‖v‖

=

√(x²+y²)
```

Example

```
(3,4)

↓

5
```

NumPy

```python
np.linalg.norm(v)
```

---

# 📊 11. Mean

Formula

```
Mean

=

Sum

÷

Count
```

NumPy

```python
arr.mean()
```

---

# 📈 12. Variance

Measures how spread out values are.

Formula

```
Variance

=

Average((x-Mean)²)
```

NumPy

```python
arr.var()
```

---

# 📉 13. Standard Deviation

Formula

```
Std

=

√Variance
```

Interpretation

- Small → Values are close together.
    
- Large → Values are spread apart.
    

NumPy

```python
arr.std()
```

---

# 🎲 14. Probability

Range

```
0 → Impossible

1 → Certain
```

Used in Machine Learning predictions.

---

# 🎚️ 15. Normalization

Convert values to a smaller range.

Example

```
0–255

↓

0–1
```

NumPy

```python
image / 255.0
```

---

# 🧠 16. Rank

Rank tells you

> How much independent information a matrix contains.

NumPy

```python
np.linalg.matrix_rank(A)
```

---

# 📡 17. Eigenvalues & Eigenvectors

Used in

- PCA
    
- Computer Vision
    
- Graphics
    
- Machine Learning
    

For now remember

> They describe the important directions and scaling of a transformation.

NumPy

```python
np.linalg.eig(A)
```

---

# 📦 18. Broadcasting

A smaller array automatically expands.

Example

```python
arr + 10
```

Think

```
10

↓

10 10 10

10 10 10
```

---

# ⚡ 19. Vectorization

Instead of

```
Loop

↓

Loop

↓

Loop
```

NumPy performs

```
Whole Array

↓

One Operation
```

---

# 📍 20. Axis

|Axis|Meaning|
|---|---|
|`axis=0`|Operate down the rows (column-wise result)|
|`axis=1`|Operate across the columns (row-wise result)|

Example

```python
arr.sum(axis=0)

arr.sum(axis=1)
```

---

# 🖼️ 21. RGB Image

One pixel

```
[R,G,B]
```

Example

```
[255,120,80]
```

Image Shape

```
(Height, Width, Channels)
```

Example

```
(720,1280,3)
```

---

# ⭐ Essential Formulas

|Concept|Formula|
|---|---|
|Mean|Sum ÷ Count|
|Variance|Average((x − Mean)²)|
|Standard Deviation|√Variance|
|Distance|√((x₂−x₁)² + (y₂−y₁)²)|
|Dot Product|Σ(ai × bi)|
|Matrix Shape|Rows × Columns|
|Matrix Multiplication|(m×n) × (n×p) → (m×p)|

---

# 🧠 Memory Map

```text
Scalar
   │
   ▼
Vector
   │
   ▼
Matrix
   │
   ▼
Tensor
   │
   ▼
NumPy
   │
   ▼
Machine Learning
   │
   ▼
Deep Learning
```

---

# 🚀 Most Important Concepts to Master

- ✅ Matrix Multiplication
    
- ✅ Dot Product
    
- ✅ Mean
    
- ✅ Standard Deviation
    
- ✅ Normalization
    
- ✅ Matrix Shape
    
- ✅ Transpose
    
- ✅ Vector Norm
    
- ✅ Broadcasting
    
- ✅ Vectorization
    

---

# 💡 Quick Revision

|Symbol / Function|Meaning|
|---|---|
|`+`|Addition|
|`*`|Element-wise Multiplication|
|`@`|Matrix Multiplication|
|`.T`|Transpose|
|`det()`|Determinant|
|`inv()`|Matrix Inverse|
|`mean()`|Average|
|`std()`|Standard Deviation|
|`argmax()`|Index of Maximum Value|
|`norm()`|Vector Length|

---

> 📌 **Rule of Thumb**
> 
> If you're learning **AI, Computer Vision, Graphics, Robotics, or Game Physics**, these math concepts will appear over and over again. You don't need to memorize every formula today—understand what each concept represents and where it's used. That's what will make the later topics much easier.