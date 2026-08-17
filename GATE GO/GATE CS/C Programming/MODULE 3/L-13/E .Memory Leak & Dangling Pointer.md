# C Pointers — Pages 79–114

## Memory Leak & Dangling Pointer — Short Notes

Based strictly on **L-13(1), pages 79–114**.

---

# 1. Memory Leak

### Definition

> **Memory leak = dynamically allocated memory that is not freed and can no longer be accessed.**

Example:

```c
void func()
{
    char *ch = malloc(10);
}
```

When `func()` ends:

```text
Stack
┌───────┐
│  ch   │  ← disappears
└───┬───┘
    │
    ↓
Heap
┌─────────────┐
│  10 bytes   │  ← still allocated
└─────────────┘
```

The pointer `ch` is gone, so there is **no way to access/free that heap memory**.

→ **Memory leak**.

---

# 2. Losing the Pointer Causes a Leak

Consider:

```c
int *x;

x = malloc(8);
x = malloc(4);
```

First:

```text
x ───→ [ 8 bytes ]
```

Then:

```text
x ───→ [ 4 bytes ]
```

The original 8-byte block has no pointer to it anymore.

```text
[ 8 bytes ]   ← LOST ❌
[ 4 bytes ] ← x
```

Therefore, this is a **memory leak**.

### Key rule

> **Before overwriting a pointer, free the memory it currently points to.**

```c
free(x);
x = malloc(4);
```

---

# 3. Returning `malloc()` Memory

This is **not automatically a memory leak**:

```c
int *func()
{
    int *p = malloc(8);
    return p;
}

int *x = func();
```

Why?

Because the heap memory remains allocated and its address is returned to `x`.

```text
func()

p ─────→ [8 bytes]
          ↑
          │
          return

main()

x ─────→ [8 bytes]
```

So `main()` can eventually:

```c
free(x);
```

The lecture explicitly contrasts this with losing the pointer inside the function.

---

# 4. Correct Way to Avoid the Leak

```c
void func()
{
    char *ch = malloc(10);

    // use memory

    free(ch);
}
```

Now:

```text
malloc()
   ↓
use memory
   ↓
free()
   ↓
memory released
```

→ **No memory leak.**

---

# 5. Important Difference

### Case 1 — No leak

```c
char *ch = malloc(10);
free(ch);
```

```text
malloc → free
```

✅ Memory released.

### Case 2 — Leak

```c
char *ch = malloc(10);
ch = 'a';
```

The pointer no longer refers to the allocated block.

```text
[10 bytes] ← LOST
```

❌ Memory leak.

---

# 6. Memory Leak ≠ Dangling Pointer

This distinction is **very important for GATE**.

### Memory Leak

Allocated memory exists, but **no valid pointer can reach it**.

```text
Heap:
[ allocated memory ]

        ❌ no pointer
```

### Dangling Pointer

A pointer still exists, but it points to **memory that has already been freed**.

```text
p ─────→ [freed memory]
```

So:

```text
Memory Leak:
memory exists + pointer lost

Dangling Pointer:
pointer exists + memory freed
```

---

# 7. Dangling Pointer

### Definition

> A dangling pointer points to memory that has already been freed.

Example:

```c
int *x = malloc(sizeof(int));

free(x);

*x = 5;
```

After:

```c
free(x);
```

the memory is no longer valid.

But `x` still contains the old address.

```text
x ─────→ [ FREED MEMORY ]
```

Therefore:

```c
*x = 5;
```

is an access through a **dangling pointer**.

---

# 8. `free()` Does Not Automatically Make Pointer `NULL`

This is a major point.

```c
int *p = malloc(sizeof(int));

free(p);
```

After `free(p)`:

```text
p ≠ NULL necessarily
```

Instead:

```text
p → old address
     ↓
   freed block
```

So `p` becomes a **dangling pointer**.

A common safe practice is:

```c
free(p);
p = NULL;
```

---

# 9. Returning Address of Local Variable

Example:

```c
char *func()
{
    char str[10] = {'H','e','l','l','o'};
    return str;
}
```

This is problematic because `str` is a **local array**.

Its lifetime ends when `func()` returns.

```text
func() stack frame
┌─────────────┐
│ str[10]     │
└─────────────┘
       ↑
       │
     return
       │
       ↓
function ends → stack memory no longer valid
```

The returned pointer therefore points to invalid memory.

→ **Dangling pointer.**

### GATE pattern

```c
int *func()
{
    int x = 10;
    return &x;
}
```

❌ Dangerous/invalid pointer after function returns.

---

# 10. Why `malloc()` Is Different

Compare:

### Local variable

```c
int *func()
{
    int x = 10;
    return &x;
}
```

`x` lives in the function's local storage.

After function returns:

```text
x → gone
```

Returned pointer becomes dangling.

### Heap allocation

```c
int *func()
{
    int *p = malloc(sizeof(int));
    *p = 10;
    return p;
}
```

The heap block remains allocated after `func()` returns.

```text
p ─────→ [10]
          HEAP
```

The caller can later `free()` it.

---

# 11. Memory Leak Example

```c
void func()
{
    char *ch = malloc(10);

    free(ch);
}
```

No leak.

But:

```c
void func()
{
    char *ch = malloc(10);
    ch = 'a';
}
```

The original 10-byte block becomes unreachable.

```text
Before:
ch ─────→ [10 bytes]

After ch = 'a':
ch
 ↓
'a'

[10 bytes] ← LOST
```

→ **Memory leak**.

---

# 12. Multiple `malloc()` Calls

Example:

```c
int *x;

x = malloc(8);
x = malloc(4);
```

Problem:

```text
First allocation
x ─────→ [8 bytes]

Second allocation
x ─────→ [4 bytes]

[8 bytes] ← inaccessible
```

The first block leaked.

Correct:

```c
x = malloc(8);

free(x);

x = malloc(4);
```

---

# 13. `free()` Twice

Once memory has been freed, don't free the same allocation again.

Conceptually:

```c
free(p);
free(p);     // ❌
```

After the first `free`, `p` no longer points to a valid allocated block.

The lecture's questions repeatedly test what happens when pointers are reused after `free()`.

---

# 14. Aliasing + `free()`

Consider:

```c
int *p, *q;

p = malloc(8);
q = p;

free(p);
```

Before `free`:

```text
       ┌───────────┐
p ────→│           │
       │  8 bytes  │
q ────→│           │
       └───────────┘
```

After:

```c
free(p);
```

both pointers still contain the old address:

```text
p ────→ [freed memory]
q ────→ [freed memory]
```

So **both `p` and `q` are dangling pointers**.

The lecture tests this exact pattern.

---

# 15. Very Important Trap

```c
p = malloc(8);
q = p;

free(p);

q = malloc(8);
```

The second `malloc()` does **not** magically repair `p`.

```text
p ─────→ old freed block ❌

q ─────→ new block      ✅
```

`p` remains dangling.

---

# 16. Loop + `malloc()` → Memory Leak

Example from the lecture:

```c
int *x;

do {
    x = malloc(sizeof(int));

    scanf("%d", x);

} while (*x != 0);

free(x);
```

Problem:

```text
Iteration 1:
x → block 1

Iteration 2:
x → block 2

Iteration 3:
x → block 3

...
```

Every new assignment to `x` loses the previous block.

Only the **last** block is freed.

Therefore previous allocations leak.

### Pattern to recognize

```text
malloc()
 ↓
pointer overwritten
 ↓
old block unreachable
 ↓
memory leak
```

---

# 17. Dynamic Memory + Copying

The lecture includes a question where a `copy()` function dynamically allocates memory and returns it. The important thing being tested is **who owns the allocated memory and who must free it**.

Mental model:

```text
copy()
  ↓
malloc()
  ↓
returns pointer
  ↓
caller receives pointer
  ↓
caller must eventually free()
```

If the returned pointer is lost → **memory leak**.

If the returned pointer refers to freed memory → **dangling pointer**.

---

# 18. Question: Which Allocation Does NOT Leak?

The lecture gives independent cases involving:

```c
ptrA
ptrB
malloc()
free()
```

The key technique is:

> **Track every allocation separately.**

Example:

```c
ptrA = malloc(4);
ptrB = malloc(4);

free(ptrA);
free(ptrB);
```

Both allocations are released.

But:

```c
ptrA = malloc(4);
ptrB = malloc(4);

ptrB = ptrA;

free(ptrA);
```

The original allocation associated with `ptrB` is now unreachable.

→ **Memory leak**.

---

# 19. Overwriting a Pointer

This is one of the most important patterns:

```c
p = malloc(...);
p = malloc(...);
```

Always ask:

> **What happened to the first allocation?**

If it wasn't freed first:

```text
first block → LOST → MEMORY LEAK
```

Correct:

```c
p = malloc(...);

free(p);

p = malloc(...);
```

---

# 20. Question: Local Pointer + Local Variable

Example:

```c
int *q;

void foo()
{
    int a;
    q = &a;
}

int main()
{
    foo();
    *q = 5;
}
```

After `foo()` returns:

```text
a
↓
function's local memory gone
```

But:

```text
q
↓
old address of a
```

Therefore `q` becomes a **dangling pointer**.

The lecture tests this exact lifetime issue.

---

# 21. Question: Returning `malloc()` Pointer

Example:

```c
int *g()
{
    int *px;

    px = malloc(sizeof(int));
    *px = 10;

    return px;
}
```

This is fundamentally different from:

```c
int *g()
{
    int x = 10;
    return &x;
}
```

### First

```text
malloc → HEAP
```

Memory survives function return.

### Second

```text
x → local storage
```

Lifetime ends when function returns.

The lecture's final GATE-style question contrasts these pointer-return patterns.

---

# 22. GATE 2017 Pattern

The lecture includes a **GATE 2017** question involving:

```c
int *assignval(int *x, int val)
{
    *x = val;
    return x;
}
```

and dynamic allocation with checks against `NULL`.

### Pattern to understand

When you see:

```c
x = malloc(...);

if (x == NULL)
    return;

...

free(x);
```

trace:

```text
malloc
 ↓
check NULL
 ↓
use memory
 ↓
free
```

The important issue is whether every allocated block is eventually freed and whether any pointer is used after its target becomes invalid.

---

# 23. Common Dynamic-Memory Errors

|Situation|Problem|
|---|---|
|`malloc()` but never `free()`|**Memory leak**|
|Pointer overwritten before `free()`|**Memory leak**|
|`free(p); *p = ...`|**Dangling pointer / use-after-free**|
|Return address of local variable|**Dangling pointer**|
|Two pointers point to same block, then block freed|Both become dangling|
|`free()` then use pointer|Invalid access|
|`malloc()` → `free()` properly|✅ Correct|
|`malloc()` → return pointer → caller frees|✅ Correct|

---

# ⭐ Most Important Mental Model

Don't think of `malloc` as "creating a variable."

Think:

```text
malloc()
   ↓
creates a HEAP BLOCK

pointer
   ↓
stores its address
```

So always track **two separate things**:

```text
        POINTER                    MEMORY

        p ──────────────────────→ [ allocated block ]
        ↑                           ↑
      variable                  actual memory
```

Then every problem becomes:

### Case A — Pointer disappears

```text
p ─────→ [block]

p disappears
       ↓
[block] ← unreachable

→ MEMORY LEAK
```

### Case B — Memory disappears

```text
p ─────→ [block]

free(p)
       ↓
p ─────→ [freed block]

→ DANGLING POINTER
```

### Case C — Pointer overwritten

```text
p ─────→ [block A]

p = malloc(...)
       ↓

p ─────→ [block B]

block A ← unreachable

→ MEMORY LEAK
```

### Case D — Local variable dies

```text
function:
x exists
p → x

function returns

x disappears
p → dead memory

→ DANGLING POINTER
```

---

# 🔥 Final GATE Revision — Pages 79–114

```text
MEMORY LEAK
= allocated memory that becomes unreachable
```

```text
DANGLING POINTER
= pointer referring to memory that is no longer valid
```

### Remember the direction:

```text
Memory Leak:
MEMORY survives
POINTER is lost

Dangling Pointer:
POINTER survives
MEMORY is gone
```

### The 4 patterns GATE loves

```c
p = malloc(...);
p = malloc(...);
```

→ **leak**

```c
free(p);
*p = 5;
```

→ **dangling/use-after-free**

```c
int x;
return &x;
```

→ **dangling pointer**

```c
p = malloc(...);
...
free(p);
```

→ correct, assuming no later invalid use.

The lecture's final pages reinforce these exact patterns through multiple questions, including GATE-style questions on leaks, dangling pointers, and returning pointers.

## 🧠 One-line rule

> **For every `malloc()`, mentally create a heap block; then track which pointer owns it, when that pointer changes, and where/when `free()` happens.**

That single habit solves most of the questions in **pages 79–114**.