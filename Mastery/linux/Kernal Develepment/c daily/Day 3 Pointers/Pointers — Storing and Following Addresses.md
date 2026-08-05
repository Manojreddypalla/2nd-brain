# ⚙️ Systems C — Day 3: Pointers — Storing and Following Addresses

**Day 1:** Variables, bytes & addresses ✅  
**Day 2:** Bits, signed/unsigned & integer representation ✅  
**Day 3:** **Pointers** ← TODAY

This is one of the foundations of systems programming. Don't treat pointers as weird C syntax. Treat them as **ordinary values whose value happens to be a memory address**.

⏱️ **45–60 min**

---

## 1. The core idea

Yesterday you already used:

```c
int x = 10;

printf("%p\n", (void *)&x);
```

Suppose:

```text
x lives at address 0x1000
```

Then conceptually:

```text
Memory

Address       Contents

0x1000        10
              ↑
              x
```

And:

```c
&x
```

produces:

```text
0x1000
```

Now here's the new question:

> What if I want to **store that address**?

That's what a pointer does.

```c
int *p = &x;
```

Mental model:

```text
x                           p
address: 0x1000             address: 0x2000
┌──────────────┐            ┌──────────────┐
│      10      │            │    0x1000    │
└──────────────┘            └──────────────┘
      ▲                            │
      │                            │
      └────────────────────────────┘
```

Notice something important:

**`p` also occupies memory.**

It's a variable.

The difference is simply:

```text
x stores → integer

p stores → address
```

---

# 2. Read pointer syntax correctly

This:

```c
int *p;
```

means:

> `p` is a pointer to an `int`.

Then:

```c
p = &x;
```

means:

> store the address of `x` inside `p`.

We now have three expressions worth distinguishing:

```text
x
↓
10


&x
↓
address of x
↓
0x1000


p
↓
address stored in p
↓
0x1000
```

Therefore:

```text
p == &x
```

conceptually.

But there's one more operation.

---

# 3. Dereferencing

If:

```c
p = &x;
```

then:

```c
*p
```

means:

> Go to the address stored inside `p` and interpret the object there as an `int`.

This is called **dereferencing**.

```text
p
│
│ contains 0x1000
│
▼

0x1000
┌──────────┐
│    10    │
└──────────┘
     ▲
     │
    *p

Result: 10
```

So keep these four separate:

```text
x     → value of x

&x    → address of x

p     → address stored in p

*p    → value found by following p
```

That's today's entire conceptual foundation.

---

# 4. The coolest consequence

If:

```c
int x = 10;
int *p = &x;
```

then:

```c
*p = 99;
```

What happens?

Think before reading further.

`p` contains the address of `x`.

Therefore:

```text
*p
```

means:

```text
follow p
   ↓
reach x
```

So:

```c
*p = 99;
```

doesn't modify `p`.

It modifies the object **p points to**.

```text
BEFORE

x
┌──────┐
│  10  │
└──────┘
   ▲
   │
   p


*p = 99;


AFTER

x
┌──────┐
│  99  │
└──────┘
   ▲
   │
   p
```

This ability to access objects **indirectly** is why pointers are so powerful.

---

# 🔬 5. Lab 1 — See everything

Create:

```bash
nano day03.c
```

Write:

```c
#include <stdio.h>

int main(void)
{
    int x = 10;
    int *p = &x;

    printf("x   = %d\n", x);
    printf("&x  = %p\n", (void *)&x);

    printf("p   = %p\n", (void *)p);
    printf("*p  = %d\n", *p);

    printf("&p  = %p\n", (void *)&p);

    return 0;
}
```

Compile:

```bash
gcc -Wall -Wextra day03.c -o day03
```

Run:

```bash
./day03
```

Don't focus on the exact addresses.

Compare:

```text
&x
```

with:

```text
p
```

They should contain the same address.

But:

```text
&p
```

will be different.

Why?

Because:

```text
p
```

is itself another variable stored somewhere.

That's an important distinction.

---

# 🔬 6. Lab 2 — Modify through the pointer

Add:

```c
printf("Before: x = %d\n", x);

*p = 500;

printf("After:  x = %d\n", x);
```

Predict before running.

You'll get:

```text
Before: x = 10
After:  x = 500
```

Visualize what happened:

```text
*p = 500

      p
      │
      │ address
      ▼
┌───────────┐
│     x     │
│           │
│    500    │
└───────────┘
```

You modified `x` without writing:

```c
x = 500;
```

You reached it **indirectly through its address**.

---

# 7. Why does the pointer have a type?

Why:

```c
int *p;
```

instead of simply:

```text
pointer p
```

?

Because an address by itself doesn't tell C enough about the object you're going to access.

Suppose:

```text
p = 0x1000
```

C needs to know:

> When I dereference `p`, what type of object should I access there?

With:

```c
int *p;
```

C knows:

```text
p
↓
address

*p
↓
treat object there as int
```

Later this becomes crucial for:

```text
pointer arithmetic
struct pointers
arrays
memory buffers
```

---

# 8. Pointer size

Try:

```c
printf("sizeof(int)  = %zu\n", sizeof(int));
printf("sizeof(int*) = %zu\n", sizeof(int *));
```

On a typical 64-bit machine you'll probably get:

```text
sizeof(int)  = 4
sizeof(int*) = 8
```

Why can a pointer be 8 bytes when the `int` it points to is only 4?

Because these are completely different things.

```text
int
↓
stores integer data


int *
↓
stores an address
```

On a typical 64-bit environment, pointers are commonly 64 bits / 8 bytes.

---

# 9. Different pointer types

You can have:

```c
char *cp;
int *ip;
double *dp;
```

Conceptually all contain addresses.

But their types describe what they point to:

```text
char *
  │
  └──→ char


int *
  │
  └──→ int


double *
  │
  └──→ double
```

This becomes very important tomorrow.

Because if:

```c
int *p;
```

then:

```c
p + 1
```

does **not necessarily mean one byte forward**.

That's pointer arithmetic, and we'll derive why rather than memorize it.

---

# ⚠️ 10. The dangerous part

This is valid:

```c
int x = 10;
int *p = &x;

printf("%d\n", *p);
```

Because `p` points to a valid `int`.

But imagine:

```c
int *p;

printf("%d\n", *p);
```

`p` hasn't been initialized to point to a valid `int`.

Conceptually:

```text
p
│
└──→ ????
```

Then:

```c
*p
```

means:

> Go to ???? and read an `int`.

That's invalid behavior.

This is one major source of low-level bugs.

Pointers give you power because C allows direct memory manipulation.

That also means **you are responsible for knowing whether the address is valid**.

---

# 11. NULL

Sometimes we explicitly want:

> This pointer currently points to nothing.

Use:

```c
int *p = NULL;
```

Conceptually:

```text
p
│
└──→ no valid object
```

You can test:

```c
if (p != NULL) {
    printf("%d\n", *p);
}
```

But:

```c
*p
```

when `p == NULL` is invalid.

A typical operating system may terminate the process with a segmentation fault, but from C's perspective the dereference is undefined behavior.

---

# 🔨 12. Today's main lab

Write this yourself rather than copying the whole solution.

Create:

```text
int a = 10;
int b = 20;

int *p = &a;
```

Print:

```text
a
&a
p
*p
&p
```

Then:

```c
*p = 100;
```

Print `a`.

Then change:

```c
p = &b;
```

Now visualize:

```text
BEFORE

p ──────→ a


AFTER

p ──────→ b
```

Then:

```c
*p = 200;
```

Finally print:

```text
a
b
```

You should end with:

```text
a = 100
b = 200
```

But the important part is understanding why.

You never directly performed:

```c
a = 100;
b = 200;
```

You redirected the pointer:

```text
p → a
modify

p → b
modify
```

That's **indirection**.

---

# 🧠 13. Pointer checkpoint

Given:

```c
int x = 50;
int *p = &x;
```

You should be able to immediately visualize:

```text
           p
           │
           │ contains &x
           ▼

        ┌───────┐
x ────→ │  50   │
        └───────┘
```

And answer:

```text
x   → 50

&x  → address of x

p   → address of x

*p  → 50

&p  → address of pointer p
```

If that model is clear, today's job is done.

---

# 📝 Day 3 Revision Note

```text
# Systems C — Day 3: Pointers

Pointer:
variable that stores an address.

int x = 10;
int *p = &x;


x
→ value of x

&x
→ address of x

p
→ address stored in pointer

*p
→ dereference p
→ access object pointed to by p

&p
→ address of pointer itself


Modification:

*p = 50;

changes the pointed-to object.


Pointer type:

int *
→ pointer to int

char *
→ pointer to char


NULL:
pointer intentionally pointing to
no valid object.

Never dereference NULL or an
uninitialized/otherwise invalid pointer.


Mental model:

p
│
│ address
▼
object
```

## 🎯 Progress

```text
Day 1 ✅ Memory & addresses
          ↓
Day 2 ✅ Bits & integers
          ↓
Day 3 ✅ Pointers
          ↓
Day 4    Pointer arithmetic
          ↓
Day 5    Arrays + pointers
          ↓
Day 6    Strings + memory
          ↓
       ...
          ↓
🔥 FIRST MERGE
```

No merge yet. **Linux and Systems C remain completely separate.**

Tomorrow: **Systems C Day 4 — Pointer Arithmetic: why `p + 1` depends on the pointed-to type, how addresses move through memory, and the foundation for understanding arrays and raw buffers.**