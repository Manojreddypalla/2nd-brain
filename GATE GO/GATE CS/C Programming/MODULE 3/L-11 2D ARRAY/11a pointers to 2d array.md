Ahh, yes — **#10: “Pointer to a 2D array”**. This is confusing because `*` and `[]` together look scary. Let's slow it way down.

The key is to understand **what `p` is supposed to point to**.

---

# 1. Start with a normal 1D array

```c
int a[5] = {10,20,30,40,50};
```

Memory:

```text
a
↓
+----+----+----+----+----+
| 10 | 20 | 30 | 40 | 50 |
+----+----+----+----+----+
```

If we write:

```c
int *p = a;
```

then:

```text
p
↓
10
```

So `p` is:

> **pointer to an integer**

That's why its type is:

```c
int *
```

---

# 2. Now make it 2D

```c
int a[3][4];
```

Think of it as:

```text
             a
             ↓
       ┌───────────────┐
row 0  │  1  2  3  4   │
       ├───────────────┤
row 1  │  5  6  7  8   │
       ├───────────────┤
row 2  │  9 10 11 12   │
       └───────────────┘
```

Now here's the important question:

### What does `a` point to?

It points to the **first row**.

And what is the first row?

```c
a[0]
```

which is:

```c
int [4]
```

So:

```text
a
↓
[ int int int int ]
```

Therefore, if we want a pointer that points to a row of 4 integers:

```c
int (*p)[4];
```

---

# 3. Read `int (*p)[4]` from the inside

Don't read it left-to-right.

Look at:

```c
int (*p)[4];
```

First:

```c
(*p)
```

means:

> `p` is a pointer.

Then:

```c
(*p)[4]
```

means:

> `p` is a pointer to an array of 4 things.

And `int` tells us those 4 things are integers.

Therefore:

```c
int (*p)[4];
```

means:

> **p is a pointer to an array of 4 integers.**

That's it.

---

# 4. Why do we need this?

Consider:

```c
int a[3][4] = {
    {1,2,3,4},
    {5,6,7,8},
    {9,10,11,12}
};
```

Now:

```c
int (*p)[4] = a;
```

Picture it:

```text
p
↓
┌───────────────┐
│ 1  2  3  4    │  ← row 0
└───────────────┘
┌───────────────┐
│ 5  6  7  8    │  ← row 1
└───────────────┘
┌───────────────┐
│ 9 10 11 12    │  ← row 2
└───────────────┘
```

`p` points to **row 0**.

---

# 5. Now the important part: `p + 1`

Because:

```c
p
```

points to a **whole row of 4 integers**, `p + 1` means:

> Move to the next row.

So:

```text
p       → row 0
p + 1   → row 1
p + 2   → row 2
```

Not:

```text
p + 1 → next integer
```

That's the whole reason for writing:

```c
int (*p)[4]
```

instead of:

```c
int *p
```

---

# 6. Compare these two

### `int *p`

```c
int *p;
```

means:

```text
p → integer
```

So:

```c
p + 1
```

moves to the **next integer**.

---

### `int (*p)[4]`

```c
int (*p)[4];
```

means:

```text
p → array of 4 integers
```

So:

```c
p + 1
```

moves to the **next row**.

---

# 7. Visualize it

Suppose each `int = 4 bytes`.

```text
int a[3][4]
```

Memory:

```text
1000  1004  1008  1012   ← row 0
+-----+-----+-----+-----+
|  1  |  2  |  3  |  4  |
+-----+-----+-----+-----+

1016  1020  1024  1028   ← row 1
+-----+-----+-----+-----+
|  5  |  6  |  7  |  8  |
+-----+-----+-----+-----+

1032  1036  1040  1044   ← row 2
+-----+-----+-----+-----+
|  9  | 10  | 11  | 12  |
+-----+-----+-----+-----+
```

If:

```c
int (*p)[4] = a;
```

then:

```text
p = 1000
```

Now:

```c
p + 1
```

does **not** become `1004`.

It becomes:

```text
1016
```

because one row =

```text
4 integers
= 4 × 4 bytes
= 16 bytes
```

---

# 8. This connects directly to `a[i][j]`

Now look at:

```c
a[1][2]
```

We can think:

```text
a
 ↓
row 0
```

Move one row:

```c
a + 1
```

Now we're at row 1.

Dereference:

```c
*(a + 1)
```

gives row 1.

Then move 2 integers:

```c
*(a + 1) + 2
```

Then dereference:

```c
*(*(a + 1) + 2)
```

Result:

```text
7
```

So:

```c
a[1][2]
```

is equivalent to:

```c
*(*(a+1)+2)
```

---

# 🔥 The one thing I want you to remember

Don't memorize:

```c
int (*p)[4]
```

as some weird syntax.

Think:

```text
What does p point to?
        ↓
A ROW
        ↓
How many things are in that row?
        ↓
4 integers
```

Therefore:

```c
int (*p)[4];
```

means:

> **p points to a row containing 4 integers.**

And consequently:

```text
p + 1 → next row
p + 2 → next-next row
```

That's the core idea behind the pointer in **page 10 onward** of your PDF.

If you understand this, the later `*(*(a+i)+j)` expressions become **much easier**.