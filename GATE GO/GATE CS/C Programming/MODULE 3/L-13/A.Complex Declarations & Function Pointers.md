Yes — you mean **pages 1–16 of L-13**, not just a few selected concepts. I’ll keep the notes **short enough for Obsidian, but deep enough for GATE**, and preserve what the lecture teaches. Pages 1–16 cover **Complex Declarations + Function Pointers**.

# C Programming — Complex Declarations & Function Pointers

## 1. Complex Declarations

C declarations become easier if we translate symbols into English:

|Symbol|Read as|
|---|---|
|`*`|pointer to|
|`[]`|array of|
|`()`|function returning|

### Basic examples

```c
int *p;
```

> `p` is a **pointer to int**.

```c
int p[10];
```

> `p` is an **array of 10 integers**.

```c
int *p[10];
```

> `p` is an **array of 10 pointers to int**.

### Core idea

The **identifier is the starting point**.  
Then read the declaration around it according to C's precedence rules.

---

# 2. Declaration Reading Algorithm

For complicated declarations, use this order.

### Step 1 — Find the identifier

Example:

```c
int *p[10];
```

Identifier:

```text
p
```

Start with:

> `p is ...`

### Step 2 — Look immediately to the right

```text
p[10]
```

`[]` means:

> `p is an array of 10 ...`

### Step 3 — Continue outward

Now encounter:

```text
*p
```

`*` means:

> `p is an array of 10 pointers to ...`

Finally:

```text
int
```

Therefore:

> `p` is an array of 10 pointers to int.

This **identifier → right → left → outward** technique is the main method for solving complex declarations.

---

# 3. Why Parentheses Matter

Consider:

```c
int *p[5];
```

versus:

```c
int (*p)[5];
```

They are **completely different**.

### `int *p[5]`

`[]` binds to `p` first:

```text
p → array of 5 → pointers to int
```

So:

```text
p = array[5] of int*
```

### `int (*p)[5]`

Parentheses force `*p` to bind first:

```text
p → pointer to → array[5] of int
```

So:

```text
p = pointer to array[5] of int
```

### GATE Trap ⚠️

```text
int *p[5]     → array of 5 int pointers
int (*p)[5]   → pointer to array of 5 ints
```

**Parentheses completely change the declaration.**

---

# 4. Function Returning Pointer

```c
int *f();
```

Read:

> `f` is a **function returning pointer to int**.

Important distinction:

```c
int *f();
```

means:

```text
function → returns int*
```

It does **not** mean:

```text
pointer → function
```

The lecture uses this exact distinction.

---

# 5. Pointer to Function

```c
int (*pf)();
```

Start at `pf`:

```text
pf
 ↓
(*pf)
```

Because `()` is inside the parentheses:

> `pf` is a **pointer to a function returning int**.

Compare:

```c
int *f();
```

> function returning `int *`

with:

```c
int (*f)();
```

> pointer to function returning `int`

### 🔥 GATE Trap

```text
int *f()      → f is FUNCTION
int (*f)()    → f is POINTER
```

The parentheses decide which one it is.

---

# 6. Function Pointer With Arguments

```c
int (*pf)(int);
```

Read:

> `pf` is a **pointer to a function taking an int argument and returning int**.

Visualize:

```text
pf
 ↓
[ address of function ]
          |
          ↓
      function(int)
          |
          ↓
         int
```

So the declaration contains three pieces:

```text
(*pf)       → pointer
(int)       → function takes int
int         → returns int
```

---

# 7. GATE PYQ Pattern

Consider:

```c
int (*f)(int *);
```

Start with `f`:

```text
f is
```

`(*f)`:

```text
f is a pointer
```

`(int *)`:

```text
f is a pointer to a function taking an int pointer
```

`int`:

```text
f is a pointer to a function taking int*
and returning int
```

### Final

> `f` is a **pointer to a function that takes an integer pointer as argument and returns an integer**.

---

# 8. Complex Declaration Example

```c
int *(*fp1)(int)[10];
```

Read from `fp1`.

### Step 1

```text
fp1
```

### Step 2

```text
(*fp1)
```

Therefore:

> `fp1` is a pointer...

### Step 3

```text
(*fp1)(int)
```

Therefore:

> `fp1` is a pointer to a function taking `int`...

### Step 4

```text
int * ...
```

The function returns:

```text
int *
```

### Step 5

```text
[10]
```

Therefore the lecture interprets it as:

> `fp1` is a pointer to a function taking an `int` and returning a pointer to an array of 10 pointers to `int`.

### ⚠️ Important

For very complicated declarations, **never read left-to-right normally**.

Always:

```text
IDENTIFIER
   ↓
RIGHT
   ↓
LEFT
   ↓
OUTWARD
```

---

# 9. Array of Pointers to Arrays

Consider:

```c
int (*apa[5])[10];
```

Start at:

```text
apa
```

`[5]`:

> `apa` is an array of 5 ...

`*`:

> ... pointers to ...

`[10]`:

> ... arrays of 10 ...

`int`:

> ... integers.

### Final

> `apa` is an **array of 5 pointers to arrays of 10 int**.

Visual:

```text
apa
│
├── apa[0] ──→ [10 ints]
├── apa[1] ──→ [10 ints]
├── apa[2] ──→ [10 ints]
├── apa[3] ──→ [10 ints]
└── apa[4] ──→ [10 ints]
```

---

# 10. Array of Pointers to Arrays — No Fixed Size

```c
int (*apa[][]);
```

The lecture describes this as:

> an array of pointers to arrays of int.

The important pattern is recognizing:

```text
(*apa)
```

as the pointer portion and the array brackets around it as the array dimensions.

---

# 11. Function Pointer — Basic Concept

A function also has an address in memory.

Example:

```c
int my_fun(int x)
{
    return x + 1;
}
```

A function pointer can store the address of `my_fun`.

Conceptually:

```text
my_fun
   ↓
function's address
```

The lecture notes that `my_fun` can be treated as a pointer to the function.

---

# 12. Declaring a Function Pointer

For:

```c
int my_fun(int x)
{
    return x + 1;
}
```

Declare:

```c
int (*p)(int);
```

Then:

```c
p = my_fun;
```

Now:

```text
p
 ↓
my_fun()
```

Calling:

```c
p(2)
```

invokes:

```c
my_fun(2)
```

Result:

```text
3
```

The lecture demonstrates this function-pointer mechanism in pages 15–16.

---

# 13. Function Pointer vs Function Returning Pointer

This is one of the **most important declaration traps**.

### Function returning pointer

```c
int *f(int);
```

Meaning:

```text
f
 ↓
function taking int
 ↓
returns int*
```

### Pointer to function

```c
int (*f)(int);
```

Meaning:

```text
f
 ↓
pointer
 ↓
function taking int
 ↓
returns int
```

### Compare visually

```text
int *f(int)
    ↑
 function

int (*f)(int)
     ↑
   pointer
```

---

# 14. Declaration Precedence

For complex declarations, remember:

```text
()
[]
```

bind more tightly than:

```text
*
```

That's why:

```c
int *p[10];
```

is:

```text
p[10] → array first
then * → pointers
```

while:

```c
int (*p)[10];
```

is:

```text
(*p) → pointer first
then [10] → array
```

### Mental rule

> **Parentheses are used to force `*` to bind to the identifier before `[]` or `()`.**

---

# 15. GATE Declaration Strategy

Whenever you see:

```c
int *(*fp)(int);
```

Don't try to memorize the whole syntax.

Do this:

```text
1. Find identifier → fp

2. Look right
   (int)
   → function taking int

3. Look left
   *
   → pointer to function

4. Reach int
   → function returns int
```

Therefore:

```text
fp = pointer to function
    taking int
    returning int
```

This method scales to extremely complicated declarations.

---

# 16. Quick GATE Reference

|Declaration|Meaning|
|---|---|
|`int *p`|pointer to int|
|`int p[10]`|array of 10 int|
|`int *p[10]`|array of 10 pointers to int|
|`int (*p)[10]`|pointer to array of 10 int|
|`int *f()`|function returning pointer to int|
|`int (*f)()`|pointer to function returning int|
|`int (*f)(int)`|pointer to function taking int, returning int|
|`int (*apa[5])[10]`|array of 5 pointers to arrays of 10 int|

---

## 🧠 The One Pattern You Should Remember

For **every complex C declaration**:

```text
                 FIND IDENTIFIER
                       ↓
                 Look immediately right
                       ↓
                  [] or ()
                       ↓
                  Look left
                       ↓
                       *
                       ↓
                  Continue outward
                       ↓
                    Base type
```

Translate as:

```text
*   → pointer to
[]  → array of
()  → function returning
```

### GATE Trigger ⚠️

When you see **parentheses around `*identifier`**:

```c
(*p)
```

immediately think:

> **`p` is a pointer to something.**

Then inspect what comes after it:

```c
int (*p)[10]
       ↑
pointer to array

int (*p)(int)
       ↑
pointer to function
```

That single distinction handles a huge percentage of complex-declaration questions.