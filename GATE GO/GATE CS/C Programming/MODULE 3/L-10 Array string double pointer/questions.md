Yep. From the **double-pointer section of the PDF**, there are **6 practice questions (Q1–Q6)**, on pages 70–78. I’ll list them **without solving first**, so you can attempt them.

# Double Pointer — All PDF Questions

## Q1

```c
int x, y, *p1, **p2;

x = 100;
y = 200;

p1 = &x;
p2 = &p1;

printf("%d", **p2);

*p2 = &y;

printf("%d", **p2);
```

### Question:

What will be the output?

**Think carefully about what `*p2` represents before and after:**

```c
*p2 = &y;
```

---

# Q2

Given:

```c
int k, x[3] = {5, 7, 9};

int *myptr, **ourptr;

myptr = x;
ourptr = &myptr;
```

Find the value of `k` in each case:

### (a)

```c
k = *myptr;
```

### (b)

```c
k = (**ourptr) + 1;
```

### (c)

```c
k = *(**ourptr + 1);
```

The PDF specifically asks you to determine these values.

---

# Q3 — GATE 2008

```c
int f(int x, int *py, int **ppz)
{
    int y, z;

    **ppz += 1;
    z = **ppz;

    *py += 2;
    y = *py;

    x += 3;

    return x + y + z;
}

void main()
{
    int c, *b, **a;

    c = 4;
    b = &c;
    a = &b;

    printf("%d", f(c, b, a));
}
```

### Question:

What is the output?

**GATE 2008**

The key is to carefully track:

```text
c
↓
b
↓
a
```

and distinguish:

```c
x
*py
**ppz
```

---

# Q4

Given:

```c
long A[3] = {1, 2, 3};

long *p;
long **q;

p = A;
p++;

q = &p;
p++;

(*p) = (**q) + 2;
```

### Question:

What is the content of array `A` after executing the code?

Options shown in the PDF:

```text
1. 123
2. 124
3. 126
4. 143
5. 146
6. None of the above
```

---

# Q5

```c
void inc_ptr(int **h)
{
    *h = *h + 1;
}

int A[3] = {50, 60, 70};

int *q = A;

inc_ptr(&q);

printf("%d", *q);
```

### Question:

What is printed?

Think:

```text
q → A[0]
```

then:

```c
inc_ptr(&q);
```

What happens to `q`?

---

# Q6

```c
int x[] = {2, 4, 6, 8, 10};

int *p = x;

int **pp = &p;

(*pp)++;

(*(*pp))++;

printf("%d\n", *p);
```

### Question:

What is the output?

Options:

```text
A: 2
B: 3
C: 4
D: 5
```

---

# 🧠 What these 6 questions are testing

Don't treat them as six unrelated questions. They're testing **three patterns**:

### Pattern 1 — Follow the pointer chain

```text
pp → p → x
```

Therefore:

```text
*pp  → p
**pp → x
```

### Pattern 2 — Pointer movement

```c
p++;
```

moves the pointer to the next element.

### Pattern 3 — Value modification

```c
(*p)++;
```

changes the value being pointed to.

And the nastiest combination:

```c
(*pp)++;
```

→ **moves `p`**

while:

```c
(*(*pp))++;
```

→ **changes the value pointed to by `p`**.

That's basically the entire skill these six questions are trying to build.

If you're practicing for GATE, **attempt these 6 first without looking at the answers**.