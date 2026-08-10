# C Programming — Integer Promotion & Related Concepts

### Pages 26–49 | GATE Short Notes

## 1. Small Integer Types

The lecture considers:

|Type|Size|
|---|--:|
|`char`|1 byte = 8 bits|
|`short`|2 bytes = 16 bits|
|`int`|4 bytes = 32 bits|

All three are **integer types**, but `char` and `short` are called **small integer types** in the lecture.

---

# 2. Character as an Integer

A `char` stores a numeric value corresponding to a character.

```c
char c = 'a';
```

`'a'` has ASCII value:

```text
'a' = 97
```

Therefore:

```c
printf("%d", c);
```

outputs:

```text
97
```

The lecture also uses:

```c
c + 1
```

and since:

```text
'a' = 97
97 + 1 = 98
```

the result is `98`.

### GATE idea

```text
character ↔ integer value
```

So arithmetic can be performed on characters.

---

# 3. Integer Promotion ⭐

### Definition

> Whenever a **small integer type (`char` or `short`) is used in an expression, it is implicitly converted to `int`.**

Remember:

```text
char / short
     ↓
   int
```

This happens **when they participate in an expression**.

### Why?

The lecture's examples show that although `char` occupies only 8 bits, operations involving it are performed after promotion to `int`.

---

# 4. Extension During Promotion

Example:

```c
signed char c = 90;
printf("%d", c);
```

`90` in 8-bit binary:

```text
01011010
```

When promoted to `int`, the value is extended to the larger representation.

For a **positive signed value**, the extension preserves the value.

```text
8-bit:
01011010

       ↓ promotion

32-bit:
00000000 00000000 00000000 01011010
```

Result:

```text
90
```

---

# 5. Signed `char` with Value > 127

Example from the lecture:

```c
signed char x = 130;
```

A signed 8-bit `char` cannot represent `130`.

The 8-bit pattern for `130` is:

```text
10000010
```

Interpreting this as signed 2's complement gives:

```text
-126
```

Therefore:

```c
printf("%d", x);
```

gives:

```text
-126
```

The lecture demonstrates this conversion on pages 33–34.

### Important pattern

```text
Value doesn't fit in small type
        ↓
bits are retained/truncated
        ↓
remaining bits interpreted according to signed/unsigned
```

---

# 6. `printf("%u")` and Signed `char`

Example:

```c
signed char x = 130;
printf("%u", x);
```

The `char` is first promoted to `int`.

The lecture demonstrates that `%u` then results in a **large unsigned number** because the promoted signed value is being interpreted as unsigned.

### ⚠️ GATE trap

Don't simply look at:

```c
signed char x = ...
```

and immediately treat it as 8 bits during `printf`.

Ask:

> **Has integer promotion happened?**

For `char`/`short` in an expression → **promote to `int`**.

---

# 7. `unsigned char`

Example:

```c
unsigned char x = 130;
```

`130` fits inside an unsigned 8-bit value:

```text
0 → 255
```

So:

```c
printf("%d", x);
printf("%u", x);
```

both give:

```text
130
```

The lecture demonstrates this on pages 36–37.

Why?

```text
unsigned char x = 130
        ↓
integer promotion
        ↓
int 130
```

So the value remains `130`.

---

# 8. Signed `char` = `-65`

```c
signed char f = -65;
```

The stored 8-bit pattern represents `-65`.

```c
printf("%d", f);
```

→

```text
-65
```

But when promoted and interpreted using an unsigned format, the lecture demonstrates a **large unsigned result**.

---

# 9. Unsigned `char` = `-65` ⭐

This is a very important example.

```c
unsigned char f = -65;
```

Since unsigned `char` has 8 bits:

```text
Range = 0 to 255
```

`-65` is represented modulo 256:

```text
256 - 65 = 191
```

So:

```text
unsigned char f = -65
                 ↓
                191
```

The lecture demonstrates:

```c
printf("%d", f);
printf("%u", f);
```

both giving:

```text
191
```

because after promotion:

```text
unsigned char 191
       ↓
     int 191
```

---

# 10. Overflow + `signed char`

Example:

```c
signed char a = 256;
```

Since `signed char` has 8 bits, only the lower 8 bits remain.

```text
256 = 1 00000000
          ↑
       lower 8 bits
```

So:

```text
00000000
```

which represents:

```text
0
```

The lecture demonstrates this on page 38.

### Pattern

For an 8-bit type:

```text
256 → 0
257 → 1
258 → 2
...
```

---

# 11. `signed char` Multiplication — VERY IMPORTANT ⭐⭐⭐

Consider:

```c
signed char a = 30;
signed char b = 40;

signed char d = a * b;
```

### Step 1 — Promotion

`a` and `b` are `signed char`.

Therefore:

```text
a → int
b → int
```

So multiplication happens as:

```text
30 × 40 = 1200
```

### Step 2 — Assignment to `d`

But `d` is only a `signed char`.

Therefore `1200` must be stored in 8 bits.

```text
1200 in binary:

00000100 10110000
```

Lower 8 bits:

```text
10110000
```

As signed 8-bit 2's complement:

```text
10110000 = -80
```

Therefore:

```c
printf("%d", d);
```

→

```text
-80
```

But:

```c
printf("%d", a * b);
```

→

```text
1200
```

because **the multiplication occurs after integer promotion**, before the result is assigned to `d`.

### 🔥 Remember this flow

```text
signed char a = 30
signed char b = 40

        ↓

       int
       int

        ↓
     a × b

        ↓

      1200

        ↓ assignment to signed char

     8-bit truncation

        ↓

      10110000

        ↓

       -80
```

This is a **classic GATE pattern**.

---

# 12. The Most Important Distinction

### Expression

```c
a * b
```

with `a` and `b` as `char`:

```text
char
 ↓
int
 ↓
multiplication
```

### Assignment

```c
signed char d = a * b;
```

After multiplication:

```text
1200
 ↓
convert to signed char
 ↓
truncate to 8 bits
 ↓
-80
```

So:

```text
a * b       → 1200
d           → -80
```

---

# 13. Summary of Lecture

The lecture's summary highlights these points:

### ① Integer types

```text
char
short
int
```

are integer types.

### ② `printf()` does not use the variable's type to decide the format

The **format specifier** matters:

```text
%d → decimal signed interpretation
%u → unsigned decimal interpretation
```

### ③ Extension

Smaller representation → larger representation.

### ④ Truncation

Larger representation → smaller representation.

### ⑤ Integer Promotion ⭐

```text
char / short
      ↓
     int
```

when used in an expression.

---

# GATE Quick Rules 🧠

Write this box in your notes:

```text
┌─────────────────────────────────────────┐
│        INTEGER PROMOTION RULES           │
├─────────────────────────────────────────┤
│ char / short in an expression → int     │
│                                         │
│ Small → Large  = Extension              │
│ Large → Small  = Truncation             │
│                                         │
│ signed char:  8-bit → -128 to 127       │
│ unsigned char: 8-bit → 0 to 255         │
│                                         │
│ Promotion happens BEFORE arithmetic     │
│ Assignment to char happens AFTER        │
│ arithmetic → possible truncation        │
└─────────────────────────────────────────┘
```

### One question you should be able to solve immediately:

```c
signed char a = 30, b = 40;
signed char d = a * b;

printf("%d", d);
printf("%d", a * b);
```

**Answer:**

```text
-80
1200
```

Reason:

```text
a,b → int
30×40 → 1200
1200 → signed char → -80
```

---

### Page 49

Page 49 only introduces the next topic:

**2's Complement** — the detailed material begins after this page.