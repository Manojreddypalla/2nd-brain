# Lecture 4B — Bitwise Operators in C

**Scope:** Only the Bitwise Operators section from your lecture. It starts at page 34 and ends before Assignment Operators begin on page 55.

---

## 1. What are Bitwise Operators?

Bitwise operators work on the **individual bits** of an integer.

The lecture covers:

```text
&    Bitwise AND
|    Bitwise OR
^    Bitwise XOR
<<   Left Shift
>>   Right Shift
~    One's Complement
```

### Core idea

Suppose:

```text
x = 13
```

In binary:

```text
13 = 0000 0000 0000 1101
```

Bitwise operators operate directly on these `0`s and `1`s.

---

# 2. Bitwise AND — `&`

### Rule

```text
1 & 1 = 1
Everything else = 0
```

|x|y|`x & y`|
|---|---|---|
|1|1|1|
|1|0|0|
|0|1|0|
|0|0|0|

### Example from lecture

```c
x = 13;
y = 25;

z = x & y;
```

Binary:

```text
13 = 0000 0000 0000 1101
25 = 0000 0000 0001 1001
     ---------------------
&    0000 0000 0000 1001
```

```text
0000 1001 = 9
```

Therefore:

```c
z = 9;
```

### Mental model

`&` **keeps a bit only when both numbers have `1` there.**

---

# 3. Important Use of `&` — Odd / Even

The lecture shows:

```c
int test = 1;

if(number & test)
    printf("Number is odd");
else
    printf("Number is even");
```

Why does this work?

```text
test = 1
     = 0000 0001
```

The last bit is the **least significant bit (LSB)**.

For integers:

```text
Even → LSB = 0
Odd  → LSB = 1
```

Therefore:

```text
number & 1
```

checks whether the last bit is `1`.

### GATE pattern

```c
number & 1
```

→ **odd/even check**

---

# 4. Bitwise OR — `|`

### Rule

```text
0 | 0 = 0
Everything else = 1
```

| x | y | `x | y` |  
|---|---|---|  
| 1 | 1 | 1 |  
| 1 | 0 | 1 |  
| 0 | 1 | 1 |  
| 0 | 0 | 0 |

### Example from lecture

```text
13 = 0000 0000 0000 1101
25 = 0000 0000 0001 1001
     ---------------------
     0000 0000 0001 1101
```

```text
0001 1101 = 29
```

Therefore:

```text
13 | 25 = 29
```

### Mental model

`|` **sets a bit to 1 if either operand has 1 there.**

---

# 5. Bitwise XOR — `^`

XOR = **Exclusive OR**.

### Rule

```text
Different → 1
Same     → 0
```

|x|y|`x ^ y`|
|---|---|---|
|1|1|0|
|1|0|1|
|0|1|1|
|0|0|0|

### Example from lecture

```text
13 = 0000 0000 0000 1101
25 = 0000 0000 0001 1001
     ---------------------
     0000 0000 0001 0100
```

```text
0001 0100 = 20
```

Therefore:

```text
13 ^ 25 = 20
```

### Mental model

```text
& → both 1
| → at least one 1
^ → exactly one 1
```

That's a very useful way to remember the three.

---

# 6. Left Shift — `<<`

Syntax:

```c
x << n
```

means:

> Shift the bits of `x` toward the **left** by `n` positions.

The lecture defines:

```text
Left shift  → op << n
Right shift → op >> n
```

---

## Example

Suppose:

```text
x = 0100 1001 1100 1011
```

Then:

```c
x << 3
```

The bits move three positions left:

```text
0100 1001 1100 1011
       << 3
--------------------
0100 1110 0101 1000
```

The lecture demonstrates this exact shift.

### Visualization

```text
Before:

0100 1001 1100 1011
                  ↑
              bits

After << 3:

0100 1110 0101 1000
               ↑
        shifted left
```

Bits that move beyond the available width are lost.

---

# 7. Right Shift — `>>`

Syntax:

```c
x >> n
```

means:

> Shift bits of `x` toward the **right** by `n` positions.

Example from the lecture:

```text
x = 0100 1001 1100 1011
```

Then:

```c
x >> 3
```

gives:

```text
0000 1001 0011 1001
```

So conceptually:

```text
0100 1001 1100 1011
>> 3
0000 1001 0011 1001
```

---

# 8. Unsigned vs Signed Right Shift

This is an important point from the lecture.

### Unsigned right shift

The left side is filled with:

```text
0
```

So:

```text
Unsigned >> → fill zeros
```

### Signed right shift

The lecture states that this **depends on the system**:

```text
Signed >> → zeros OR sign bit
```

### GATE note

Don't blindly assume:

```text
signed x >> n
```

always fills with `0`.

The lecture explicitly flags this as system-dependent.

---

# 9. Shift Examples with Decimal Numbers

The lecture gives:

```c
int x, y = 10;

x = y << 1;
```

Result:

```text
10 << 1 = 20
```

and:

```c
x = y >> 1;
```

Result shown:

```text
10 >> 1 = 5
```

For the positive examples shown in the lecture:

```text
x << 1 → roughly ×2
x >> 1 → roughly ÷2
```

But for GATE, **understand the bit movement first** rather than memorizing multiplication/division.

---

# 10. One's Complement — `~`

Syntax:

```c
~x
```

It operates at the **bit level**.

Every bit is flipped:

```text
0 → 1
1 → 0
```

### Example from lecture

```text
x  = 1001 0110 1100 1011

~x = 0110 1001 0011 0100
```

Every single bit changes.

---

# 11. `~` vs `!` — VERY IMPORTANT

Don't confuse these:

```c
~x
!x
```

They are completely different.

### `~` — Bitwise NOT

Works on **every bit**:

```text
~1010
= 0101
```

### `!` — Logical NOT

Works on the **truth value**:

```text
!0     → 1
!10    → 0
!100   → 0
```

The lecture explicitly contrasts these ideas.

### Remember

```text
~ → bit level
! → logical level
```

---

# 12. Example: `~8`

The lecture uses:

```c
int x = 8;

printf("%d", ~x);
```

For the representation used in the lecture:

```text
8  = 0000 0000 0000 1000

~8 = 1111 1111 1111 0111
```

This represents:

```text
-9
```

So:

```text
~8 = -9
```

---

# 13. Important Relationship

The lecture gives the relationship:

```text
-x = ~x + 1
```

for the **2's-complement system**.

Therefore:

```text
~x = -x - 1
```

### Example

```text
x = 8

~8 = -8 - 1
   = -9
```

Exactly what we obtained above.

---

# 14. Why `~0` Is Interesting

The lecture also demonstrates:

```text
~0
```

Start with:

```text
0 = 0000 0000 ...
```

Complement every bit:

```text
~0 = 1111 1111 ...
```

So in the representation being used:

```text
~0 = -1
```

The lecture's page shows the decimal `0` being converted to an all-ones binary representation after `~`.

---

# ⭐ 4B — GATE Short Notes

Write this version in your notebook:

```text
LECTURE 4B — BITWISE OPERATORS

Bitwise operators work on individual bits.

&  → Bitwise AND
|  → Bitwise OR
^  → Bitwise XOR
<< → Left Shift
>> → Right Shift
~  → One's Complement


1. AND (&)

1 & 1 = 1
otherwise = 0

Common:
number & 1
→ odd/even check

LSB:
even → 0
odd  → 1


2. OR (|)

0 | 0 = 0
otherwise = 1


3. XOR (^)

same bits      → 0
different bits → 1


4. Left Shift (<<)

x << n
→ shift bits left by n positions


5. Right Shift (>>)

x >> n
→ shift bits right by n positions

Unsigned right shift:
→ fill 0s

Signed right shift:
→ system-dependent
→ 0 or sign bit


6. One's Complement (~)

0 → 1
1 → 0

~x → complement every bit

IMPORTANT:

!x → logical NOT
~x → bitwise NOT


2's complement relation:

-x = ~x + 1

therefore:

~x = -x - 1

Example:
~8 = -9
```

### 🧠 One mental picture

```text
        BITWISE
           │
 ┌─────────┼─────────┐
 │         │         │
 &         |         ^
both       either    different
1          1         bits → 1

           │
      ┌────┴────┐
      │         │
     <<        >>
   left       right
   shift      shift

           │
           ~
      flip every bit
```

**4B ends here.** The next section, **4C — Assignment Operators**, starts on page 55.