# GATE C Programming — Short Notes

### Lecture 1/5 — Integer Promotion → 2's Complement → Type Conversion

## 1. Signed & Unsigned

### Signed `n`-bit integer

```text
Range = -2^(n-1) to 2^(n-1)-1
```

- MSB `0` → positive/non-negative
    
- MSB `1` → negative
    
- Negative numbers → **2's complement**
    

### Unsigned `n`-bit integer

```text
Range = 0 to 2^n - 1
```

- No sign bit
    
- All bits represent magnitude
    

**Same bits can have different values depending on signed/unsigned interpretation.**

---

## 2. Integer Promotion ⭐⭐⭐

```text
char / short
     ↓
    int
```

When `char` or `short` participates in an **expression**, it is promoted to `int`.

### Example

```c
signed char a = 30, b = 40;
signed char c = a * b;
```

```text
a,b → int
30 × 40 → 1200
1200 → signed char
→ truncate to 8 bits
→ -80
```

Therefore:

```text
a * b → 1200
c     → -80
```

**GATE trap:** promotion happens **before arithmetic**; truncation happens when assigning back to the smaller type.

---

## 3. Extension vs Truncation

```text
Small type → Large type = Extension
Large type → Small type = Truncation
```

### Signed

Sign is preserved during extension.

### Example

```text
signed char → int
```

Positive:

```text
01011010 → 00000000 00000000 00000000 01011010
```

Negative → **sign extension**.

---

## 4. `char` as Integer

```c
char c = 'a';
```

```text
'a' = 97
```

Therefore:

```c
c + 1
```

behaves like:

```text
97 + 1 = 98
```

Character values can participate in arithmetic.

---

# 5. 2's Complement ⭐⭐⭐

### 1's complement

Invert every bit:

```text
0 → 1
1 → 0
```

### 2's complement

```text
2's complement = 1's complement + 1
```

Example:

```text
0101
↓ invert
1010
↓ +1
1011
```

So:

```text
2's complement(0101) = 1011
```

### Fast method ⭐

From **right to left**:

1. Copy bits up to and including the **first `1`**
    
2. Invert everything to its left
    

---

## 6. Negative Number using 2's Complement

To represent `-X`:

```text
X
↓
invert bits
↓
+1
↓
representation of -X
```

Example:

```text
+5 = 0101
-5 = 1011
```

Verification:

```text
  0101
+ 1011
------
 10000
```

Discard carry:

```text
0000
```

So:

```text
X + (-X) = 0
```

for fixed-width arithmetic, ignoring carry out.

---

## 7. Decoding 2's Complement

For:

```text
1011
```

MSB = `1` → negative.

Find magnitude:

```text
1011
↓ invert
0100
↓ +1
0101 = 5
```

Therefore:

```text
1011 = -5
```

---

## 8. `n`-bit 2's Complement Range

```text
-2^(n-1) to 2^(n-1)-1
```

For 4 bits:

```text
-8 to +7
```

```text
0000 →  0
0111 → +7
1000 → -8
1111 → -1
```

---

# 9. Type Conversion ⭐⭐⭐

Lecture hierarchy:

```text
long double
     ↑
   double
     ↑
   float
     ↑
unsigned long int
     ↑
long int
     ↑
unsigned int
     ↑
int
   ↗ ↖
short char
```

### Core rule

When different types participate in an expression, lower-ranked types may be converted to a higher-ranked type according to the lecture's conversion hierarchy.

---

# 10. Integer vs Floating Division ⭐⭐⭐

### `int / int`

```c
10 / 3
```

```text
int / int
→ integer division
→ 3
```

Even:

```c
float result = 10 / 3;
```

gives:

```text
3.000000
```

because **operation happens first, assignment happens later**.

### Mixed

```c
10 / 3.0
```

```text
int / float
→ float division
→ 3.333...
```

---

# 11. Explicit Casting

Force conversion:

```c
(float)x / y
```

or:

```c
x / (float)y
```

Example:

```c
int x = 10, y = 3;

(float)x / y
```

```text
10 → 10.0
10.0 / 3
→ 3.333...
```

### ⭐ Golden rule

```text
float result = x / y;
```

does **NOT** make `x/y` floating-point division.

Instead:

```text
x / y
↓
operation first
↓
result converted to float
```

---

# 12. Signed + Unsigned Comparison ⭐⭐⭐

Classic GATE trap:

```c
unsigned int a = 1000;
int b = -1;

if (a > b)
    printf("a is BIG");
else
    printf("a is SMALL");
```

`b` gets converted to `unsigned int`.

For 32-bit unsigned:

```text
-1 → 2^32 - 1
   → 4294967295
```

Comparison becomes:

```text
1000 > 4294967295
```

FALSE.

### Answer:

```text
a is SMALL
```

### 🚨 GATE RULE

When signed and unsigned values meet:

> **Don't compare their written numerical values first. Check the types and conversion first.**

---

# 13. `unsigned char = -65`

```c
unsigned char x = -65;
```

For 8 bits:

```text
-65 → 256 - 65
    → 191
```

So:

```text
x = 191
```

---

# 14. Essential GATE Question Patterns

### Pattern 1

```c
int a = 10, b = 3;
float x = a / b;
```

```text
int/int → 3
3 → float
```

**Answer: `3.0`**

---

### Pattern 2

```c
float x = (float)a / b;
```

```text
float/int → float division
```

**Answer: `3.333...`**

---

### Pattern 3

```c
signed char a = 30, b = 40;
signed char c = a * b;
```

```text
a,b → int
30×40 = 1200
1200 → char
→ truncation
→ -80
```

**Answer: `c = -80`**

---

### Pattern 4

```c
unsigned int a = 1000;
int b = -1;

a > b
```

```text
b → unsigned
-1 → UINT_MAX
1000 > UINT_MAX → false
```

**Answer: false**

---

# 🔥 LAST-MINUTE GATE BOX

```text
CHAR / SHORT
─────────────
char, short in expression → int

Small → Large → extension
Large → Small → truncation


2's COMPLEMENT
──────────────
1's comp = invert bits
2's comp = 1's comp + 1

Fast:
Copy from right through first 1,
invert remaining left bits.

n-bit signed range:
-2^(n-1) → 2^(n-1)-1


DIVISION
─────────
int / int     → int division
int / float   → float division
float / int   → float division

float result = int/int
→ still integer division first!


CASTING
───────
(float)x / y
x / (float)y
→ floating-point division


SIGNED + UNSIGNED
─────────────────
Check types BEFORE comparing.

int -1 → unsigned
→ UINT_MAX

Never blindly compare the written values.


GATE SOLVING ORDER
──────────────────
1. Identify types
2. Apply promotion/conversion
3. Perform operation
4. Apply assignment conversion
5. Check truncation/overflow
```

**Page 79 only introduces `if` and `switch`; their detailed rules aren't covered in this lecture, so don't add extra material to these notes from outside the lecture.**