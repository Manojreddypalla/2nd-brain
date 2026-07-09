# NumPy Chapter 14 – Sorting & Searching

> **Goal:** Sort arrays and find values or positions.

---

# Sorting

```python
import numpy as np

arr = np.array([5,2,9,1,7])

np.sort(arr)
```

Output

```python
[1 2 5 7 9]
```

Original array is unchanged.

---

# Descending

```python
np.sort(arr)[::-1]
```

Output

```python
[9 7 5 2 1]
```

---

# 2D Sorting

```python
matrix = np.array([
    [3,1,2],
    [9,5,6]
])
```

Sort rows

```python
np.sort(matrix)
```

Output

```python
[[1 2 3]
 [5 6 9]]
```

---

Sort columns

```python
np.sort(matrix, axis=0)
```

---

# argsort()

Returns **indices**, not values.

```python
arr = np.array([30,10,20])

np.argsort(arr)
```

Output

```python
[1 2 0]
```

Meaning

```
10 → index 1

20 → index 2

30 → index 0
```

---

# max & argmax

```python
arr.max()
```

↓

Largest value

---

```python
arr.argmax()
```

↓

Index of largest value

---

# min & argmin

```python
arr.min()

arr.argmin()
```

---

# unique()

Remove duplicates.

```python
arr = np.array([1,2,2,3,3,3])

np.unique(arr)
```

Output

```python
[1 2 3]
```

---

# Count Duplicates

```python
values, counts = np.unique(arr, return_counts=True)

print(values)

print(counts)
```

Output

```python
[1 2 3]

[1 2 3]
```

Meaning

```
1 appears once

2 appears twice

3 appears three times
```

---

# where()

Find indices matching a condition.

```python
arr = np.array([10,20,30,40])

np.where(arr > 20)
```

Output

```python
(array([2,3]),)
```

Meaning

```
30 → index 2

40 → index 3
```

---

# Cheat Sheet

| Function | Purpose |
|----------|---------|
| `np.sort()` | Sort values |
| `[::-1]` | Reverse |
| `np.argsort()` | Indices after sorting |
| `np.unique()` | Remove duplicates |
| `np.where()` | Find matching indices |
| `argmax()` | Index of max |
| `argmin()` | Index of min |

---

# Practice

```python
arr = np.array([9,2,7,4,2,9,1])
```

Try

```python
np.sort(arr)

np.unique(arr)

np.argsort(arr)

np.where(arr > 5)

arr.argmax()

arr.argmin()
```

---

# Key Takeaways

- `sort()` → Sort values.
- `argsort()` → Sort indices.
- `unique()` → Remove duplicates.
- `where()` → Find positions.