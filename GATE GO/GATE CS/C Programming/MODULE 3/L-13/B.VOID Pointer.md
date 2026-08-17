# Void Pointer (`void *`) — GATE C Notes

## 1. What is a Void Pointer?

A **void pointer** is a pointer that can store the address of an object of **any data type**.

```c
void *p;
```

Example:

```c
int a = 7;
char c = 'A';

void *p;

p = &a;   // valid
p = &c;   // also valid
```

Think of `void *` as:

> **"I know this is an address, but I don't currently specify what type of object is there."**

The lecture demonstrates this using an `int` and a `char`.

---

# 2. Why Do We Need `void *`?

Normally, a pointer carries **type information**.

```c
int *p;
```

means:

```text
p → address of an int
```

while:

```c
char *p;
```

means:

```text
p → address of a char
```

But:

```c
void *p;
```

means:

```text
p → address of some object
    ↑
    type not specified
```

So `void *` is useful when a function needs to work with **different types of data**.

---

# 3. Void Pointer Can Store Any Object Address

```c
int a = 7;
void *p;

p = &a;
```

Memory:

```text
p
│
│ address
▼
┌───────┐
│   7   │
└───────┘
   a
```

Now:

```c
char c = 'A';
p = &c;
```

The same pointer can now point to the character.

```text
p
│
▼
┌───────┐
│   A   │
└───────┘
   c
```

So:

```text
void * → can hold address of different object types
```

---

# 4. ⚠️ Void Pointer Cannot Be Directly Dereferenced

This is the **main GATE point**.

Suppose:

```c
int a = 7;

void *p;
p = &a;
```

Can we do:

```c
*p
```

❌ **No.**

Why?

Because the compiler does not know:

> **What type of object does `p` point to?**

And therefore it doesn't know how to interpret the memory.

For example:

```text
p → 1000
```

Does `1000` contain:

```text
int?
char?
double?
some structure?
```

`void *` doesn't tell us.

The lecture explicitly states that a `void *` cannot be directly dereferenced and must first be typecast.

---

# 5. Typecast Before Dereferencing

If `p` actually points to an `int`:

```c
int a = 7;
void *p = &a;
```

we can recover the type:

```c
*((int *)p)
```

Break it down:

```text
p
↓
(int *)p
↓
convert p into int*
↓
*(int *)p
↓
access the int
```

So:

```c
printf("%d", *((int *)p));
```

gives:

```text
7
```

### Mental model

```text
void *
  ↓
"I only know it's an address"

(int *)
  ↓
"Now I know it's an int address"

*
  ↓
"Now read the int stored there"
```

---

# 6. Why Typecasting Is Necessary

Dereferencing a pointer means:

> **Go to this address and interpret the bytes as this type.**

For example:

```c
int *p;
*p
```

means:

```text
Go to address p
Read sizeof(int) bytes
Interpret them as int
```

But:

```c
void *p;
*p
```

has no type information.

The compiler doesn't know the size or interpretation of the object.

Therefore:

```text
void *p
   ↓
typecast
   ↓
specific pointer type
   ↓
dereference
```

---

# 7. GATE Trap — `void *` vs `int *`

```c
int *p;
```

Compiler knows:

```text
*p → int
```

But:

```c
void *p;
```

Compiler does **not** know:

```text
*p → ?
```

Therefore:

```c
*p
```

is not allowed directly.

Instead:

```c
*(int *)p
```

or another appropriate type depending on what `p` actually points to.

---

# 8. Important Property

A `void *` can hold addresses of different **object types**:

```c
int a;
char c;
double d;

void *p;

p = &a;
p = &c;
p = &d;
```

But when accessing the object:

```text
you must know the actual type
```

and cast accordingly.

---

# 9. `sizeof(void *)`

The lecture also shows that the **size of a pointer itself** is independent of the pointed-to data type on the given system. It compares pointer sizes such as `sizeof(int *)` and `sizeof(char *)`.

For example, on a typical 64-bit system:

```text
sizeof(int *)   = 8
sizeof(char *)  = 8
sizeof(void *)  = 8
```

### Important distinction

```text
sizeof(int)   → size of integer object
sizeof(int*)  → size of pointer storing an address
```

These are completely different things.

---

# 🔥 GATE Quick Revision

```text
void *p;
```

### Meaning

> Generic pointer that can store the address of an object of any type.

### Can assign:

```c
int a;
char c;

p = &a;   // ✓
p = &c;   // ✓
```

### Direct dereference:

```c
*p        // ✗
```

### Typecast first:

```c
*(int *)p     // ✓ if p points to int
*(char *)p   // ✓ if p points to char
```

### Core reason

```text
void * has no specific pointed-to type
        ↓
compiler doesn't know how to interpret memory
        ↓
typecast
        ↓
dereference
```

### 🎯 GATE Trigger

Whenever you see:

```c
void *p;
```

immediately ask:

> **"What does `p` actually point to?"**

Then:

```text
actual type
   ↓
cast to that pointer type
   ↓
dereference
```

That is the entire mental model behind the first four Void Pointer pages.