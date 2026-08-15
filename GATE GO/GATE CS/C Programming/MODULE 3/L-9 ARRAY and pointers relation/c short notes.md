# Pointer Arithmetic — Short Notes

### 1. Basic Rule

Pointer arithmetic moves by the **size of the pointed-to data type**, not by 1 byte.

```text
p + i = address(p) + i × sizeof(*p)
```

### 2. Pointer Increment

```c
int *p;
p + 1
```

→ moves to the **next `int`**.

If `sizeof(int) = 4`:

```text
p     → 1000
p + 1 → 1004
p + 2 → 1008
```

For different types:

```text
char *p  → p + 1 = +sizeof(char)
int *p   → p + 1 = +sizeof(int)
long *p  → p + 1 = +sizeof(long)
```

---

### 3. Pointer + Integer

```c
p + i
p - i
```

Moves **`i` elements**, not `i` bytes.

```text
p + i → i × sizeof(*p) bytes
```

---

### 4. Dereferencing

```c
*(p + i)
```

→ accesses the value at the `i`-th element from `p`.

For arrays:

```c
a[i] == *(a + i)
```

---

### 5. Important Difference ⚠️

```c
*p + 1
```

→ value pointed to by `p` + 1

```c
*(p + 1)
```

→ move pointer to next element, then dereference.

---

### 6. Pointer Subtraction

```c
p1 - p2
```

→ gives the **distance in number of elements**, not bytes.

```text
p1 - p2 = number of elements between them
```

---

### 7. Valid Pointer Operations

Generally, pointer arithmetic includes:

```text
p + integer
p - integer
p1 - p2
pointer comparisons: ==, <, >, etc.
```

Pointer arithmetic is generally used with **array elements**.

---

### 🔥 GATE Formula

```text
T *p

p + i
↓
address + i × sizeof(T)
```

**Remember:**

> **Pointer arithmetic is element-based, not byte-based.**