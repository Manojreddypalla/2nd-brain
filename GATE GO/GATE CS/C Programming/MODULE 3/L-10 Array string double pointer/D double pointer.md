# Double Pointer — GATE Notes

## 1. What is a Double Pointer?

A normal pointer stores the address of a variable:

```c
int x = 100;
int *p = &x;
```

Think:

```text
p ──→ x
     100
```

A **double pointer** stores the address of a pointer:

```c
int **p2 = &p;
```

Now:

```text
p2 ──→ p ──→ x
              100
```

So:

```text
int x
 ↓
int *p
 ↓
int **p2
```

The lecture describes this as **"pointer to a pointer."**

---

# 2. The Most Important Part: `*`

Suppose:

```c
int x = 100;
int *p = &x;
int **p2 = &p;
```

Then:

```c
p2
```

means:

> address of `p`

```c
*p2
```

means:

> go to `p2` → get `p`

So:

```text
*p2 == p
```

And:

```c
**p2
```

means:

> go through `p2` → get `p` → go through `p` → get `x`

Therefore:

```text
**p2 == x == 100
```

### 🧠 Mental model

```text
p2 ──→ p ──→ x
 ↑      ↑      ↑
**p2   *p2    **p2
              = x
```

Actually, more precisely:

```text
p2      → address of p
*p2     → p
**p2    → x
```

---

# 3. Accessing the Same Value in Different Ways

Given:

```c
int x = 100;
int *p = &x;
int **p2 = &p;
```

All of these ultimately access `100`:

```c
x
*p
**p2
```

The lecture explicitly demonstrates this idea.

---

# 4. `**p2 = value`

This is extremely important.

```c
**p2 = 200;
```

Trace it:

```text
p2
 ↓
 p
 ↓
 x
```

So:

```text
**p2 = 200
```

changes `x`.

```text
Before:

x = 100

After:

x = 200
```

---

# 5. Changing What a Pointer Points To

Consider:

```c
int x = 100;
int y = 200;

int *p1 = &x;
int **p2 = &p1;
```

Initially:

```text
p2
 ↓
p1
 ↓
x = 100
```

Now:

```c
*p2 = &y;
```

What happened?

`*p2` is `p1`.

So this is essentially:

```c
p1 = &y;
```

Now:

```text
p2
 ↓
p1
 ↓
y = 200
```

Therefore:

```c
**p2
```

now gives:

```text
200
```

The lecture demonstrates exactly this pointer-redirection idea.

---

# 6. Why Double Pointers Are Useful

A normal pointer lets a function modify the **value** of a variable:

```c
void fun(int *p)
{
    *p = 100;
}
```

A double pointer lets a function modify the **pointer itself**.

Example:

```c
void fun(int **p)
{
    *p = ...;
}
```

Because:

```text
*p
```

is itself a pointer.

This is one of the major reasons you'll see `int **` in C.

---

# 7. Double Pointer + Array

The lecture gives:

```c
int x[3] = {5, 7, 9};

int *myptr;
int **ourptr;

myptr = x;
ourptr = &myptr;
```

Memory concept:

```text
x
↓
┌───┬───┬───┐
│ 5 │ 7 │ 9 │
└───┴───┴───┘
 ↑
myptr

ourptr
  ↓
myptr
  ↓
  x
```

---

# 8. Evaluating `*myptr`

```c
k = *myptr;
```

Since:

```text
myptr → x[0]
```

we get:

```text
*myptr = 5
```

So:

```text
k = 5
```

---

# 9. Evaluating `(**ourptr) + 1`

This one is a classic GATE-style expression.

```c
k = (**ourptr) + 1;
```

Break it:

```text
ourptr
 ↓
myptr
 ↓
x[0]
```

Therefore:

```c
**ourptr
```

is:

```text
5
```

So:

```text
(**ourptr) + 1
= 5 + 1
= 6
```

---

# 10. `*(ourptr + 1)` — VERY DIFFERENT

Compare:

```c
(**ourptr) + 1
```

with:

```c
*(ourptr + 1)
```

They are **not the same**.

### First:

```c
(**ourptr) + 1
```

means:

> Get the value → add 1.

```text
5 + 1 = 6
```

### Second:

```c
*(ourptr + 1)
```

means:

> Move the pointer `ourptr` by one element → dereference.

This is **pointer arithmetic**, not value arithmetic.

🔥 Always watch the parentheses.

---

# 11. `*p2` vs `(*p2)`

Suppose:

```c
int **p2;
```

Then:

```c
*p2
```

means:

> Dereference `p2`.

And:

```c
**p2
```

means:

> Dereference twice.

But:

```c
(*p2)++
```

means:

> Increment the pointer stored in `p2`.

Whereas:

```c
(*(*p2))++
```

means:

> Increment the actual integer value.

This distinction appears in the later questions.

---

# 12. GATE 2008 Example

The lecture includes a GATE 2008 problem with:

```c
int f(int x, int *py, int **ppz)
```

The key operations are:

```c
**ppz += 1;
*py += 2;
x += 3;
```

The important thing to recognize is:

```text
x
→ local copy

py
→ pointer to original variable

ppz
→ pointer to a pointer
→ can reach another original variable
```

So changes through `*py` and `**ppz` can affect the original variables, while changing `x` only changes the function's local copy.

---

# 13. The Core Difference

This is worth memorizing:

```text
int x;

int *p = &x;
int **pp = &p;
```

### Access levels

```text
x       → value
p       → address of x
*p      → value of x

pp      → address of p
*pp     → p
**pp    → value of x
```

### Visual

```text
        address
pp ─────────→ p ─────────→ x
              address       value
                              100

*pp = p
**pp = x = 100
```

---

# 14. `++` Questions — Huge GATE Trap

Look at:

```c
(**pp)++;
```

This means:

> Increment the **value** being ultimately pointed to.

Example:

```text
x = 2

(**pp)++

x = 3
```

But:

```c
(*pp)++;
```

means:

> Increment the pointer `p`.

So the pointer moves to the next element if it points into an array.

The lecture's final question tests exactly this distinction.

---

# 15. Double Pointer with an Array

Suppose:

```c
int x[] = {2,4,6,8,10};

int *p = x;
int **pp = &p;
```

Initially:

```text
pp
 ↓
 p
 ↓
[2][4][6][8][10]
 ↑
```

Now:

```c
(*pp)++;
```

`*pp` is `p`.

Therefore:

```text
p++
```

Now:

```text
pp
 ↓
 p
 ↓
[2][4][6][8][10]
     ↑
```

So:

```c
*p
```

is now:

```text
4
```

The lecture's Question 6 tests this exact pattern.

---

# 16. The Two Expressions You MUST Distinguish

### `(*pp)++`

```text
Change pointer
```

```text
pp → p → [2][4][6]
     ↑
     moves →
```

### `(*(*pp))++`

```text
Change value
```

```text
pp → p → [2][4][6]
          ↑
        becomes 3
```

This is probably the single most useful distinction in these pages.

---

# 17. Quick Operator Table

Given:

```c
int x = 10;
int *p = &x;
int **pp = &p;
```

|Expression|Meaning|
|---|---|
|`x`|value of x|
|`&x`|address of x|
|`p`|address of x|
|`*p`|value of x|
|`&p`|address of p|
|`pp`|address of p|
|`*pp`|p|
|`**pp`|value of x|
|`(*pp)++`|increment p|
|`(**pp)++`|increment x|

---

# 🔥 Final GATE Mental Model

Don't think:

> "Two stars means something complicated."

Instead, **follow the arrows one at a time**.

```text
pp
 ↓       ← first *
p
 ↓       ← second *
x
```

Therefore:

```text
*pp  → p
**pp → x
```

And when you see:

```c
(*pp)++
```

ask:

> **What is inside the parentheses?**

`*pp` is `p`, so you're incrementing the **pointer**.

When you see:

```c
(**pp)++
```

`**pp` is `x`, so you're incrementing the **value**.

### The one-line rule:

> **Count the `*` from the outside inward, and use parentheses to determine whether you're modifying the pointer or the value.**

These pages then use exactly that idea in the array and GATE questions.