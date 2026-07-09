# NumPy Chapter 12 – Fancy Indexing

> **Goal:** Select specific elements using index arrays.

---

# Mental Model

Normal Indexing

```python
arr[2]
```

↓

One index

↓

One element

---

Fancy Indexing

```python
arr[[0,2,4]]
```

↓

Many indices

↓

Many elements

---

# Example

```python
import numpy as np

arr = np.array([10,20,30,40,50])
```

---

# Select Multiple Elements

```python
arr[[0,2,4]]
```

Output

```python
[10 30 50]
```

Think

```
Index

0 ✓

2 ✓

4 ✓
```

---

# Reorder Elements

```python
arr[[4,3,2,1,0]]
```

Output

```python
[50 40 30 20 10]
```

You choose the order.

---

# Duplicate Elements

```python
arr[[0,0,0,2]]
```

Output

```python
[10 10 10 30]
```

Duplicates are allowed.

---

# 2D Example

```python
matrix = np.array([
    [1,2,3],
    [4,5,6],
    [7,8,9]
])
```

Single element

```python
matrix[1,2]
```

Output

```
6
```

---

# Multiple Rows

```python
matrix[[0,2]]
```

Output

```python
[[1 2 3]
 [7 8 9]]
```

---

# Multiple Columns

```python
matrix[:,[0,2]]
```

Output

```python
[[1 3]
 [4 6]
 [7 9]]
```

---

# Select Specific Elements

```python
matrix[[0,1,2],[2,1,0]]
```

Think

```
(0,2)

(1,1)

(2,0)
```

Output

```python
[3 5 7]
```

---

# Difference

Normal

```python
arr[2]
```

↓

One index

---

Fancy

```python
arr[[2]]
```

↓

Array of indices

---

# Use Cases

✅ Select specific students

```python
students[[1,5,8]]
```

---

✅ Select specific rows

```python
data[[2,7,10]]
```

---

✅ Select specific columns

```python
data[:,[0,3,5]]
```

---

# Cheat Sheet

| Code | Meaning |
|------|---------|
| `arr[[1,3]]` | Select index 1 and 3 |
| `arr[[4,2,0]]` | Reorder elements |
| `matrix[[0,2]]` | Select rows |
| `matrix[:,[0,2]]` | Select columns |
| `matrix[[0,1],[2,0]]` | Select individual elements |

---

# Practice

```python
arr = np.arange(10)
```

Try

```python
arr[[1,3,5]]
```

```python
arr[[9,8,7]]
```

---

```python
matrix = np.arange(1,17).reshape(4,4)
```

Try

```python
matrix[[0,3]]
```

```python
matrix[:,[1,2]]
```

```python
matrix[[0,2],[1,3]]
```

---

# Key Takeaways

- Fancy indexing uses **arrays of indices**.
- You can reorder elements.
- You can repeat elements.
- Great for selecting specific rows, columns, or positions.