# NumPy Chapter 9 – Vectorization

> **Goal:** Perform operations on an entire array without writing loops.

---

# Mental Model

Instead of saying

> "Process one element at a time"

You say

> "Process the entire array."

---

# Without NumPy

```python
numbers = [1,2,3,4,5]

result = []

for i in numbers:
    result.append(i * 2)

print(result)
```

Output

```python
[2,4,6,8,10]
```

---

# With NumPy

```python
import numpy as np

arr = np.array([1,2,3,4,5])

print(arr * 2)
```

Output

```python
[2 4 6 8 10]
```

No loop needed.

---

# Why is it Faster?

Python

```
Loop

↓

Read element

↓

Check type

↓

Multiply

↓

Store result

↓

Repeat...
```

NumPy

```
Whole Array

↓

Optimized C Code

↓

Done
```

Less Python overhead = More speed.

---

# Common Vectorized Operations

## Addition

```python
arr + 10
```

---

## Multiplication

```python
arr * 5
```

---

## Division

```python
arr / 2
```

---

## Square

```python
arr ** 2
```

---

## Square Root

```python
np.sqrt(arr)
```

---

## Exponential

```python
np.exp(arr)
```

---

## Logarithm

```python
np.log(arr)
```

---

# Comparison

```python
arr > 3
```

Output

```python
[False False False True True]
```

Every element is checked at once.

---

# Boolean Filtering

```python
arr[arr > 3]
```

Output

```python
[4 5]
```

---

# Vectorized Math

```python
a = np.array([1,2,3])

b = np.array([10,20,30])

print(a + b)
```

Output

```python
[11 22 33]
```

---

# Real AI Example

Normalize image pixels

```python
image = image / 255.0
```

Increase brightness

```python
image = image + 20
```

Apply threshold

```python
image[image < 100] = 0
```

All pixels are processed together.

---

# Avoid This

```python
for i in range(len(arr)):
    arr[i] *= 2
```

Instead

```python
arr *= 2
```

Cleaner and much faster.

---

# Cheat Sheet

| Operation | Example |
|-----------|---------|
| Add | `arr + 5` |
| Subtract | `arr - 5` |
| Multiply | `arr * 5` |
| Divide | `arr / 2` |
| Power | `arr ** 2` |
| Square Root | `np.sqrt(arr)` |
| Log | `np.log(arr)` |
| Exp | `np.exp(arr)` |
| Compare | `arr > 5` |

---

# Practice

```python
arr = np.array([2,4,6,8,10])
```

Try:

```python
arr + 5
```

```python
arr * 3
```

```python
arr / 2
```

```python
arr ** 2
```

```python
np.sqrt(arr)
```

```python
arr[arr > 5]
```

Predict the output before running.

---

# Key Takeaways

- Vectorization means operating on **entire arrays**.
- Avoid Python loops whenever possible.
- NumPy uses optimized compiled code internally.
- Vectorized code is shorter, cleaner, and much faster.