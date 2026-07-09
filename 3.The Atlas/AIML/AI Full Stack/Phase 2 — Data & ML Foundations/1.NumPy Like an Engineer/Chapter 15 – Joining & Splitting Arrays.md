# NumPy Chapter 15 – Joining & Splitting Arrays

> **Goal:** Combine multiple arrays or divide one array into smaller arrays.

---

# Mental Model

Joining

```
Array A

+

Array B

↓

One Bigger Array
```

Splitting

```
One Big Array

↓

Multiple Small Arrays
```

---

# Example Arrays

```python
import numpy as np

a = np.array([1,2,3])

b = np.array([4,5,6])
```

---

# concatenate()

Join arrays.

```python
np.concatenate((a,b))
```

Output

```python
[1 2 3 4 5 6]
```

---

# stack()

Creates a **new dimension**.

```python
np.stack((a,b))
```

Output

```python
[[1 2 3]
 [4 5 6]]
```

Notice

```
1D + 1D

↓

2D
```

---

# vstack()

Vertical Stack

```python
np.vstack((a,b))
```

Output

```python
[[1 2 3]
 [4 5 6]]
```

Think

```
↓

↓

↓

Stack Down
```

---

# hstack()

Horizontal Stack

```python
np.hstack((a,b))
```

Output

```python
[1 2 3 4 5 6]
```

Think

```
→ → →
```

---

# 2D Example

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

---

Vertical

```python
np.vstack((A,B))
```

Output

```python
[[1 2]
 [3 4]
 [5 6]
 [7 8]]
```

Shape

```
(4,2)
```

---

Horizontal

```python
np.hstack((A,B))
```

Output

```python
[[1 2 5 6]
 [3 4 7 8]]
```

Shape

```
(2,4)
```

---

# split()

Split an array.

```python
arr = np.arange(8)

np.split(arr,2)
```

Output

```python
[array([0,1,2,3]),
 array([4,5,6,7])]
```

---

# vsplit()

Split rows.

```python
matrix = np.arange(16).reshape(4,4)

np.vsplit(matrix,2)
```

↓

Two matrices

```
Top Half

Bottom Half
```

---

# hsplit()

Split columns.

```python
np.hsplit(matrix,2)
```

↓

```
Left Half

Right Half
```

---

# Shape Rule

Joining requires compatible shapes.

Example

```
(2,3)

+

(2,3)

↓

Horizontal ✅

↓

(2,6)
```

---

```
(2,3)

+

(3,2)

↓

❌ Error
```

---

# Cheat Sheet

| Function | Purpose |
|----------|---------|
| concatenate() | Join arrays |
| stack() | Add new dimension |
| vstack() | Join vertically |
| hstack() | Join horizontally |
| split() | Split array |
| vsplit() | Split rows |
| hsplit() | Split columns |

---

# Practice

```python
a = np.array([1,2,3])

b = np.array([4,5,6])
```

Try

```python
np.concatenate((a,b))

np.stack((a,b))

np.vstack((a,b))

np.hstack((a,b))
```

---

# Key Takeaways

- `concatenate()` → Simple join.
- `stack()` → Adds a new dimension.
- `vstack()` → Stack downward.
- `hstack()` → Stack sideways.
- `split()` → Break an array into smaller arrays.