## 🧱 1. Python Lists (the built-in array-like structure)

### 🧠 Concept:

A Python list is a **dynamic array** — it can grow and shrink automatically.

Under the hood, it behaves like a C dynamic array.

---

### 🔹 Traverse (Visiting Elements)

**Goal:** Access or print each element.

### Example:

```python
arr = [10, 20, 30, 40, 50]

# Method 1: Using a for loop
for x in arr:
    print(x)

# Method 2: Using index
for i in range(len(arr)):
    print("Index:", i, "Value:", arr[i])

```

✅ **Time complexity:** O(n)

✅ **Space:** O(1)

---

### 🔹 Insert (Add Element)

You can insert elements at:

- **End** → `append()`
- **Specific position** → `insert(index, value)`
- **Another list** → `extend()`

### Example:

```python
arr = [10, 20, 30, 40]

# Insert at end
arr.append(50)         # [10, 20, 30, 40, 50]

# Insert at specific position
arr.insert(2, 99)      # [10, 20, 99, 30, 40, 50]

# Insert multiple elements
arr.extend([60, 70])   # [10, 20, 99, 30, 40, 50, 60, 70]

print(arr)

```

✅ **Time complexity:**

- `append()` → O(1) (amortized)
- `insert()` → O(n) (shifts elements)
- `extend()` → O(k) (where k = number of new elements)

---

### 🔹 Delete (Remove Element)

You can delete by:

- **Value** → `remove(value)`
- **Index** → `pop(index)`
- **Range or condition** → `del` statement or list comprehension

### Example:

```python
arr = [10, 20, 30, 40, 50]

# Delete by value
arr.remove(30)      # removes first 30 → [10, 20, 40, 50]

# Delete by index
arr.pop(1)          # removes value at index 1 → [10, 40, 50]

# Delete using del
del arr[0]          # [40, 50]

# Delete by condition (remove all elements > 45)
arr = [10, 20, 50, 60, 70]
arr = [x for x in arr if x <= 45]
print(arr)          # [10, 20]

```

✅ **Time complexity:**

- `remove()` / `pop(index)` / `del` → O(n)
- `pop()` at end → O(1)

---

### 🧩 Example Program (All 3 Operations)

```python
arr = [10, 20, 30, 40, 50]

# Traverse
print("Traversal:")
for x in arr:
    print(x, end=' ')
print()

# Insert
arr.insert(2, 99)
print("After insertion:", arr)

# Delete
arr.pop(3)
print("After deletion:", arr)

```

---

## ⚙️ 2. NumPy Arrays

Now let’s look at **NumPy arrays** — faster and more numerical.

You’ll need to install NumPy first if not available:

```bash
pip install numpy

```

Then:

```python
import numpy as np
a = np.array([10, 20, 30, 40, 50])

```

---

### 🔹 Traverse

You can loop through or use vectorized operations.

```python
for i in a:
    print(i)

# OR vectorized
print(a * 2)     # Multiply all by 2

```

✅ **Time:** O(n)

✅ **Vectorized ops are much faster**

---

### 🔹 Insert

NumPy arrays are **fixed-size**, so insertion creates a **new array**.

```python
import numpy as np
a = np.array([10, 20, 30, 40])

b = np.insert(a, 2, 99)
print("Original:", a)
print("After insertion:", b)

```

Output:

```
Original: [10 20 30 40]
After insertion: [10 20 99 30 40]

```

✅ **Time:** O(n) (creates a new copy)

---

### 🔹 Delete

```python
a = np.array([10, 20, 30, 40, 50])
b = np.delete(a, 2)
print("After deletion:", b)

```

Output:

```
After deletion: [10 20 40 50]

```

✅ **Time:** O(n)

---

### 🔹 Traverse, Insert, Delete — All Together

```python
import numpy as np

a = np.array([10, 20, 30, 40, 50])

# Traverse
for x in a:
    print(x, end=' ')
print()

# Insert
a = np.insert(a, 2, 99)
print("After insertion:", a)

# Delete
a = np.delete(a, 3)
print("After deletion:", a)

```

---

## 🧮 Comparison — Python List vs NumPy Array

|Feature|Python List|NumPy Array|
|---|---|---|
|Type|Can mix types|Single data type|
|Dynamic|Grows/Shrinks easily|Fixed-size|
|Insertion|In-place|Creates new array|
|Deletion|In-place|Creates new array|
|Speed (loops)|Slower|Much faster (C-level)|
|Arithmetic ops|Not supported directly|Vectorized|

---

## ✅ Quick Summary

| Operation  | Python List                       | NumPy Array                |
| ---------- | --------------------------------- | -------------------------- |
| Traverse   | `for x in arr:`                   | same                       |
| Insert     | `arr.insert(i, val)` / `append()` | `np.insert(arr, i, val)`   |
| Delete     | `arr.pop(i)` / `remove()`         | `np.delete(arr, i)`        |
| Complexity | O(1)–O(n)                         | O(n) (new array each time) |