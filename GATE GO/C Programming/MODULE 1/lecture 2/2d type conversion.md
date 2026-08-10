# Type Conversion + Conditional Statements

### Remaining pages: 69–79 | GATE Notes

## 1. Type Conversion

**Type conversion** means converting a value from one data type to another.

The lecture gives the conversion hierarchy as:

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
                ↗   ↖
            short   char
```

### Core idea

When different data types participate in an expression, C may convert one type into another according to this hierarchy.

---

# 2. Integer Division vs Float Division ⭐

Consider:

```c
int value1 = 10, value2 = 3;
float result;

result = value1 / value2;
```

At first glance, you may think:

```text
10 / 3 = 3.333...
```

But **both operands are `int`**.

Therefore:

```text
10 / 3
↓
integer division
↓
3
```

Then:

```text
result = 3;
```

So:

```c
printf("%f", result);
```

prints:

```text
3.000000
```

The lecture's Question 1 demonstrates exactly this case.

### ⭐ GATE rule

```text
int / int → int
```

Even if the result variable is `float`.

---

# 3. One Operand `float` → Float Division

Now:

```c
int a = 10;
float b = 3;
float result;

result = a / b;
```

Here:

```text
int / float
```

The `int` is implicitly converted to `float`:

```text
(float)10 / 3.0f
```

Therefore:

```text
10.0 / 3.0
= 3.333...
```

The lecture explicitly marks this as **float division**.

### Pattern

```text
int / int       → integer division
int / float     → float division
float / int     → float division
float / float   → float division
```

---

# 4. Explicit Type Conversion / Casting ⭐

You can force conversion using a **cast**:

```c
(float)x
```

Example:

```c
int x = 10;
int y = 3;
float result;

result = (float)x / y;
```

First:

```text
(float)x
   ↓
10.0
```

Then:

```text
10.0 / 3
```

`y` is implicitly converted to `float`:

```text
10.0 / 3.0
= 3.333...
```

The lecture's Question 3 demonstrates this.

---

# 5. Cast Only One Operand

You don't need to cast both operands.

```c
result = (float)x / y;
```

is enough.

Why?

```text
(float)x / y
     ↓
float / int
     ↓
int converted to float
     ↓
float / float
```

So:

```text
10 / 3       → 3
(float)10/3 → 3.333...
```

---

# 6. Cast the Other Operand

You can also write:

```c
result = x / (float)y;
```

Now:

```text
int / float
```

so `x` is promoted to `float`.

```text
10 / 3.0
= 3.333...
```

The lecture's Question 4 demonstrates this form.

### ⭐ Important

These are equivalent for this situation:

```c
(float)x / y
```

and

```c
x / (float)y
```

Both force **floating-point division**.

---

# 7. The Most Important Trap

### ❌ Wrong thinking

```c
float result;
result = x / y;
```

does **NOT** mean:

```text
x / y happens as float
```

The operation is decided **before assignment**.

For:

```c
int x = 10;
int y = 3;

float result = x / y;
```

the process is:

```text
x / y
 ↓
int / int
 ↓
3
 ↓
3 converted to float
 ↓
3.0
```

### Correct mental model

```text
                  OPERATION
                     ↓
        determine operand types
                     ↓
             perform operation
                     ↓
              assignment
                     ↓
        convert result if needed
```

---

# 8. GATE Question 1

```c
int value1 = 10, value2 = 3;
float result;

result = value1 / value2;
printf("%f", result);
```

### Solve:

```text
value1 / value2
= int / int
= 3
```

Then:

```text
3 → float
= 3.000000
```

**Answer: `3.000000`**

---

# 9. GATE Question 2

```c
int a = 10;
float b = 3;
float result;

result = a / b;
```

Types:

```text
int / float
```

Convert `a`:

```text
10 → 10.0
```

Then:

```text
10.0 / 3.0
= 3.333...
```

**Answer: approximately `3.333333`**

---

# 10. GATE Question 3

```c
int x = 10;
int y = 3;
float result;

result = (float)x / y;
```

```text
(float)x
   ↓
10.0

10.0 / 3
   ↓
float division
   ↓
3.333...
```

**Answer: approximately `3.333333`**

---

# 11. GATE Question 4

```c
int x = 10;
int y = 3;
float result;

result = x / (float)y;
```

```text
x / (float)y
= 10 / 3.0
= 3.333...
```

**Answer: approximately `3.333333`**

---

# 12. ⭐ Tough Question — Signed vs Unsigned

This is the **important GATE question** from the lecture.

```c
unsigned int a = 1000;
int b = -1;

if (a > b)
    printf("a is BIG");
else
    printf("a is SMALL");
```

At first glance:

```text
1000 > -1
```

so you might answer:

```text
a is BIG
```

❌ **Wrong.**

The output is:

```text
a is SMALL
```

The lecture shows this result experimentally.

---

# 13. Why?

This is where the **type conversion hierarchy** becomes important.

We have:

```text
a → unsigned int
b → int
```

When comparing:

```c
a > b
```

the `int` gets converted to `unsigned int`.

So:

```text
b = -1
```

becomes an **unsigned integer**.

For a 32-bit `unsigned int`:

```text
-1 → 2^32 - 1
```

which is:

```text
4294967295
```

Therefore the comparison effectively becomes:

```text
1000 > 4294967295
```

which is:

```text
FALSE
```

Hence:

```text
a is SMALL
```

The lecture highlights the `-1` binary representation and the `unsigned` vs `signed` comparison as the reason for this behavior.

### 🔥 GATE trap

Never compare only the **numerical values written in the code**.

First ask:

> **What types are being compared?**

Then determine whether conversion occurs.

---

# 14. Conditional Statements

Page 79 begins the next topic:

**Conditional Statements**.

The lecture lists:

```text
1. if statement
2. switch statement
```

So this PDF ends by introducing the next section rather than explaining these constructs in detail.

---

# 🔥 Final GATE Cheat Sheet

```text
TYPE CONVERSION
────────────────────────────────

int / int
   ↓
integer division

int / float
   ↓
float division

float / int
   ↓
float division


Explicit cast:

(float)x / y
x / (float)y

→ forces floating-point division


IMPORTANT:
Result variable's type DOES NOT determine
how the operation is performed.

Example:

float r = 10 / 3;

10 / 3
↓
int / int
↓
3
↓
3.0 stored in r


MIXED SIGNED/UNSIGNED:
────────────────────────

unsigned int a = 1000;
int b = -1;

a > b

int b → unsigned int

-1 → 2^32 - 1
   → 4294967295

1000 > 4294967295
→ FALSE

Output:
"a is SMALL"


CONDITIONAL STATEMENTS
────────────────────────

if statement
switch statement
```

### 🧠 Pattern to recognize in GATE

Whenever you see an expression, **don't calculate immediately**.

Do this:

```text
        1. Identify operand types
                  ↓
        2. Apply type conversion
                  ↓
        3. Determine operation
                  ↓
        4. Calculate
                  ↓
        5. Apply assignment conversion
```

That sequence will save you from a _ton_ of C questions.