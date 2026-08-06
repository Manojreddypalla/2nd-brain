# ⚙️ Systems C — Day 4: Pointer Arithmetic

**Day 1:** Memory & addresses ✅  
**Day 2:** Bits & integers ✅  
**Day 3:** Pointers & dereferencing ✅  
**Day 4:** **Pointer arithmetic** ← TODAY

⏱️ **40–50 min**

Today's goal is one idea:

> `p + 1` means **move to the next object of the pointed-to type**, not necessarily move one byte.

This is the bridge from **pointers → arrays → raw memory traversal**.

---

## 1. Start with the memory model

Suppose:

```c
int x = 10;
int *p = &x;
```

If an `int` is 4 bytes and `x` begins at:

```text
0x1000
```

then conceptually:

```text
Address

0x1000 ─┐
0x1001  │
0x1002  │ x = 10
0x1003 ─┘

p = 0x1000
```

Now consider:

```c
p + 1
```

A common beginner assumption is:

```text
0x1000 + 1
      ↓
0x1001
```

But `p` is:

```c
int *
```

C interprets `+1` as:

> Move forward by one `int`.

If:

```text
sizeof(int) = 4
```

then conceptually:

```text
p     = 0x1000
p + 1 = 0x1004
p + 2 = 0x1008
p + 3 = 0x100C
```

Think:

```text
p + n

≈ address + n × sizeof(*p)
```

That's the key intuition.

---

# 2. Why does C work this way?

Imagine an array:

```c
int a[4] = {10, 20, 30, 40};
```

Memory is contiguous:

```text
        int      int      int      int

       4 bytes  4 bytes  4 bytes  4 bytes

       ┌──────┬──────┬──────┬──────┐
       │  10  │  20  │  30  │  40  │
       └──────┴──────┴──────┴──────┘
Address
       1000   1004   1008   1012
```

If:

```c
int *p = &a[0];
```

then:

```text
p     → 10

p + 1 → 20

p + 2 → 30

p + 3 → 40
```

So pointer arithmetic naturally walks **objects**, rather than forcing you to manually calculate byte offsets.

---

# 3. Dereferencing after movement

We already know:

```c
*p
```

means:

> Follow `p` and access the pointed-to object.

Therefore:

```c
*(p + 1)
```

means:

```text
take p
 ↓
move one int
 ↓
follow that address
 ↓
access next int
```

Given:

```c
int a[] = {10, 20, 30};
int *p = &a[0];
```

we get:

```text
*p       → 10
*(p + 1) → 20
*(p + 2) → 30
```

Notice how we're beginning to **traverse memory**.

---

# 🔬 4. Lab — observe real addresses

Create:

```bash
nano day04.c
```

Write:

```c
#include <stdio.h>

int main(void)
{
    int a[] = {10, 20, 30, 40};

    int *p = &a[0];

    printf("sizeof(int) = %zu\n\n", sizeof(int));

    printf("p     = %p\n", (void *)p);
    printf("p + 1 = %p\n", (void *)(p + 1));
    printf("p + 2 = %p\n", (void *)(p + 2));
    printf("p + 3 = %p\n", (void *)(p + 3));

    return 0;
}
```

Compile:

```bash
gcc -Wall -Wextra day04.c -o day04
```

Run:

```bash
./day04
```

Your addresses will differ, but look at the **difference** between them.

You might see:

```text
sizeof(int) = 4

p     = 0x...a0
p + 1 = 0x...a4
p + 2 = 0x...a8
p + 3 = 0x...ac
```

That is not coincidence.

```text
int * + 1
      ↓
move sizeof(int)
      ↓
4 bytes on this machine
```

---

# 🔬 5. Now dereference them

Add:

```c
printf("*p       = %d\n", *p);
printf("*(p + 1) = %d\n", *(p + 1));
printf("*(p + 2) = %d\n", *(p + 2));
printf("*(p + 3) = %d\n", *(p + 3));
```

Predict before running:

```text
10
20
30
40
```

Memory traversal:

```text
p
│
▼

┌──────┬──────┬──────┬──────┐
│  10  │  20  │  30  │  40  │
└──────┴──────┴──────┴──────┘
   ▲      ▲      ▲      ▲
   │      │      │      │
  *p   *(p+1) *(p+2) *(p+3)
```

This is the foundation of array traversal.

---

# 6. Pointer type controls the jump

Here's the important experiment.

Create:

```c
char c[] = {'A', 'B', 'C'};
int  x[] = {10, 20, 30};

char *cp = &c[0];
int  *ip = &x[0];
```

Print:

```c
printf("cp     = %p\n", (void *)cp);
printf("cp + 1 = %p\n", (void *)(cp + 1));

printf("ip     = %p\n", (void *)ip);
printf("ip + 1 = %p\n", (void *)(ip + 1));
```

Typically:

```text
char *

cp
↓
+1 byte


int *

ip
↓
+4 bytes
```

Why?

Because:

```text
sizeof(char) = 1

sizeof(int)  = typically 4
```

General rule:

```text
T *p;

p + 1
```

moves by:

```text
sizeof(T)
```

bytes.

---

# 7. `p++`

If:

```c
int *p = &a[0];
```

then:

```c
p++;
```

advances `p` to the next `int`.

Conceptually:

```text
BEFORE

p
│
▼

10      20      30
▲
│
p


p++


AFTER

10      20      30
        ▲
        │
        p
```

Then:

```c
printf("%d\n", *p);
```

prints:

```text
20
```

This gives us a natural traversal pattern:

```c
int *p = &a[0];

printf("%d\n", *p);
p++;

printf("%d\n", *p);
p++;

printf("%d\n", *p);
```

You're literally moving through contiguous memory.

---

# 8. Pointer subtraction

Suppose:

```c
int a[] = {10, 20, 30, 40};

int *p = &a[0];
int *q = &a[3];
```

Then:

```c
q - p
```

gives the distance in **array elements**, not bytes.

Conceptually:

```text
p                       q
│                       │
▼                       ▼

10      20      30      40
0       1       2       3

q - p
  ↓
  3
```

This is useful later for buffers and array ranges.

Pointer subtraction is meaningful under C's rules when the pointers refer into the same array object (or one past it).

---

# ⚠️ 9. The boundary rule

Suppose:

```c
int a[4];
```

These pointer values are meaningful for traversal:

```text
&a[0]
&a[1]
&a[2]
&a[3]
```

C also allows forming:

```c
&a[4]
```

as the **one-past-the-end pointer**.

Think:

```text
      valid elements

┌────┬────┬────┬────┐
│ a0 │ a1 │ a2 │ a3 │
└────┴────┴────┴────┘
                    ▲
                    │
                  one past
```

You may use that pointer as a boundary:

```c
p < &a[4]
```

But you must **not dereference it**:

```c
*(&a[4])   // invalid
```

There's no element there.

This gives C a very useful range model:

```text
[start, end)
```

where:

```text
start → first element

end → one past last element
```

You'll see this pattern constantly in systems programming.

---

# 🧪 10. Main coding lab

Write:

```c
#include <stdio.h>

int main(void)
{
    int values[] = {10, 20, 30, 40, 50};

    int *p = &values[0];
    int *end = &values[5];

    while (p < end)
    {
        printf("address=%p value=%d\n",
               (void *)p,
               *p);

        p++;
    }

    return 0;
}
```

Before running it, visualize:

```text
p
│
▼

10    20    30    40    50
│
read
│
p++
     │
     ▼
10    20    30    40    50
      │
     read
      │
     p++
            ↓
           ...
```

Compile:

```bash
gcc -Wall -Wextra day04.c -o day04
```

Run:

```bash
./day04
```

You just traversed an array **without using an array index**.

That's an important milestone.

---

# 11. Don't memorize `a[i]` as magic

Here's a preview of tomorrow.

Given:

```c
int a[] = {10, 20, 30};
```

C's array indexing is deeply connected to pointer arithmetic:

```c
a[2]
```

corresponds to:

```c
*(a + 2)
```

Mentally:

```text
a
↓
start address

a + 2
↓
move two elements

*(a + 2)
↓
access that element
```

So:

```text
array indexing
```

and:

```text
pointer traversal
```

aren't two unrelated ideas.

That's exactly what Day 5 will unpack properly.

---

# 🧠 Day 4 checkpoint

Given:

```c
int a[] = {5, 10, 15, 20};

int *p = &a[0];
```

You should visualize:

```text
              p
              │
              ▼

Memory:

┌─────┬─────┬─────┬─────┐
│  5  │ 10  │ 15  │ 20  │
└─────┴─────┴─────┴─────┘
   0     1     2     3
```

Then immediately understand:

```text
*p
→ 5

p + 1
→ address of 10

*(p + 1)
→ 10

p + 3
→ address of 20

*(p + 3)
→ 20
```

Most importantly:

```text
p + 1

does NOT mean

address + 1 byte
```

It means:

```text
next object of p's pointed-to type
```

---

# 📝 Quick Revision Note

```text
# Systems C — Day 4
## Pointer Arithmetic

Pointer arithmetic moves in units
of the pointed-to type.

T *p;

p + 1
→ next T object

Conceptually:

new address =
old address + sizeof(T)


Example:

int *p

if sizeof(int) = 4:

p     → 0x1000
p + 1 → 0x1004
p + 2 → 0x1008


Dereference:

*(p + n)
→ access nth object from p


Increment:

p++
→ move to next object


Pointer subtraction:

q - p
→ distance in elements
  when valid within same array


One-past-end:

&a[array_size]

may be used as boundary

but MUST NOT be dereferenced.


Important connection:

a[i]

is connected to:

*(a + i)
```

### Progress

```text
Day 1 ✅ Memory
Day 2 ✅ Integer representation
Day 3 ✅ Pointers
Day 4 ✅ Pointer arithmetic
             │
             ▼
Day 5    Arrays + pointers
             │
             ▼
Day 6    Strings + byte traversal
             │
             ▼
         Structs / memory layout
             │
             ▼
         Bit manipulation
             │
             ▼
            ...
             │
             ▼
        🔥 FIRST MERGE
```

**Systems C and Linux Internals remain separate.**

Next: **Systems C Day 5 — Arrays & Pointers**, where we'll derive why arrays and pointers feel so closely related in C, while also learning the crucial reason **an array is not actually a pointer**.