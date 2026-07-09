# NumPy Chapter 11 – Boolean Masking

> **Goal:** Filter and modify data using True/False conditions.

---

# Mental Model

Think of a mask like a filter.

```
Array

↓

Condition

↓

True / False

↓

Keep only True values
```

---

# Example Array

```python
import numpy as np

arr = np.array([10,20,30,40,50])
```

---

# Create a Mask

```python
mask = arr > 25

print(mask)
```

Output

```python
[False False True True True]
```

Each element is checked.

---

# Filter Values

```python
print(arr[arr > 25])
```

Output

```python
[30 40 50]
```

Think

```
30 ✓

40 ✓

50 ✓

Keep them
```

---

# Common Comparisons

Greater than

```python
arr > 20
```

Less than

```python
arr < 20
```

Equal

```python
arr == 30
```

Not Equal

```python
arr != 30
```

Greater or Equal

```python
arr >= 30
```

Less or Equal

```python
arr <= 30
```

---

# Multiple Conditions

Use

```
&
```

for **AND**

```python
arr[(arr > 20) & (arr < 50)]
```

Output

```python
[30 40]
```

---

Use

```
|
```

for **OR**

```python
arr[(arr < 20) | (arr > 40)]
```

Output

```python
[10 50]
```

---

Use

```
~
```

for **NOT**

```python
arr[~(arr > 30)]
```

Output

```python
[10 20 30]
```

---

# Modify Values

Replace values

```python
arr[arr > 30] = 0

print(arr)
```

Output

```python
[10 20 30  0  0]
```

---

# Even Numbers

```python
arr[arr % 2 == 0]
```

---

# Odd Numbers

```python
arr[arr % 2 != 0]
```

---

# 2D Example

```python
matrix = np.array([
    [10,20],
    [30,40]
])
```

Filter

```python
matrix[matrix > 20]
```

Output

```python
[30 40]
```

Notice

The result becomes **1D**.

---

# Real AI Example

Threshold an image

```python
image[image < 100] = 0
```

Every dark pixel becomes black.

---

Normalize bright pixels

```python
image[image > 200] = 255
```

---

# Common Mistake

Wrong

```python
(arr > 10) and (arr < 20)
```

❌ Doesn't work.

Correct

```python
(arr > 10) & (arr < 20)
```

Always use

```
&

|

~
```

for NumPy arrays.

---

# Cheat Sheet

| Condition | Example |
|-----------|---------|
| Greater | `arr > 5` |
| Less | `arr < 5` |
| Equal | `arr == 5` |
| Not Equal | `arr != 5` |
| AND | `&` |
| OR | `\|` |
| NOT | `~` |
| Filter | `arr[arr>5]` |
| Modify | `arr[arr>5]=0` |

---

# Practice

```python
arr = np.array([5,10,15,20,25,30])
```

Try:

```python
arr[arr > 15]
```

```python
arr[arr % 2 == 0]
```

```python
arr[(arr > 10) & (arr < 30)]
```

```python
arr[arr >= 20] = 100
```

Predict the output before running.

---

# Key Takeaways

- A Boolean mask is an array of **True/False** values.
- Use masks to **filter** or **modify** data.
- Use `&`, `|`, and `~` for combining conditions.
- Boolean masking is heavily used in AI, image processing, and data cleaning.