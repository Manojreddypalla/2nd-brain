## C Pointers — Short Notes (Pages 25–49)

Based directly on **L-13(1), pages 25–49**.

### 1. Pointer to Array

```c
int (*t)[10];
```

Read from `t`:

- `(*t)` → `t` is a **pointer**
    
- `[10]` → points to an **array of 10**
    
- `int` → elements are `int`
    

Therefore:

> `t` = pointer to an array of 10 integers.

Important:

```c
int (*t)[10];
```

≠

```c
int *t[10];
```

The second one is an **array of 10 integer pointers**.

---

### 2. Pointer Arithmetic with Arrays

For:

```c
int (*t)[10];
```

`t` points to the **first array of 10 integers**.

```text
t       → row 0
t + 1   → row 1
t + 2   → row 2
```

So:

```c
*(t + 1)
```

means the **second array/row**.

And:

```c
*(t + 1) + 3
```

points to the **4th element of the second row**.

Finally:

```c
*(*(t + 1) + 3)
```

gives the **value** of that element.

**Mental model:**

```text
t
 ↓
[ row 0: 10 ints ]
[ row 1: 10 ints ] ← t+1
[ row 2: 10 ints ]
```

---

### 3. `int *t[10]`

```c
int *t[10];
```

Because `[]` has higher priority than `*`:

```text
t → array[10] → each element is int*
```

So:

> `t` is an **array of 10 pointers to int**.

Compare:

```c
int (*t)[10];   // pointer → array of 10 int
int *t[10];     // array of 10 → int pointers
```

This distinction is **very GATE-relevant**.

---

### 4. `int **t`

```c
int **t;
```

Read outward:

```text
t → pointer → pointer → int
```

Therefore:

> `t` is a pointer to a pointer to an integer.

```text
t
 ↓
pointer
 ↓
int
```

So:

```c
*t
```

→ gives an `int *`

```c
**t
```

→ gives the actual `int` value.

---

# 5. Drawing Pointer Diagrams

Pages 31–47 repeatedly practice the same fundamental skill: **translate declarations into memory diagrams**.

### Example 1

```c
double d = 5.0;
double *pd = &d;
```

Memory:

```text
pd
 ↓
[ d = 5.0 ]
```

`pd` stores the address of `d`.

---

### Example 2

```c
double da[4] = {1.0, 2.0, 3.0, 4.0};
double *pd = da;
```

Since:

```c
da == &da[0]
```

we get:

```text
pd
 ↓
[1.0][2.0][3.0][4.0]
 ↑
da
```

So:

```c
*pd       → 1.0
*(pd+1)   → 2.0
*(pd+2)   → 3.0
```

---

### Example 3

```c
double *pd = &da[1];
```

Now `pd` points to the **second element**:

```text
[1.0][2.0][3.0][4.0]
      ↑
      pd
```

Therefore:

```c
*pd → 2.0
pd + 1 → address of 3.0
```

---

### 6. Pointer to a 2D Array

Given:

```c
double points[3][4] = {
    {1,2,3,4},
    {5,6,7,8},
    {9,10,11,12}
};
```

Declaration:

```c
double (*p1)[4] = points;
```

means:

> `p1` is a pointer to an array of 4 doubles.

```text
p1
 ↓
[1  2  3  4]
[5  6  7  8]
[9 10 11 12]
```

---

### 7. Accessing Rows

```c
double (*p2)[4] = &points[1];
```

`p2` points to the **second row**:

```text
[1  2  3  4]

[5  6  7  8] ← p2

[9 10 11 12]
```

Therefore:

```c
*p2
```

represents the second row.

And:

```c
(*p2)[2]
```

→ `7`.

---

### 8. Accessing an Individual Element

```c
double *p3 = &(*p2)[2];
```

Break it:

```text
p2
 ↓
second row
 ↓
(*p2)[2]
 ↓
7
```

So `p3` points directly to `7`.

```text
[5][6][7][8]
       ↑
       p3
```

---

## ⭐ GATE Pointer Pattern

Whenever you see a complicated pointer, **don't memorize it**.

Read it in this order:

```text
1. Find identifier
2. Look immediately around identifier
3. Resolve () first
4. Resolve [] / *
5. Move outward
```

### Quick translation

|Declaration|Meaning|
|---|---|
|`int *p`|pointer to int|
|`int **p`|pointer to pointer to int|
|`int *p[10]`|array of 10 pointers to int|
|`int (*p)[10]`|pointer to array of 10 int|
|`double (*p)[4]`|pointer to array of 4 doubles|

---

# 9. `malloc` — Introduction

Page 49 starts the **Malloc and Free** section.

`malloc` = **memory allocation**

It dynamically allocates memory from the **heap**.

Basic idea:

```c
int *p = malloc(10 * sizeof(int));
```

Conceptually:

```text
Stack                    Heap

p ─────────────────────→ [ int ][ int ][ int ] ... [ int ]
                           10 integers
```

The pointer variable `p` itself is local, while the dynamically allocated memory is on the heap.

### Core idea

```text
Normal variable:
int x;
→ memory decided automatically

Dynamic allocation:
malloc(...)
→ memory requested at runtime
→ comes from heap
```

---

## 🔥 Pages 25–49 — What You Actually Need to Remember

```text
int *p
→ pointer to int

int **p
→ pointer to pointer

int *p[10]
→ array of 10 int pointers

int (*p)[10]
→ pointer to array of 10 ints

double (*p)[4]
→ pointer to a row containing 4 doubles

p + 1
→ moves according to what p points to

*p
→ go one level toward the data

**p
→ go two levels toward the data

For 2D arrays:
points[i][j]
= *(*(points+i)+j)

For pointer-to-row:
p + 1
→ next row

malloc()
→ dynamically allocates heap memory
```

**Most important GATE skill from these pages:** don't look at `*` and `[]` independently—**find what the identifier ultimately points to and then determine how pointer arithmetic moves.** The lecture's pages 26–30 and 32–47 are essentially training this exact skill.