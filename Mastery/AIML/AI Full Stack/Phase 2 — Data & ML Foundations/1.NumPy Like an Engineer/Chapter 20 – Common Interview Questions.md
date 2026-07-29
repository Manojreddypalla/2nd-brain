# NumPy Chapter 20 – Common Interview Questions

> **Goal:** Know the questions interviewers frequently ask about NumPy.

---

# 1. What is NumPy?

A Python library for fast numerical computing using **N-dimensional arrays (ndarray)**.

---

# 2. Why is NumPy faster than Python lists?

- Contiguous memory
- Homogeneous data types
- Optimized C implementation
- Vectorization
- Better CPU cache utilization

---

# 3. Difference: Python List vs NumPy Array

| Python List | NumPy Array |
|-------------|-------------|
| Mixed data types | Same data type |
| Slower | Faster |
| More memory | Less memory |
| No vectorization | Vectorized operations |

---

# 4. What is Broadcasting?

Automatically expands smaller arrays so operations between different shapes are possible.

---

# 5. What is Vectorization?

Performing operations on an entire array instead of writing Python loops.

---

# 6. What is a ufunc?

A Universal Function.

Example

```python
np.sqrt(arr)
```

Applies the function to every element.

---

# 7. Difference: `*` vs `@`

```python
A * B
```

↓

Element-wise multiplication

---

```python
A @ B
```

↓

Matrix multiplication

---

# 8. Difference: `copy()` vs `view()`

`copy()`

```
New memory
```

`view()`

```
Shared memory
```

---

# 9. Difference: `flatten()` vs `ravel()`

`flatten()`

```
Always copy
```

`ravel()`

```
Usually shares memory
```

---

# 10. Difference: `arange()` vs `linspace()`

`arange()`

```
Specify step
```

---

`linspace()`

```
Specify number of values
```

---

# 11. What does `shape` return?

The dimensions of an array.

Example

```python
(3,4)
```

↓

3 rows

4 columns

---

# 12. Difference: `size` vs `shape`

`shape`

↓

Structure

---

`size`

↓

Total elements

---

# 13. Difference: `max()` vs `argmax()`

`max()`

↓

Maximum value

---

`argmax()`

↓

Index of maximum value

---

# 14. What is Boolean Masking?

Filtering elements using True/False conditions.

Example

```python
arr[arr>10]
```

---

# 15. What is Fancy Indexing?

Selecting elements using index arrays.

Example

```python
arr[[1,3,5]]
```

---

# 16. Difference: `concatenate()` vs `stack()`

`concatenate()`

↓

Join existing axis

---

`stack()`

↓

Create a new axis

---

# 17. Why use `np.random.seed()`?

To make random results reproducible.

---

# 18. What is `dtype`?

The data type of every element in the array.

---

# 19. What is `ndim`?

Number of dimensions.

---

# 20. What is `nbytes`?

Total memory used by the array.

---

# Top 10 Functions to Remember

```python
np.array()

reshape()

sum()

mean()

max()

min()

where()

sort()

unique()

random.randint()
```

---

# Interview Cheat Sheet

```
shape

↓

Rows & Columns

----------------

size

↓

Total Elements

----------------

ndim

↓

Dimensions

----------------

dtype

↓

Data Type

----------------

itemsize

↓

Bytes per Element

----------------

nbytes

↓

Total Memory

----------------

max

↓

Value

----------------

argmax

↓

Index

----------------

*

↓

Element-wise

----------------

@

↓

Matrix Multiplication
```

---

# Most Asked Questions ⭐⭐⭐⭐⭐

If you remember these, you're already ahead of many beginners.

✅ Why NumPy is faster than Python.

✅ Broadcasting.

✅ Vectorization.

✅ copy() vs view().

✅ flatten() vs ravel().

✅ Matrix multiplication.

✅ Boolean masking.

✅ shape vs size vs ndim.

---

# Key Takeaways

Most interview questions are **concept-based**, not API-based.

Understand the ideas behind NumPy, and the functions become easy to remember.