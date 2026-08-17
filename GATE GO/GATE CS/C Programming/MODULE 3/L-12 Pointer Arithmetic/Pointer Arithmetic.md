# Pointer Arithmetic — GATE C Notes

> **Source:** L-12.pdf, 20 pages. The lecture focuses on pointer arithmetic, pointer subtraction, and how pointer arithmetic behaves with multidimensional arrays.

---

## 1. Pointer Arithmetic — Core Idea

A pointer does **not** move byte-by-byte when we do `p + 1`.

It moves by the **size of the object it points to**.

```c
int *p;
p + 1
```

means:

```text
address(p) + sizeof(int)
```

If `sizeof(int) = 4`:

```text
p     → 1000
p + 1 → 1004
p + 2 → 1008
p + 3 → 1012
```

### General Rule

```text
p + k = address(p) + k × sizeof(*p)
```

So pointer arithmetic is **type-dependent**.

---

# 2. Valid Pointer Arithmetic

For a pointer `p`, the valid arithmetic operations covered in the lecture are:

```c
p + integer
p - integer

p1 - p2       // only when both belong to same array
```

Also valid comparisons:

```c
p1 < p2
p1 <= p2
p1 > p2
p1 >= p2
p1 == p2
```

The relational comparisons and subtraction are meaningful when the pointers refer to elements of the **same array object**.

---

# 3. Pointer + Integer

Consider:

```c
int a[10];
int *p = a;
```

If:

```text
p = 1000
sizeof(int) = 4
```

Then:

```text
p + 1 = 1004
p + 2 = 1008
p + 3 = 1012
```

But conceptually:

```text
p + 3
```

means:

```text
3 elements ahead
```

not "3 bytes ahead".

---

# 4. Pointer Subtraction

This is a **very important GATE concept**.

```c
p2 - p1
```

does **NOT** give the byte-address difference.

It gives the **number of array elements between the pointers**.

### Example

```c
int a[10];

int *p1 = &a[0];
int *p2 = &a[3];

printf("%d", p2 - p1);
```

Result:

```text
3
```

Even though:

```text
address difference = 12 bytes
```

because:

```text
sizeof(int) = 4
```

So:

```text
p2 - p1
= (address2 - address1) / sizeof(int)
= 12 / 4
= 3
```

The lecture explicitly emphasizes that subtraction produces an **integer distance**, not an address.

---

# 5. HUGE GATE TRAP — Same Array Requirement

Pointer subtraction is meaningful only when:

```text
p1 and p2 → elements of the SAME array
```

Example:

```c
int a[10];

p1 = &a[2];
p2 = &a[7];

p2 - p1   // valid → 5
```

But:

```c
int a[10], b[10];

p1 = &a[2];
p2 = &b[7];

p2 - p1
```

is **undefined behavior**.

### Remember

```text
Same array → pointer subtraction meaningful
Different arrays → undefined behavior
```

The lecture specifically warns that the result is undefined when the pointers do not belong to the same array.

---

# 6. Negative Pointer Difference

Subtraction can be negative.

```c
int a[10];

int *p1 = &a[5];
int *p2 = &a[2];

p1 - p2 = 3
p2 - p1 = -3
```

Think in terms of **array indices**:

```text
p1 → index 5
p2 → index 2

p1 - p2 = 5 - 2 = 3
p2 - p1 = 2 - 5 = -3
```

---

# 7. Pointer Arithmetic Depends on Pointed Type

Suppose:

```c
int *p;
char *q;
```

Then:

```text
p + 1 → + sizeof(int)
q + 1 → + sizeof(char)
```

So always ask:

> **"What type does this pointer point to?"**

That determines its movement.

---

# 8. Multidimensional Arrays — The Important Mental Model

Consider:

```c
int a[3][3];
```

Do **NOT** initially imagine this as one mysterious 2D object.

Think:

```text
a
│
├── row 0 → [ int ][ int ][ int ]
├── row 1 → [ int ][ int ][ int ]
└── row 2 → [ int ][ int ][ int ]
```

The type of `a` is effectively:

```c
int (*)[3]
```

when used in most expressions.

So:

```text
a + 1
```

moves by **one entire row**.

This is the key to all the multidimensional pointer-arithmetic questions in the lecture.

---

# 9. `a`, `*a`, `a+1`, `*(a+1)`

For:

```c
int a[3][3];
```

### `a`

Points to the first row.

```text
a → row 0
```

Type:

```c
int (*)[3]
```

---

### `a + 1`

Moves to the next row.

```text
a + 1 → row 1
```

Movement:

```text
sizeof(row)
= 3 × sizeof(int)
```

---

### `*a`

Dereferences the row pointer.

```text
*a → first row
```

That row behaves like a pointer to its first element.

Therefore:

```c
**a
```

gives:

```text
a[0][0]
```

---

### `*(a + 1)`

Gives the second row.

```text
*(a + 1) → row 1
```

and:

```c
*(*(a + 1))
```

gives:

```text
a[1][0]
```

---

# 10. Why `a[1] - a[0]` Works

For:

```c
int a[3][3];
```

each:

```c
a[0]
a[1]
a[2]
```

represents a row containing 3 integers.

So:

```c
a[1] - a[0]
```

means:

```text
address of row1's first element
-
address of row0's first element
```

Since each row contains 3 integers:

```text
difference = 3
```

The lecture demonstrates this using the row structure and `sizeof(int)`.

---

# 11. `a[i]` vs `&a[i]`

This distinction is **GATE-important**.

For:

```c
int a[3][3];
```

### `a[i]`

Represents the `i`th row and usually decays to:

```c
int *
```

because it points to the first integer of that row.

### `&a[i]`

Points to the **whole row**:

```c
int (*)[3]
```

Therefore:

```text
a[i] + 1
```

moves by:

```text
sizeof(int)
```

while:

```text
&a[i] + 1
```

moves by:

```text
sizeof(entire row)
= 3 × sizeof(int)
```

---

# 12. General Multidimensional Rule

For:

```c
int a[R][C];
```

### `a`

```text
int (*)[C]
```

### `a + 1`

moves:

```text
C × sizeof(int)
```

### `a[i]`

behaves as:

```text
int *
```

### `a[i] + 1`

moves:

```text
sizeof(int)
```

### `&a[i] + 1`

moves:

```text
C × sizeof(int)
```

This is the underlying pattern behind the lecture's 2D/3D examples.

---

# 13. 3D Arrays — Same Pattern Continues

Consider:

```c
int a[2][3][4];
```

Visualize:

```text
a
│
├── a[0]
│   ├── a[0][0] → 4 ints
│   ├── a[0][1] → 4 ints
│   └── a[0][2] → 4 ints
│
└── a[1]
    ├── a[1][0] → 4 ints
    ├── a[1][1] → 4 ints
    └── a[1][2] → 4 ints
```

The important rule:

> **Pointer arithmetic moves according to the type of the object the pointer currently points to.**

So don't memorize separate rules for 2D and 3D arrays.

Just determine the **pointed-to type**.

---

# 14. How to Solve Address Questions

Suppose:

```c
int a[2][3][4];
```

and:

```text
a = 1000
sizeof(int) = 4
```

To find:

```c
&a[1]
```

Ask:

### Step 1 — What does `a` point to?

```text
a → a[0]
```

`a[0]` contains:

```text
3 × 4 = 12 integers
```

Size:

```text
12 × 4 = 48 bytes
```

### Step 2 — Move one element

```text
a + 1
= 1000 + 48
= 1048
```

Therefore:

```text
&a[1] = 1048
```

The lecture repeatedly uses this **base address + number of elements × element size** method for multidimensional arrays.

---

# 15. The Universal Formula

For:

```c
T *p;
```

```text
p + k
=
address(p) + k × sizeof(T)
```

This is the single formula to remember.

### Example

```c
int *p;
p + 3
```

```text
address + 3 × sizeof(int)
```

For:

```c
int (*p)[3];
```

```text
p + 3
```

means:

```text
address + 3 × sizeof(int[3])
```

That is:

```text
address + 3 × 3 × sizeof(int)
```

---

# 16. Valid vs Invalid Pointer Operations

## ✅ Valid

```c
p + 3
p - 3
p1 - p2       // same array
p1 < p2       // same array
p1 <= p2
p1 > p2
p1 >= p2
p1 == p2
```

## ❌ Invalid

```c
p1 + p2
p1 * p2
p1 / p2
p1 % p2
```

Pointer + pointer, multiplication, division and modulo are invalid pointer arithmetic.

---

# 17. GATE Pattern — Identify the Pointer Type First

Whenever you see something like:

```c
a + 1
&a[1] + 1
a[1] + 1
*(a + 1)
```

**DO NOT calculate immediately.**

First ask:

```text
What is the type of this expression?
        ↓
What object does the pointer point to?
        ↓
What is sizeof(that object)?
        ↓
How far does +1 move?
```

This prevents most multidimensional-array mistakes.

---

# 18. Quick Type Table

For:

```c
int a[3][4];
```

|Expression|Meaning|Effective pointer type|
|---|---|---|
|`a`|first row|`int (*)[4]`|
|`a + 1`|next row|`int (*)[4]`|
|`*a`|first row|`int [4]` → `int *` in expression|
|`a[0]`|first row's first element|`int *` in expression|
|`a[0] + 1`|next integer|`int *`|
|`&a[0]`|address of whole row|`int (*)[4]`|
|`&a[0] + 1`|next row|`int (*)[4]`|

---

# 19. GATE Traps ⚠️

### Trap 1 — Pointer difference ≠ byte difference

```c
p2 - p1
```

returns:

```text
number of elements
```

not bytes.

---

### Trap 2 — Different arrays

```c
int a[5], b[5];

&a[2] - &b[2]
```

Do **not** calculate the numerical address difference.

The operation has undefined behavior because they are different array objects.

---

### Trap 3 — `a + 1` in 2D array

For:

```c
int a[3][4];
```

`a + 1` does **not** move by one `int`.

It moves by:

```text
one complete row
= 4 ints
```

---

### Trap 4 — `a[i] + 1`

For:

```c
int a[3][4];
```

`a[i]` points to an `int`, so:

```c
a[i] + 1
```

moves by:

```text
sizeof(int)
```

---

### Trap 5 — `&a[i] + 1`

This points to a whole row, so it moves by:

```text
sizeof(row)
```

not `sizeof(int)`.

---

# 20. 🔥 One Mental Model for Everything

Don't memorize:

> "2D array has special pointer rules."

There aren't really special rules.

There is only:

> **Pointer arithmetic moves by the size of the object pointed to.**

For:

```c
int *p
```

```text
*p = int
```

so:

```text
p + 1 → next int
```

For:

```c
int (*p)[4]
```

```text
*p = array of 4 ints
```

so:

```text
p + 1 → next array of 4 ints
```

For:

```c
int (*p)[3][4]
```

```text
*p = 3×4 array of ints
```

so:

```text
p + 1 → next 3×4 block
```

**This is the pattern GATE wants you to recognize.**

---

# ⚡ Quick Revision

```text
Pointer arithmetic:
p + k = address + k × sizeof(*p)

Pointer subtraction:
p2 - p1 = number of elements between them

Requirement:
p1 and p2 must point into the same array object

For int a[3][4]:

a          → pointer to row
a + 1      → next row
a[i]       → pointer to int
a[i] + 1   → next int
&a[i] + 1  → next row

Valid:
p ± integer
p1 - p2
pointer comparisons

Invalid:
p1 + p2
p1 * p2
p1 / p2
p1 % p2
```

### 🎯 GATE Question Trigger

Whenever a question gives:

```c
int a[...][...];
```

and asks for an **address/value involving `+`, `-`, `*`, `&`**:

**1. Find the pointer type → 2. Find pointed object → 3. Find its `sizeof` → 4. Apply pointer arithmetic.**

That 4-step process is much safer than memorizing formulas for individual array dimensions.