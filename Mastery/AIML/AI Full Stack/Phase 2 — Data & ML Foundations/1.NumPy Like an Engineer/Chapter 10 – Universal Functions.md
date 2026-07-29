# NumPy Chapter 10 – Universal Functions (ufuncs)

> **Goal:** Learn how NumPy performs fast mathematical operations on entire arrays.

---

# What are ufuncs?

**ufunc = Universal Function**

A ufunc is simply a **function that operates on every element of an array.**

Think

```
Array

↓

Function

↓

Every element is processed
```

---

# Example

```python
import numpy as np

arr = np.array([1,4,9,16])

print(np.sqrt(arr))
```

Output

```python
[1. 2. 3. 4.]
```

Instead of writing a loop, NumPy applies `sqrt()` to every element.

---

# Common ufuncs

## Square Root

```python
np.sqrt(arr)
```

---

## Square

```python
np.square(arr)
```

---

## Absolute Value

```python
arr = np.array([-5,-2,3])

np.abs(arr)
```

Output

```python
[5 2 3]
```

---

## Exponential

```python
np.exp(arr)
```

Calculates

```
e^x
```

---

## Logarithm

```python
np.log(arr)
```

Natural logarithm

---

## Power

```python
np.power(arr,2)
```

Same as

```python
arr ** 2
```

---

# Trigonometric Functions

```python
np.sin(arr)

np.cos(arr)

np.tan(arr)
```

Remember

Angles are in **radians**, not degrees.

Convert degrees

```python
np.deg2rad(90)
```

---

# Rounding

```python
arr = np.array([1.2,2.7,3.5])
```

Round

```python
np.round(arr)
```

Floor

```python
np.floor(arr)
```

Output

```
1 2 3
```

Ceil

```python
np.ceil(arr)
```

Output

```
2 3 4
```

---

# Minimum & Maximum

```python
a = np.array([1,5,3])

b = np.array([2,4,6])

np.maximum(a,b)
```

Output

```
[2 5 6]
```

---

```python
np.minimum(a,b)
```

Output

```
[1 4 3]
```

---

# Clip Values

Limit values to a range.

```python
arr = np.array([10,50,300])

np.clip(arr,0,255)
```

Output

```
[10 50 255]
```

Very useful in image processing.

---

# Comparison Functions

```python
arr = np.array([1,2,3,4])

arr > 2
```

Output

```
[False False True True]
```

These results are often used for filtering.

---

# Why ufuncs?

Without NumPy

```python
result = []

for i in arr:
    result.append(i**2)
```

With NumPy

```python
np.square(arr)
```

Cleaner.

Faster.

More readable.

---

# Cheat Sheet

| Function | Purpose |
|----------|---------|
| `np.sqrt()` | Square root |
| `np.square()` | Square |
| `np.abs()` | Absolute value |
| `np.exp()` | Exponential |
| `np.log()` | Natural log |
| `np.power()` | Raise to a power |
| `np.sin()` | Sine |
| `np.cos()` | Cosine |
| `np.tan()` | Tangent |
| `np.round()` | Round |
| `np.floor()` | Round down |
| `np.ceil()` | Round up |
| `np.maximum()` | Element-wise maximum |
| `np.minimum()` | Element-wise minimum |
| `np.clip()` | Limit values |

---

# Practice

```python
arr = np.array([1,4,9,16,25])
```

Try:

```python
np.sqrt(arr)
```

```python
np.square(arr)
```

```python
np.log(arr)
```

```python
np.exp(arr)
```

```python
np.clip(arr,5,20)
```

```python
np.round(np.array([2.3,4.8,6.5]))
```

Predict the output before running.

---

# Key Takeaways

- A **ufunc** applies a function to every element of an array.
- No Python loops are needed.
- Ufuncs are fast because they're implemented in optimized C code.
- They're heavily used in AI, data science, simulations, and image processing.