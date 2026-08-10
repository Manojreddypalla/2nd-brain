# C Programming — Lecture 4: Short Notes

### Operators → Bitwise → Assignment → Sequence Points → Short-Circuiting

This is the **complete Lecture 4 revision sheet**, kept short but with the GATE-level traps.

---

# 1. Arithmetic Operators

```text
+  -  *  /  %
```

### Precedence

```text
* / %    >    + -
```

Same precedence → **Left → Right**

Example:

```c
10 - 6 / 2 + 3
```

```text
= 10 - 3 + 3
= 7 + 3
= 10
```

---

# 2. Relational Operators

```text
<   <=   >   >=   ==   !=
```

Result:

```text
TRUE  → 1
FALSE → 0
```

### 🚨 GATE Trap

```c
10 < 20 < 30
```

C does **not** treat this mathematically.

```c
(10 < 20) < 30
```

```text
1 < 30
→ 1
```

**Always bracket chained comparisons.**

---

# 3. Logical Operators

```text
!   → NOT
&&  → AND
||  → OR
```

### Truth table

```text
A B | A&&B | A||B
------------------
0 0 |  0   |  0
0 1 |  0   |  1
1 0 |  0   |  1
1 1 |  1   |  1
```

```text
!0 → 1
!non-zero → 0
```

### Precedence

```text
!  >  &&  >  ||
```

Associativity:

```text
!   → R → L
&&  → L → R
||  → L → R
```

---

# 4. Increment / Decrement

```text
++x → pre-increment
x++ → post-increment

--x → pre-decrement
x-- → post-decrement
```

### Remember

```text
++x → CHANGE → USE
x++ → USE → CHANGE

--x → CHANGE → USE
x-- → USE → CHANGE
```

Example:

```c
x = 5;
y = x++;
```

Afterwards:

```text
y = 5
x = 6
```

```c
x = 5;
y = ++x;
```

```text
y = 6
x = 6
```

---

# 5. Bitwise Operators

```text
&   |   ^   ~   <<   >>
```

## `&` AND

```text
1 & 1 = 1
otherwise = 0
```

Useful:

```c
n & 1
```

```text
0 → even
1 → odd
```

because the LSB determines odd/even.

## `|` OR

```text
0 | 0 = 0
otherwise = 1
```

## `^` XOR

```text
same      → 0
different → 1
```

### Easy memory

```text
& → BOTH
| → EITHER
^ → DIFFERENT
```

---

# 6. Shift Operators

```text
x << n → shift left n bits
x >> n → shift right n bits
```

For positive values:

```text
x << 1 ≈ x × 2
x >> 1 ≈ x / 2
```

But **think in bits**, don't blindly use these formulas.

### Right shift

```text
unsigned → zeros shifted in

signed → implementation/system dependent
         (0 or sign bit)
```

---

# 7. One's Complement `~`

Flips every bit:

```text
0 → 1
1 → 0
```

Example:

```text
8  = 00001000
~8 = 11110111
```

In the usual 2's-complement representation:

```text
~x = -x - 1
```

Therefore:

```text
~8 = -9
~0 = -1
```

### 🚨 Don't confuse

```text
!x → logical NOT
~x → bitwise NOT
```

---

# 8. Assignment Operators

```text
=   +=   -=   *=   /=   %=
&=  |=   ^=   <<=  >>=
```

### Compound assignment

```c
x += y;
```

means:

```c
x = x + y;
```

More generally:

```c
x op= expression
```

means:

```c
x = x op (expression);
```

Example:

```c
x *= y + 2;
```

means:

```c
x = x * (y + 2);
```

**NOT**

```c
x = (x * y) + 2;
```

### Associativity

```text
Assignment → Right → Left
```

Therefore:

```c
a = b = c = 10;
```

is:

```c
a = (b = (c = 10));
```

---

# 9. Conditional Operator `?:`

Syntax:

```c
condition ? expression1 : expression2
```

```text
TRUE  → expression1
FALSE → expression2
```

Example:

```c
max = (a > b) ? a : b;
```

### Associativity

```text
R → L
```

---

# 10. Comma Operator `,`

Expressions are evaluated:

```text
LEFT → RIGHT
```

Example:

```c
x = (a = 5, b = 10, a + b);
```

Evaluation:

```text
a = 5
b = 10
a + b = 15
```

Therefore:

```text
x = 15
```

### Key point

**The value of a comma expression is the value of the rightmost expression.**

---

# 11. Sequence Points

A sequence point is a point where **previous side effects are guaranteed to have been completed**.

Important ones from the lecture include:

```text
;
&&
||
?:
control statements such as if/for/while/switch
```

### 🚨 GATE Trap

```c
a++ + a++
```

❌ **Undefined behavior**

Because `a` is modified more than once without an intervening sequence point.

But:

```c
a++;
a++;
```

✅ Defined.

### Pattern

Whenever you see:

```text
++ / -- / assignment
```

appearing multiple times on the **same variable**, stop and check for sequencing before calculating.

---

# 12. Short-Circuit Evaluation

Only important for:

```text
&&
||
```

### AND

```text
FALSE && X
```

→ `X` is **NOT evaluated**.

### OR

```text
TRUE || X
```

→ `X` is **NOT evaluated**.

Example:

```c
int x = 1;

if (x || x++)
```

Since:

```text
x → TRUE
```

we get:

```text
TRUE || x++
```

Therefore:

```text
x++ is NOT executed
```

---

# 13. Precedence vs Associativity vs Evaluation

This distinction is **very important for GATE**.

### Precedence

**Which operator binds stronger?**

```c
a + b * c
```

→

```c
a + (b * c)
```

### Associativity

If operators have **same precedence**, how are they grouped?

```c
a || b || c
```

→

```c
(a || b) || c
```

### Evaluation

Which parts actually execute?

Short-circuiting can prevent evaluation:

```text
TRUE || X
       ↑
   never evaluate
```

---

# 🔥 GATE MASTER TABLE

|Topic|Must remember|
|---|---|
|`* / %`|Higher than `+ -`|
|Arithmetic same precedence|L → R|
|Relational|Result 0 or 1|
|`a < b < c`|`(a < b) < c`|
|`!`|R → L|
|`&&`|L → R|
|`||
|`!`|Logical NOT|
|`~`|Bitwise NOT|
|`&`|Both bits 1|
|`\|`|At least one 1|
|`^`|Different bits → 1|
|`<<`|Left shift|
|`>>`|Right shift|
|`n & 1`|Odd/even|
|`++x`|Change → use|
|`x++`|Use → change|
|Assignment|R → L|
|`?:`|Conditional, R → L|
|Comma|L → R; rightmost value|
|`a++ + a++`|Undefined|
|`FALSE && X`|X skipped|
|`TRUE \| X`|X skipped|

### 🧠 The one workflow to use in GATE

```text
Expression
    ↓
1. Check precedence
    ↓
2. Apply associativity / put brackets
    ↓
3. Evaluate LEFT → RIGHT where applicable
    ↓
4. Check short-circuiting
    ↓
5. Watch ++ / -- and sequence-point traps
```

This is the **whole Lecture 4 in revision form**, without mixing in Lecture 5.