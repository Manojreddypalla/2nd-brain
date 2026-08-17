# C — `malloc()` & `free()` Short Notes

### Pages 49–78

These notes follow the lecture content and diagrams from **L-13(1), pages 49–78**.

---

## 1. Memory Areas

A C program's memory is shown as:

```text
┌─────────────┐
│    Stack    │ ← local variables / automatic allocation
├─────────────┤
│    Heap     │ ← dynamic allocation: malloc()
├─────────────┤
│    Static   │ ← global/static variables
└─────────────┘
```

Example:

```c
char *c = "goclasses";
int a;
static char s[] = "goclasses";
int x[3] = {1,2,3};
```

The lecture illustrates local variables on the **stack**, dynamically allocated memory on the **heap**, and global/static data in **static memory**.

---

# 2. `malloc()`

`malloc()` dynamically allocates memory from the **heap**.

### Syntax

```c
malloc(number_of_bytes);
```

Example:

```c
int *p = (int *)malloc(sizeof(int) * 10);
```

This requests enough heap memory for **10 integers**.

### Mental model

```text
Stack                         Heap

p ───────────────────────→ [ ][ ][ ][ ]...[ ]
                              10 integers
```

`p` itself is a pointer variable, while the memory it points to is dynamically allocated on the heap.

---

# 3. `malloc()` Takes Bytes

Very important:

```c
malloc(4);
```

means:

> Allocate **4 bytes**, NOT 4 integers.

If you want 4 integers:

```c
malloc(4 * sizeof(int));
```

For example, if `sizeof(int) = 4`:

```text
malloc(4)              → 4 bytes
malloc(4*sizeof(int))  → 16 bytes
```

The lecture explicitly demonstrates this distinction.

---

# 4. `malloc()` Does Not Know the Data Type

Example:

```c
malloc(40);
```

`malloc()` only knows:

> "Give me 40 bytes."

It doesn't know whether those bytes will contain:

- integers
    
- characters
    
- doubles
    
- structures
    
- etc.
    

That's why `malloc()` returns a **`void *`** according to the lecture.

---

# 5. Return Type of `malloc()`

```c
malloc(...)
```

returns:

```c
void *
```

So conceptually:

```text
malloc()
   ↓
void *
   ↓
address of allocated memory
```

The lecture notes that the `void *` can be used without an explicit cast in C.

Example:

```c
int *p = malloc(sizeof(int));
```

The allocated memory is later treated as an `int` because `p` is an `int *`.

---

# 6. `malloc()` Failure

If allocation fails:

```c
malloc(...)
```

returns:

```c
NULL
```

Therefore:

```c
if (p == NULL) {
    // allocation failed
}
```

The lecture explicitly highlights `NULL` on allocation failure.

---

# 7. Example: Allocate One Integer

```c
int *p = malloc(sizeof(int));

*p = 3;
```

Memory:

```text
Stack                    Heap

p ───────────────────→ [ 3 ]
                         int
```

`p` contains the address of the heap block.

`*p` accesses the value stored there.

---

# 8. Pointer Arithmetic on Allocated Memory

Suppose:

```c
int *p = malloc(10 * sizeof(int));
```

Then:

```c
p[0]
p[1]
p[2]
...
p[9]
```

can be used as the 10 integer elements.

```text
p
↓
[0][1][2][3][4][5][6][7][8][9]
```

The lecture demonstrates accessing allocated elements using expressions such as:

```c
p[4]
*(p+1)
*(p+5)
```

---

# 9. Different Types of Dynamic Memory

Example:

```c
int *p;
int *arr;
char *c_arr;

p = malloc(sizeof(int));
arr = malloc(sizeof(int) * 20);
c_arr = malloc(sizeof(char) * 10);
```

Conceptually:

```text
p      → 1 integer
arr    → 20 integers
c_arr  → 10 characters
```

Each pointer is stored in the stack, while the allocated blocks are on the heap.

---

# 10. Memory Diagram — Important GATE Concept

For:

```c
int *p = malloc(sizeof(int));
```

think:

```text
STACK                         HEAP

p ───────────────────────→   [ 6 ]
```

For:

```c
int *arr = malloc(sizeof(int) * 20);
```

```text
STACK                         HEAP

arr ─────────────────────→  [ ][ ][ ][ ] ... [ ]
                              20 integers
```

The **pointer variable** and the **memory it points to** are in different places.

---

# 11. Example Question: Where Is What Stored?

```c
void foo()
{
    int a = 9;
    int b[3] = {2,7,8};
    char *c = malloc(100);
}
```

According to the lecture:

```text
a → Stack
b → Stack
c → Stack
100-byte block → Heap
```

🔥 Important:

> `c` is on the stack, but the memory **pointed to by `c`** is on the heap.

This is a classic GATE trap.

---

# 12. Example

```c
int *parray = malloc(sizeof(int) * 10);
```

There are **two separate things**:

```text
parray
   ↓
pointer variable       → STACK

10-integer memory block → HEAP
```

The lecture's solution explicitly states this distinction.

---

# 13. `free()`

Once dynamically allocated memory is no longer required:

```c
free(ptr);
```

is used to release it.

Example:

```c
int *p = malloc(sizeof(int));

*p = 3;

free(p);
```

`free(p)` releases the heap memory associated with `p`.

---

# 14. What `free()` Does

Before:

```text
p ─────→ [ 3 ]
          HEAP
```

After:

```c
free(p);
```

the allocated memory is released.

Conceptually:

```text
p ─────→ [ released memory ]
```

### Important lecture point

`free(p)` releases the **allocated memory**.

It does **not** mean that the pointer variable `p` itself disappears.

---

# 15. Dangling Pointer

After:

```c
int *p = malloc(sizeof(int));

*p = 3;

free(p);
```

`p` still contains the old address, but that memory has been released.

So:

```text
p ─────→ released memory
```

This is a **dangling pointer**.

The lecture warns that using the pointer after `free()` can cause problems/crashes.

---

# 16. Don't Dereference After `free()`

Bad:

```c
free(p);

printf("%d", *p);
```

After `free(p)`, the program must not assume that `*p` is valid.

```text
free(p)
   ↓
memory no longer belongs to your program
   ↓
*p → invalid use
```

The lecture describes the memory after `free()` as no longer being something you can rely on.

---

# 17. Why `p = NULL` Is Useful

After freeing:

```c
free(p);
p = NULL;
```

Now:

```text
p → NULL
```

instead of keeping the old address.

This makes it clear that `p` no longer refers to a valid allocated block.

**Mental model:**

```text
Before free:

p ─────→ [memory]

After free:

p ─────→ old address   ❌ dangling

After p = NULL:

p ─────→ NULL          ✅
```

The lecture discusses the alternatives around the pointer after `free()`, including setting it to `NULL`.

---

# 18. `free()` Does NOT Free Stack Memory

`free()` is for dynamically allocated memory.

Correct:

```c
int *p = malloc(sizeof(int));
free(p);
```

Do **not** use `free()` for an ordinary local variable:

```c
int x;
free(&x);    // ❌
```

The lecture's entire `free()` discussion is in the context of dynamically allocated heap memory.

---

# 19. Important Example

```c
int *p = malloc(sizeof(int));

*p = 3;

int n = *p + 2;

free(p);
```

Execution:

```text
1. malloc()
      ↓
   heap memory created

2. p
      ↓
   heap block

3. *p = 3
      ↓
   heap block contains 3

4. n = *p + 2
      ↓
   n = 5

5. free(p)
      ↓
   heap block released
```

---

# ⭐ GATE Memory Pattern

Whenever you see:

```c
int *p = malloc(...);
```

immediately separate it into:

```text
                STACK             HEAP
                  │                 │
                  │                 │
                  p ─────────────→ memory
```

### Therefore:

|Object|Location|
|---|---|
|`p`|Stack|
|`*p` / allocated block|Heap|
|local `int x`|Stack|
|global/static variable|Static|
|`malloc()` memory|Heap|

The lecture uses this exact distinction in its memory questions and diagrams.

---

# 🔥 `malloc()` — GATE Quick Revision

```text
malloc(n)
→ requests n BYTES
→ memory comes from HEAP
→ returns void *
→ returns NULL if allocation fails
```

For `N` objects:

```c
malloc(N * sizeof(type))
```

Examples:

```c
malloc(sizeof(int))
malloc(10 * sizeof(int))
malloc(20 * sizeof(char))
malloc(5 * sizeof(double))
```

---

# 🔥 `free()` — GATE Quick Revision

```c
free(p);
```

→ releases dynamically allocated memory pointed to by `p`.

After:

```c
free(p);
```

`p` should **not be dereferenced**.

Safer pattern:

```c
free(p);
p = NULL;
```

---

## 🧠 The One Mental Model You Need

```text
                C PROGRAM MEMORY

        ┌─────────────────────────┐
        │          STACK          │
        │                         │
        │  int x;                 │
        │  int *p;                │
        │  int *arr;              │
        └──────────┬──────────────┘
                   │
                   │ pointers
                   ↓
        ┌─────────────────────────┐
        │           HEAP          │
        │                         │
        │ malloc() allocated      │
        │ memory                  │
        └─────────────────────────┘
```

**The key GATE distinction is:**

> **Pointer ≠ pointed memory.**

For:

```c
int *p = malloc(10 * sizeof(int));
```

`p` is the pointer variable; the 10-integer block is the dynamically allocated heap memory.