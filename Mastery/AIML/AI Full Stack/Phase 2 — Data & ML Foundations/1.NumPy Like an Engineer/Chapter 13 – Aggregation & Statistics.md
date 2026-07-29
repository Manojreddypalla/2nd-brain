# Mental Model

Aggregation means

```
Many Values

↓

One Result
```

Example

```
1 2 3 4 5

↓

Sum

↓

15
```

---

# Example Array

```python
import numpy as np

arr = np.array([
    [10,20,30],
    [40,50,60]
])
```

---

# Sum

```python
arr.sum()
```

Output

```python
210
```

---

# Mean (Average)

```python
arr.mean()
```

Output

```python
35
```

Formula

```
Sum

÷

Number of Elements
```

---

# Maximum

```python
arr.max()
```

Output

```python
60
```

---

# Minimum

```python
arr.min()
```

Output

```python
10
```

---

# Standard Deviation

```python
arr.std()
```

Measures

```
How spread out the values are.
```

Large std

↓

Values are far apart.

Small std

↓

Values are close together.

---

# Variance

```python
arr.var()
```

Variance = Standard Deviation²

---

# Axis

This is the only confusing part.

## axis = 0

```
↓

↓

↓

Columns
```

```python
arr.sum(axis=0)
```

Output

```python
[50 70 90]
```

Because

```
10+40

20+50

30+60
```

---

## axis = 1

```
→ →
```

Rows

```python
arr.sum(axis=1)
```

Output

```python
[60 150]
```

Because

```
10+20+30

40+50+60
```

---

# Argmax

Returns

```
Index

NOT value
```

```python
a = np.array([4,9,2])

a.argmax()
```

Output

```python
1
```

Because

```
9

↓

Index 1
```

---

# Argmin

```python
a.argmin()
```

Output

```python
2
```

---

# Median

```python
np.median(a)
```

Middle value after sorting.

---

# Cheat Sheet

| Function | Purpose |
|----------|---------|
| `sum()` | Total |
| `mean()` | Average |
| `max()` | Largest |
| `min()` | Smallest |
| `std()` | Standard deviation |
| `var()` | Variance |
| `argmax()` | Index of max |
| `argmin()` | Index of min |
| `median()` | Middle value |

---

# Practice

```python
arr = np.arange(1,10).reshape(3,3)
```

Try

```python
arr.sum()
```

```python
arr.mean()
```

```python
arr.max()
```

```python
arr.min()
```

```python
arr.sum(axis=0)
```

```python
arr.sum(axis=1)
```

```python
arr.argmax()
```

---

# Key Takeaways

- Aggregation reduces many values into one.
- `axis=0` → operate down the columns.
- `axis=1` → operate across the rows.
- These functions are used constantly in AI, ML, and data analysis.