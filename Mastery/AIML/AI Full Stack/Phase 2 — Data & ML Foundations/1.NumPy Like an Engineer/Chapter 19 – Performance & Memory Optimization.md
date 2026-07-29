# Mental Model

Fast NumPy =

```
Less Python

+

More NumPy
```

---

# 1. Avoid Python Loops

❌ Slow

```python
result = []

for x in arr:
    result.append(x*2)
```

---

✅ Fast

```python
arr*2
```

---

# 2. Vectorization

Always prefer

```python
arr+5

arr*3

np.sqrt(arr)
```

Instead of loops.

---

# 3. Broadcasting

Instead of

```python
for row in matrix:
```

Use

```python
matrix+10
```

---

# 4. Use Correct dtype

Default

```python
int64
```

Sometimes

```python
int32
```

uses half the memory.

Images

```python
uint8
```

Only

```
1 Byte
```

per pixel.

---

# 5. Avoid Copies

Bad

```python
copy=arr.copy()
```

if you don't need it.

Better

```python
view=arr.view()
```

Shares memory.

---

# 6. Use Built-in Functions

Instead of

```python
max(arr)
```

Use

```python
arr.max()
```

Same for

```
sum

mean

min

std
```

---

# 7. Preallocate Memory

Instead of

```python
arr=[]

for i in range(1000):
    arr.append(i)
```

Use

```python
np.zeros(1000)
```

Then fill values.

---

# 8. Use In-place Operations

Instead of

```python
arr=arr+5
```

Use

```python
arr+=5
```

Avoids creating another array.

---

# 9. Check Memory

```python
arr.nbytes
```

Shows memory used.

---

# 10. Check Data Type

```python
arr.dtype
```

Very useful for optimization.

---

# Performance Rules

✅ Use vectorization.

✅ Avoid Python loops.

✅ Avoid unnecessary copies.

✅ Choose correct dtype.

✅ Use broadcasting.

---

# Cheat Sheet

| Good | Avoid |
|------|-------|
| `arr*2` | Python loops |
| `view()` | `copy()` |
| `+=` | `arr=arr+...` |
| `float32` | `float64` if unnecessary |
| `uint8` | `int64` for images |

---

# Practice

Find the better version

```python
for i in arr:
```

↓

```python
arr*2
```

---

```python
arr=arr+10
```

↓

```python
arr+=10
```

---

```python
arr.copy()
```

↓

```python
arr.view()
```

(when you don't need a separate copy)

---

# Key Takeaways

- Let NumPy do the work.
- Write fewer Python loops.
- Share memory when possible.
- Use smaller data types when appropriate.
- In-place operations reduce memory usage.