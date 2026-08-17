# L-13 — C Pointers + Dynamic Memory

### Short Notes / Cheat Sheet

This lecture mainly covers **complex declarations → pointer-to-function → void pointers → pointer/array interpretation → malloc/free → memory leaks & dangling pointers**.

---

## 1. Complex Declarations — Golden Rule

### Translate symbols into English

|Symbol|Meaning|
|---|---|
|`*`|pointer to|
|`[]`|array of|
|`()`|function returning|

### How to decode

1. **Find identifier**
    
2. Look **right**
    
3. Then move **left**
    
4. Resolve parentheses first when needed.
    

Example:

```c
int *p[10];
```

Start at `p`:

> `p` is an **array of 10** → of **pointers** → to `int`

✅ `p` = array of 10 integer pointers.

```c
int (*p)[10];
```

`()` forces `*p` together:

> `p` is a **pointer to an array of 10 int**

```c
int *f();
```

> `f` is a **function returning pointer to int**

```c
int (*pf)();
```

> `pf` is a **pointer to a function returning int**

```c
int (*pf)(int);
```

> `pf` is a **pointer to a function taking int and returning int**

---

# 2. Pointer to Function

```c
int my_fun(int x) {
    return x + 1;
}

int (*p)(int) = my_fun;
```

Now:

```c
p(2)
```

is equivalent to:

```c
my_fun(2)
```

### GATE focus

Understand the **declaration**, not necessarily implementation. The lecture explicitly marks function-pointer implementation as **not required for GATE**.

---

# 3. Void Pointer `void *`

### Core idea

`void *` = **generic pointer**

It can store address of different types:

```c
int a = 7;
void *p;

p = &a;
```

But:

```c
*p
```

❌ invalid directly.

Because compiler doesn't know:

> “How many bytes should I read, and what type is there?”

So typecast first:

```c
*(int *)p
```

### Rule

> **Before dereferencing `void *`, typecast it.**

```c
int a = 7;
void *p = &a;

printf("%d", *(int *)p);
```

### Important

```c
sizeof(void *)
```

is the **size of a pointer**, not the size of the object it points to.

So on a typical 64-bit system:

```text
sizeof(int *)   = 8
sizeof(char *)  = 8
sizeof(void *)  = 8
```

Pointer size depends on architecture.

---

# 4. Pointer + Array Mental Model

For:

```c
int (*t)[10];
```

`t` points to an **array of 10 integers**.

For:

```c
int *t[10];
```

`t` itself is an **array containing 10 integer pointers**.

### Never confuse these

```text
int *t[10]     → ARRAY of POINTERS
int (*t)[10]   → POINTER to ARRAY
```

Parentheses completely change the meaning.

---

# 5. 2D Array Pointer Thinking

Given:

```c
double points[3][4];
```

Memory:

```text
points
 ↓
[1  2  3  4]
[5  6  7  8]
[9 10 11 12]
```

```c
double (*p1)[4] = points;
```

`p1` → pointer to an **array of 4 doubles**.

Therefore:

```c
p1 + 1
```

moves by **one entire row**, i.e. `4 doubles`.

### Important

```c
double *p2 = points[1];
```

`p2` → first element of row 2.

```c
double *p3 = &points[1][2];
```

`p3` → third element of second row.

The lecture's diagram questions heavily test exactly this interpretation.

---

# 6. `malloc()`

### Purpose

Dynamically allocate memory from the **heap**.

```c
int *p = malloc(sizeof(int));
```

or for 10 integers:

```c
int *p = malloc(10 * sizeof(int));
```

Better pattern:

```c
int *p = malloc(10 * sizeof(*p));
```

### Mental model

```text
Stack                         Heap

p ───────────────────────→ [ ][ ][ ][ ][ ]
                              dynamically allocated
```

`p` itself is a local pointer variable → stack.

Memory returned by `malloc()` → heap.

---

# 7. `malloc()` Important Facts

```c
malloc(number_of_bytes)
```

It allocates **bytes**, not number of elements.

Example:

```c
malloc(40)
```

means:

> Allocate 40 bytes.

It does NOT mean 40 integers.

For 10 integers:

```c
malloc(10 * sizeof(int))
```

### Return type

`malloc()` returns:

```c
void *
```

So it can be assigned to different pointer types.

If allocation fails:

```c
malloc(...) == NULL
```

---

# 8. `malloc()` Does NOT Initialize Memory

```c
int *p = malloc(10 * sizeof(int));
```

The allocated memory contains **indeterminate/garbage values** until you initialize it.

```c
p[0] = 10;
p[1] = 20;
```

---

# 9. Stack vs Heap

### Stack

Usually contains:

- local variables
    
- local pointer variables
    
- function call information
    

### Heap

Contains:

- memory dynamically allocated using `malloc()`
    

Example:

```c
int *p = malloc(sizeof(int));
```

```text
STACK                  HEAP

p = 1000  ─────────→   [  ?  ]
```

The **pointer `p`** is on stack.

The **allocated integer** is on heap.

This distinction is directly tested in the lecture.

---

# 10. `free()`

Release dynamically allocated heap memory:

```c
int *p = malloc(sizeof(int));

*p = 5;

free(p);
```

### Golden rule

> Every dynamically allocated chunk should eventually be freed.

---

# 11. What Happens After `free(p)`?

```c
free(p);
```

The heap memory is no longer yours.

But the pointer variable `p` still contains the **old address**.

```text
p ─────X────→ [freed memory]
```

So:

```c
*p = 10;
```

after `free(p)` is **invalid**.

---

# 12. Dangling Pointer

### Definition

A pointer that points to memory that has already been freed.

```c
int *p = malloc(sizeof(int));

free(p);

*p = 5;       // WRONG
```

`p` is now a **dangling pointer**.

Also occurs when a pointer refers to a local variable that has gone out of scope:

```c
int *p;

void fun() {
    int x;
    p = &x;
}
```

After `fun()` returns:

```text
x's lifetime ended
        ↑
        |
        p
```

`p` is dangling.

---

# 13. Best Practice After `free`

Use:

```c
free(p);
p = NULL;
```

Now:

```c
p == NULL
```

and accidental dereference is easier to detect.

### But remember

Setting pointer to `NULL` **does not free memory**.

You must:

```c
free(p);
p = NULL;
```

---

# 14. Memory Leak

### Definition

Allocated memory that is no longer accessible and therefore cannot be freed.

Example:

```c
void func() {
    char *p = malloc(10);
}
```

When `func()` ends:

```text
p disappears
   ↓
heap memory still exists
   ↓
no pointer can reach it
   ↓
MEMORY LEAK
```

---

# 15. Classic Memory Leak

```c
int *p = malloc(sizeof(int));

*p = 5;

p = malloc(sizeof(int));
```

❌ First allocation is leaked.

Why?

```text
Before:

p ─────→ Block A

After p = malloc():

p ─────→ Block B

Block A
  ↑
  no pointer
```

There is no way to `free(Block A)` anymore.

### Correct

```c
free(p);
p = malloc(sizeof(int));
```

---

# 16. Memory Leak ≠ Dangling Pointer

|Situation|Meaning|
|---|---|
|Allocated memory becomes unreachable|**Memory leak**|
|Pointer still points to freed memory|**Dangling pointer**|

### Easy visualization

```text
MEMORY LEAK:

pointer ❌ → [allocated memory]
             ↑
             unreachable


DANGLING POINTER:

pointer ─────→ [FREED memory]
```

The lecture repeatedly tests this distinction.

---

# 17. `free()` Does NOT Mean Pointer Becomes NULL

```c
int *p = malloc(4);

free(p);
```

After this:

```text
p ≠ necessarily NULL
```

It still contains the old address, but using it is invalid.

Correct safe pattern:

```c
free(p);
p = NULL;
```

---

# 18. Pointer Aliasing + `free()`

Suppose:

```c
int *p = malloc(8);
int *q = p;
```

Now:

```text
p ──┐
    ├──→ same heap block
q ──┘
```

If:

```c
free(p);
```

then **both pointers refer to freed memory**.

So:

```c
*q = 5;
```

❌ dangling-pointer access.

The lecture uses these aliasing cases in its questions.

---

# 19. `free()` Twice

```c
int *p = malloc(4);

free(p);
free(p);
```

❌ Invalid — attempting to free the same allocation twice.

Likewise:

```c
free(p);
*p = 5;
```

❌ use-after-free / dangling pointer access.

---

# 20. Returning Address of Local Variable

Dangerous:

```c
int *fun() {
    int x = 10;
    return &x;
}
```

Why?

`x` belongs to the function's stack frame.

When function returns:

```text
fun() ends
   ↓
x's lifetime ends
   ↓
returned pointer becomes dangling
```

This is one of the key pointer-lifetime traps in the lecture.

---

# 21. Returning Dynamically Allocated Memory

This is okay:

```c
int *fun() {
    int *p = malloc(sizeof(int));
    *p = 10;
    return p;
}
```

Why?

The pointer variable `p` is local, **but the heap object survives after the function returns**.

Caller must eventually:

```c
free(p);
```

---

# 22. Scope vs Lifetime — Important

Don't confuse:

```text
Pointer variable lifetime
        ↓
        stack/local

Pointed object lifetime
        ↓
        heap/dynamic
```

Example:

```c
int *p = malloc(sizeof(int));
```

- `p` → local variable
    
- allocated `int` → heap object
    

When the function ends, `p` disappears, but heap object **doesn't automatically disappear**.

That is why memory leaks happen.

---

# 23. Dynamic Allocation Question Pattern

Whenever you see:

```c
int *p = malloc(...);
```

ask:

### Q1. Where is `p`?

➡️ Usually stack if local.

### Q2. Where is allocated memory?

➡️ Heap.

### Q3. Who owns it?

➡️ Program must eventually `free()` it.

### Q4. Is there another pointer to it?

➡️ Check aliasing.

### Q5. Has it already been freed?

➡️ If yes → dangling pointer.

### Q6. Was original address overwritten?

➡️ If yes → possible memory leak.

---

# 24. GATE Pointer Declaration Cheat Sheet

```c
int *p;
```

→ `p` is pointer to int

```c
int **p;
```

→ `p` is pointer to pointer to int

```c
int p[10];
```

→ `p` is array of 10 int

```c
int *p[10];
```

→ `p` is array of 10 pointers to int

```c
int (*p)[10];
```

→ `p` is pointer to array of 10 int

```c
int f();
```

→ `f` is function returning int

```c
int *f();
```

→ `f` is function returning pointer to int

```c
int (*f)();
```

→ `f` is pointer to function returning int

```c
int (*f)(int);
```

→ `f` is pointer to function taking int and returning int

---

# 25. GATE Pointer Arithmetic — Core Rule

If:

```c
int *p;
```

then:

```c
p + 1
```

moves by:

```text
sizeof(int)
```

If:

```c
double *p;
```

then:

```c
p + 1
```

moves by:

```text
sizeof(double)
```

### Therefore

> Pointer arithmetic is scaled according to the **pointed-to type**.

For:

```c
double (*p)[4];
```

`p + 1` moves by:

```text
4 × sizeof(double)
```

because `p` points to an **array of 4 doubles**.

---

# 26. Ultimate Mental Model

Whenever you see complicated pointer code, don't try to memorize it.

Draw:

```text
VARIABLE
   |
   | address
   ↓
OBJECT
   |
   | maybe another pointer
   ↓
OBJECT
```

Then ask:

> **What does this variable contain?**

> **What type does it point to?**

> **How far does `+1` move?**

> **Is the target still alive?**

That solves most of the lecture's GATE questions.

---

# ⚡ FINAL 1-MINUTE REVISION

```text
*       → pointer to
[]      → array of
()      → function

int *p[10]       → array of 10 int pointers
int (*p)[10]     → pointer to array of 10 int

int *f()         → function returning int*
int (*f)()       → pointer to function returning int

void *            → generic pointer
void *            → cannot directly dereference
typecast first    → *(int *)p

malloc(n)         → allocate n BYTES on heap
malloc            → returns void *
failure           → NULL
malloc memory     → uninitialized

local pointer     → usually stack
malloc block      → heap

free(p)           → release heap block
free(p)            ≠ p = NULL

freed memory + pointer still exists
                 → dangling pointer

allocated memory becomes unreachable
                 → memory leak

free → then use
                 → dangling/use-after-free

overwrite only pointer to allocation
                 → memory leak

return address of local variable
                 → dangling pointer

return malloc'ed memory
                 → valid, caller must free
```

The lecture's later GATE examples specifically revolve around **memory leaks, dangling pointers, aliasing, `malloc`, and pointer lifetime**, so these are the highest-priority things to drill after making the notes.