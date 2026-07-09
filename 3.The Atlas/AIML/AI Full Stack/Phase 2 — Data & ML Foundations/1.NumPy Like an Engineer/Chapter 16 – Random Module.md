# NumPy Chapter 16 – Random Module

> **Goal:** Generate random numbers for testing, AI, and simulations.

---

# Mental Model

```
Need random numbers?

↓

Use np.random
```

---

# Random Float

Between **0 and 1**

```python
import numpy as np

np.random.rand()
```

Example

```
0.7345...
```

---

Multiple values

```python
np.random.rand(5)
```

Output

```python
[0.12 0.89 0.44 0.77 0.31]
```

---

Matrix

```python
np.random.rand(3,3)
```

---

# Random Integer

```python
np.random.randint(1,10)
```

Output

```python
7
```

Notice

```
1 included

10 excluded
```

---

Matrix

```python
np.random.randint(0,100,(3,3))
```

Example

```python
[[54 13 82]
 [ 9 61 40]
 [27 91 15]]
```

---

# Random Choice

Pick randomly from values.

```python
colors = ["Red","Blue","Green"]

np.random.choice(colors)
```

Output

```
Blue
```

---

Pick multiple

```python
np.random.choice(colors,3)
```

---

# Shuffle

Changes the original array.

```python
arr = np.array([1,2,3,4,5])

np.random.shuffle(arr)

print(arr)
```

Example

```python
[3 1 5 2 4]
```

---

# Permutation

Returns a shuffled copy.

```python
arr = np.array([1,2,3])

new = np.random.permutation(arr)
```

Original stays unchanged.

---

# Seed

Very important.

```python
np.random.seed(42)
```

Every time you run

```python
np.random.randint(1,10,5)
```

You get the **same numbers**.

Useful for

- AI
- Testing
- Debugging

---

# Normal Distribution

```python
np.random.randn(5)
```

Example

```python
[-0.34 1.21 -0.88 0.11 0.45]
```

Mean ≈ 0

Standard deviation ≈ 1

Very common in Machine Learning.

---

# Uniform Distribution

```python
np.random.uniform(10,20,5)
```

Output

```
Random floats between

10

and

20
```

---

# Cheat Sheet

| Function | Purpose |
|----------|---------|
| `rand()` | Random float (0–1) |
| `randint()` | Random integer |
| `choice()` | Random selection |
| `shuffle()` | Shuffle original |
| `permutation()` | Shuffled copy |
| `seed()` | Repeat random results |
| `randn()` | Normal distribution |
| `uniform()` | Random float in range |

---

# Practice

```python
np.random.rand(5)
```

```python
np.random.randint(1,50,10)
```

```python
np.random.choice(["A","B","C"])
```

```python
np.random.shuffle(arr)
```

```python
np.random.seed(42)
```

Run twice and observe.

---

# Key Takeaways

- `rand()` → Random decimal.
- `randint()` → Random integer.
- `choice()` → Pick random items.
- `shuffle()` → Changes original array.
- `permutation()` → Creates a shuffled copy.
- `seed()` → Makes random results repeatable.