Absolutely. Treat these **5 sub-lectures as one single L2: Integer Representation, Promotion & Type Conversion**.

Since you're following Go Classes for GATE, I'll keep this **exam-oriented and compact**—enough intuition to understand questions, but no unnecessary C rabbit holes.

# C Programming — L2 Short Notes

### Signed/Unsigned → Integer Promotion → 2's Complement → Type Conversion

---

## 1. Signed & Unsigned Numbers in Memory

A variable is ultimately stored as **bits**.

For `k` bits:

### Unsigned

```text
Range = 0 → 2^k - 1
```

Example: 8-bit

```text
0 → 255
```

### Signed 2's Complement

```text
Range = -2^(k-1) → 2^(k-1)-1
```

Example:

```text
8-bit → -128 → +127
```

### ⭐ GATE IMP CORNER

**Same bits can represent different values.**

```text
11111111
```

As:

```text
unsigned → 255
signed   → -1
```

So always ask:

> **Signed or unsigned? How many bits?**

---

# 2. 2's Complement

For a negative number:

```text
1. Write positive magnitude in binary
2. Invert all bits
3. Add 1
```

Example: `-5` in 8 bits:

```text
+5
00000101

invert
11111010

+1
11111011
```

Therefore:

```text
-5 = 11111011
```

### ⭐ GATE IMP CORNER

For signed 2's complement:

```text
MSB = 0 → positive/non-negative
MSB = 1 → negative
```

For a negative value, to find decimal:

```text
invert → +1 → decimal → negative sign
```

---

# 3. Integer Promotion ⭐⭐⭐

This is the big idea of L2.

Small integer types are generally promoted before arithmetic.

```text
char
short
_Bool
 ↓
integer promotion
 ↓
int / unsigned int
```

Example:

```c
char a = 30;
char b = 40;

a * b
```

It is **not**:

```text
char × char → char
```

Instead:

```text
char → int
char → int

int × int
   ↓
 1200
```

So:

```c
a * b
```

has an `int` result.

---

# 4. Why Integer Promotion Matters

Look at:

```c
signed char a = 30;
signed char b = 40;

signed char d = a * b;
```

First:

```text
a → int 30
b → int 40

30 × 40
   ↓
1200
```

Then assignment:

```text
int 1200
    ↓
signed char
```

If `signed char` is 8-bit:

```text
1200 = 00000100 10110000
                         ↓
                  lower 8 bits
                         ↓
                    10110000
                         ↓
                      -80
```

Therefore:

```c
printf("%d", d);
```

→ `-80`

But:

```c
printf("%d", a * b);
```

→ `1200`

### 🔥 GATE IMP CORNER

Remember this chain:

```text
small type
    ↓
integer promotion
    ↓
operation
    ↓
result
    ↓
assignment may convert result
```

**Always identify the type at every stage.**

---

# 5. Integer Promotion During `printf`

Example:

```c
signed char g = 100;

printf("%d", g);
```

Why does `%d` work?

```text
signed char
     ↓
integer promotion
     ↓
int
     ↓
%d
```

So:

```text
char/short argument
       ↓
promoted before passing
       ↓
usually int
```

### Remember

```text
%d → signed decimal integer
%u → unsigned decimal integer
```

---

# 6. Signed → Unsigned Conversion

Example:

```c
int x = -1;
unsigned int u = x;
```

Assuming 32-bit integers:

```text
x = -1
```

Representation:

```text
11111111 11111111 11111111 11111111
```

As unsigned:

```text
2^32 - 1
= 4294967295
```

Therefore:

```text
x = -1
u = 4294967295
```

### ⭐ GATE FORMULA

For conversion of a negative integer to `unsigned` with `k` bits:

```text
x → x mod 2^k
```

Example:

```text
-1 mod 2^32
= 4294967295
```

---

# 7. Type Conversion

Two big ideas:

### Implicit conversion

C automatically converts the type.

```c
int x = 10;
float y = x;
```

Conceptually:

```text
int
 ↓
float
```

### Explicit conversion / Casting

You tell C to convert:

```c
float x = 10.5;

int y = (int)x;
```

```text
10.5 → 10
```

---

# 8. Narrowing Conversion ⭐⭐⭐

This is when a larger type is converted into a smaller type.

Example:

```c
int x = 1200;
signed char y = x;
```

Conceptually:

```text
int 1200
    ↓
smaller signed char
    ↓
information may be lost
```

This is exactly what happened in:

```c
signed char d = a * b;
```

### Mental model

```text
Bigger container
       ↓
Smaller container
       ↓
May not fit
       ↓
Information/value can change
```

---

# 9. Widening Conversion

Smaller → larger.

```text
char → int
short → int
int → long
```

Generally safer because the larger type can represent more values.

But **don't blindly assume every conversion preserves value**—GATE may introduce signed/unsigned interactions.

---

# 10. The Most Important L2 Pattern

When you see something like:

```c
char a, b;
char c = a + b;
```

Don't solve it immediately.

Run this checklist:

```text
① What are the original types?

② Do integer promotions happen?

③ What type does the operation happen in?

④ What is the result?

⑤ Is the result assigned to another type?

⑥ Can that destination type hold it?

⑦ How is the final value interpreted?
```

That's the **problem-solving pattern** behind most of this lecture.

---

# 🧠 GATE IMP CORNER

### Memorize these, not random examples:

```text
1. k-bit unsigned:
   0 → 2^k − 1

2. k-bit signed 2's complement:
   −2^(k−1) → 2^(k−1) − 1

3. Negative 2's complement:
   invert + 1

4. char/short generally undergo integer promotion
   before arithmetic.

5. Arithmetic often happens at int level,
   NOT at char level.

6. Assignment can convert the result back
   to a smaller type.

7. Signed ↔ unsigned conversion can change
   the interpreted value dramatically.

8. Same bits ≠ same numerical value.
   Interpretation matters.
```

---

# 🔥 GATE Question Attack Strategy

When you see C code involving:

```text
char
short
signed
unsigned
%d
%u
+
-
*
/
2's complement
casting
assignment
```

**Don't calculate immediately.**

First make a **type table**:

```text
Variable     Type
---------------------
a            signed char
b            signed char
a*b          int       ← promotion!
d            signed char
```

Then follow the value:

```text
30, 40
 ↓
promotion
 ↓
30 × 40
 ↓
1200 (int)
 ↓
conversion to signed char
 ↓
final representation
```

That's the habit I want you to build for **GATE C questions**.

### L2 in one picture

```text
       MEMORY
          ↓
       bits
          ↓
   signed / unsigned
          ↓
    interpretation
          ↓
  integer promotion
          ↓
      operation
          ↓
    type conversion
          ↓
    final stored value
```

**This is the entire L2 mental model.**