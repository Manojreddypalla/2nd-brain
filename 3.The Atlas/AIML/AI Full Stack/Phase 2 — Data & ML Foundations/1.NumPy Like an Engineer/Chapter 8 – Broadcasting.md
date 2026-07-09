# NumPy Chapter 8 – Broadcasting

> **Goal:** Learn how NumPy performs operations on arrays of different shapes without copying data.

---

# Mental Model

Broadcasting means

> **NumPy automatically expands smaller arrays to match larger ones.**

Example

```python
arr + 10
```

Instead of writing

```python
[
 [1+10,2+10,3+10],
 [4+10,5+10,6+10]
]
```

NumPy does it automatically.

---

# Example 1

```python
import numpy as np

arr = np.array([
    [1,2,3],
    [4,5,6]
])

print(arr + 10)
```

Output

```python
[[11 12 13]
 [14 15 16]]
```

The number `10` is treated like

```python
[
 [10,10,10],
 [10,10,10]
]
```

---

# Example 2 – Row Broadcasting

```python
arr = np.array([
    [1,2,3],
    [4,5,6]
])

row = np.array([10,20,30])

print(arr + row)
```

Output

```python
[[11 22 33]
 [14 25 36]]
```

NumPy imagines

```
10 20 30
10 20 30
```

---

# Example 3 – Column Broadcasting

```python
col = np.array([
    [10],
    [20]
])

print(arr + col)
```

Output

```python
[[11 12 13]
 [24 25 26]]
```

NumPy imagines

```
10 10 10

20 20 20
```

---

# Broadcasting Rules

Compare dimensions **from right to left**.

A dimension is compatible if:

- They are equal
- One of them is `1`

Example

```
(2,3)

+

(3)

↓

Works
```

Because `(3)` becomes `(1,3)`.

---

Example

```
(2,3)

+

(2,1)

↓

Works
```

The `1` expands to `3`.

---

Example

```
(2,3)

+

(2,2)

↓

❌ Error
```

Neither dimension is `1`, and they don't match.

---

# Another Example

```python
a = np.array([1,2,3])

b = np.array([[10],[20]])

print(a + b)
```

Output

```python
[[11 12 13]
 [21 22 23]]
```

Shapes

```
(3)

+

(2,1)

↓

(2,3)
```

---

# Why Broadcasting?

Without broadcasting

```python
for i in range(...):
    for j in range(...):
        ...
```

With broadcasting

```python
arr + 10
```

Less code.

Much faster.

---

# Real AI Example

Normalize pixel values

```python
image = image / 255
```

Increase brightness

```python
image + 50
```

Subtract mean RGB

```python
image - [123,117,104]
```

Broadcasting automatically applies the RGB values to every pixel.

---

# Common Errors

```python
a = np.ones((2,3))

b = np.ones((2,2))

a + b
```

Output

```
ValueError:
operands could not be broadcast together
```

Because

```
(2,3)

≠

(2,2)
```

---

# Cheat Sheet

| Shapes | Result |
|---------|--------|
| `(3)` + `(3)` | ✅ |
| `(2,3)` + `(3)` | ✅ |
| `(2,3)` + `(2,1)` | ✅ |
| `(2,3)` + `(2,2)` | ❌ |
| `(5,4,3)` + `(3)` | ✅ |

---

# Practice

```python
arr = np.arange(6).reshape(2,3)
```

Try:

```python
arr + 5
```

```python
arr * 2
```

```python
arr + np.array([10,20,30])
```

```python
arr + np.array([[100],[200]])
```

Predict the output **before** running the code.

---

# Key Takeaways

- Broadcasting automatically expands smaller arrays.
- No extra copy is created.
- Compare shapes from **right to left**.
- Dimensions must be equal or one of them must be `1`.
- Broadcasting removes the need for many Python loops.