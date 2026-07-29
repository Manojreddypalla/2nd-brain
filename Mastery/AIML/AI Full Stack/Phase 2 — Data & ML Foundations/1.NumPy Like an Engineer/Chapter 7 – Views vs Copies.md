# NumPy Chapter 7 – Views vs Copies

> **Goal:** Understand when NumPy shares memory and when it creates new memory.

---

# Mental Model

There are **three ways** another array can be created.

```
Original Array
      │
      ├── Assignment (=)
      ├── View (shares memory)
      └── Copy (new memory)
```

---

# 1. Assignment (`=`)

```python
import numpy as np

arr = np.array([1,2,3])

b = arr

b[0] = 100

print(arr)
```

Output

```python
[100 2 3]
```

Why?

```
arr
  │
  ▼
[1 2 3]
  ▲
  │
 b
```

Both variables point to the **same array**.

---

# 2. View

```python
arr = np.array([1,2,3])

view = arr.view()

view[1] = 50

print(arr)
```

Output

```python
[1 50 3]
```

A **view shares the same memory** but is a different NumPy object.

```
Shared Memory

arr ──────► [1 50 3]
view ─────►
```

Check

```python
print(np.shares_memory(arr, view))
```

Output

```python
True
```

---

# 3. Copy

```python
arr = np.array([1,2,3])

copy = arr.copy()

copy[0] = 999

print(arr)
print(copy)
```

Output

```python
[1 2 3]

[999 2 3]
```

Memory

```
arr  ──► [1 2 3]

copy ─► [999 2 3]
```

Independent arrays.

---

# 4. `reshape()`

Usually returns a **view** if possible.

```python
arr = np.arange(6)

b = arr.reshape(2,3)
```

Changing one may affect the other because they often share memory.

---

# 5. `flatten()` vs `ravel()`

### `flatten()`

Always creates a copy.

```python
flat = arr.flatten()
```

Memory

```
New Array
```

---

### `ravel()`

Returns a view whenever possible.

```python
flat = arr.ravel()
```

Memory

```
Shared
```

---

# Quick Comparison

| Function | Shares Memory? |
|-----------|----------------|
| `=` | ✅ Yes |
| `view()` | ✅ Yes |
| `reshape()` | Usually |
| `ravel()` | Usually |
| `copy()` | ❌ No |
| `flatten()` | ❌ No |

---

# Check Memory Sharing

```python
np.shares_memory(arr, b)
```

Returns

```python
True
```

or

```python
False
```

---

# Common Mistake

```python
b = arr

b[0] = 100
```

Many people expect `arr` to stay unchanged.

It won't.

Both variables refer to the same data.

---

# Practice

```python
arr = np.array([10,20,30])
```

Try:

1. Create an assignment (`=`) and modify it.
2. Create a view and modify it.
3. Create a copy and modify it.
4. Compare `flatten()` and `ravel()`.
5. Use `np.shares_memory()` to verify your results.

---

# Key Takeaways

- `=` → Same object.
- `view()` → Different object, same memory.
- `copy()` → New object, new memory.
- `ravel()` → Usually shares memory.
- `flatten()` → Always creates a new array.