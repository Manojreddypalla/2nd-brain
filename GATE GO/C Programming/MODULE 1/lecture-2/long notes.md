Yep. Let's make this the **full L2 study sheet**, not the tiny revision sheet.

One important boundary: I could retrieve a related **L1 homework** from your Library, but the actual L2 lecture transcripts/annotated notes for all five sub-lectures were not surfaced by file search. So I **won't invent unseen lecture examples or claim they came from the PDF**. The notes below cover the L2 material we've actually established from your lecture/screenshots and the questions you've asked me so far. The retrieved homework confirms the lecture's 2's-complement practice style, including 8-bit/16-bit conversions.

# C Programming — L2

## Signed/Unsigned Numbers + Integer Promotion + 2's Complement + Type Conversion

---

# 0. The Big Picture

L2 is basically teaching you to track **what happens to a value as it moves through different C types**.

The mental pipeline is:

```text
Value
  ↓
Representation in bits
  ↓
Type
  ↓
Integer promotion
  ↓
Operation
  ↓
Type conversion / assignment
  ↓
Final representation
  ↓
Interpretation / printf
```

For GATE, **don't just calculate the number**.

Track:

> **Type → promotion → operation → conversion → final type**

That is the main skill.

---

# 1. Signed and Unsigned Numbers

A computer stores values as **bits**.

Suppose we have 8 bits:

```text
00000000
```

There are:

```text
2⁸ = 256
```

possible bit patterns.

The important question is:

> **How are those 256 patterns interpreted?**

---

## 1.1 Unsigned

All bits contribute positively.

For `k` bits:

```text
Minimum = 0

Maximum = 2ᵏ - 1
```

For 8 bits:

```text
0 → 255
```

because:

```text
2⁸ - 1
= 256 - 1
= 255
```

---

## 1.2 Signed 2's Complement

For `k` bits:

```text
Minimum = -2^(k-1)

Maximum = 2^(k-1) - 1
```

For 8 bits:

```text
-128 → +127
```

So:

```text
Unsigned 8-bit:
0 → 255

Signed 8-bit:
-128 → +127
```

### ⭐ GATE IMP CORNER

Whenever GATE says:

> "An n-bit integer..."

**STOP.**

Ask:

```text
Signed or unsigned?
```

If signed, check whether the question assumes **2's complement**.

---

# 2. Why Signed Range Is Asymmetric

For 8-bit signed 2's complement:

```text
10000000 → -128
11111111 → -1
00000000 → 0
01111111 → +127
```

There are 256 patterns total.

```text
Negative:
-128 ... -1
        ↓
       128 values

Non-negative:
0 ... 127
    ↓
   128 values
```

So the range becomes:

```text
-128 → +127
```

rather than:

```text
-127 → +127
```

The extra negative value is:

```text
10000000 = -128
```

---

# 3. Same Bits, Different Meaning ⭐⭐⭐

This is perhaps the most important intuition.

Take:

```text
11111111
```

As unsigned:

```text
128 + 64 + 32 + 16 + 8 + 4 + 2 + 1
= 255
```

As signed 8-bit 2's complement:

```text
11111111 = -1
```

Therefore:

```text
11111111

unsigned → 255
signed   → -1
```

### Mental model

Bits themselves don't say:

> "I am -1."

The **type/interpretation** tells us what those bits mean.

---

# 4. 2's Complement

To represent a negative number:

```text
1. Write positive magnitude in binary
2. Invert every bit
3. Add 1
```

---

## Example: `-5` in 8 bits

First:

```text
+5 = 00000101
```

Invert:

```text
11111010
```

Add 1:

```text
11111010
+       1
---------
11111011
```

Therefore:

```text
-5 = 11111011
```

---

# 5. Finding Decimal from Negative 2's Complement

Suppose:

```text
11111011
```

MSB is `1`, therefore it is negative.

Take 2's complement.

### Invert

```text
11111011
↓
00000100
```

### Add 1

```text
00000100
+       1
---------
00000101
```

`00000101 = 5`.

Therefore:

```text
11111011 = -5
```

---

# 6. The 2's Complement Shortcut

For an `n`-bit signed number, you can calculate directly using weights.

For 8 bits:

```text
b7 b6 b5 b4 b3 b2 b1 b0
```

Weights:

```text
-128 64 32 16 8 4 2 1
```

Notice:

```text
MSB weight = -2⁷
```

while the remaining weights are positive.

Example:

```text
10110000
```

Calculate:

```text
1(-128)
+ 0(64)
+ 1(32)
+ 1(16)
+ 0(8)
+ 0(4)
+ 0(2)
+ 0(1)

= -128 + 32 + 16
= -80
```

Therefore:

```text
10110000 = -80
```

---

# 7. Integer Promotion ⭐⭐⭐

Now we reach the most important C-specific concept.

Suppose:

```c
char a = 10;
char b = 20;
```

and:

```c
a + b
```

You might think:

```text
char + char
   ↓
char
```

But C generally performs **integer promotion** first.

```text
char a
   ↓
int

char b
   ↓
int

int + int
   ↓
int
```

So:

```c
a + b
```

is effectively:

```c
(int)a + (int)b
```

and the result is an `int`.

---

# 8. Which Types Undergo Integer Promotion?

The important small integer types are:

```text
_Bool
char
signed char
unsigned char
short
unsigned short
```

They can undergo integer promotion when required by an expression/context.

Usually:

```text
small integer type
       ↓
      int
```

provided `int` can represent all values of the original type.

Otherwise:

```text
unsigned int
```

may be used.

### GATE focus

For the typical questions you're seeing:

```text
char / signed char / unsigned char / short
                 ↓
               int
```

is the pattern you should recognize first.

---

# 9. Why Does Integer Promotion Exist?

The easiest mental model:

```text
char/short
   ↓
small integer
   ↓
C promotes it
   ↓
int-sized arithmetic
```

So C doesn't normally perform arithmetic directly in `char`.

This is extremely important when the result is later assigned back to a smaller type.

---

# 10. Your `signed char` Example ⭐⭐⭐

The screenshot you showed:

```c
signed char a = 30, b = 40;
signed char d = a * b;

printf("%d", d);
printf("%d", a * b);
```

Let's solve it exactly.

---

## Step 1 — Initial values

```text
a = 30
b = 40
```

Both are:

```text
signed char
```

Assume 8-bit `signed char`.

---

## Step 2 — Multiplication

```c
a * b
```

Integer promotion happens:

```text
a: signed char → int
b: signed char → int
```

So:

```text
int 30 × int 40
```

Therefore:

```text
1200
```

And importantly:

```text
a * b
```

has type:

```text
int
```

---

# 11. First Important Difference

Consider:

```c
signed char d = a * b;
```

We have:

```text
a * b
↓
int 1200
↓
signed char d
```

Now `1200` has to be converted into `signed char`.

But an 8-bit signed char can only represent:

```text
-128 → +127
```

So `1200` cannot fit.

---

# 12. What Happens to 1200?

Convert:

```text
1200
```

to binary:

```text
1200 = 0000 0100 1011 0000
```

8-bit destination:

```text
0000 0100 1011 0000
                 ↓
        lower 8 bits
                 ↓
            1011 0000
```

So `d` gets the 8-bit representation:

```text
10110000
```

---

# 13. Interpret `10110000`

Since MSB is `1`:

```text
10110000
```

is negative under signed 2's complement.

Using weights:

```text
-128 + 32 + 16
= -80
```

So:

```text
d = -80
```

---

# 14. First `printf`

```c
printf("%d", d);
```

`d` is `signed char`.

Before being passed as an argument, it undergoes integer promotion:

```text
signed char -80
       ↓
integer promotion
       ↓
int -80
       ↓
%d
```

Output:

```text
-80
```

---

# 15. Second `printf`

Now:

```c
printf("%d", a * b);
```

Remember:

```text
a * b
```

was already promoted:

```text
a → int 30
b → int 40

30 × 40
= 1200
```

There is **no `signed char` assignment** here.

Therefore:

```text
a * b → int 1200
```

Output:

```text
1200
```

---

# 🔥 Compare These Two

```c
signed char d = a * b;

printf("%d", d);
```

versus:

```c
printf("%d", a * b);
```

### First

```text
char × char
    ↓
int × int
    ↓
1200
    ↓
convert to signed char
    ↓
10110000
    ↓
-80
```

### Second

```text
char × char
    ↓
int × int
    ↓
1200
    ↓
%d
    ↓
1200
```

### GATE lesson

> **An expression's result can be different depending on whether you store it in a smaller type before using it.**

---

# 16. `%d` and `%u`

You asked about this earlier.

```text
%d → signed decimal integer
%u → unsigned decimal integer
```

Example:

```c
int x = -1;
unsigned int u = 10;
```

Correct matching:

```c
printf("%d", x);
printf("%u", u);
```

---

# 17. Your `-1` Example ⭐⭐⭐

```c
int x = -1;
unsigned int u = x;
```

Assume 32-bit `int`.

`x` is:

```text
-1
```

Its 32-bit 2's-complement representation is:

```text
11111111 11111111 11111111 11111111
```

Now conversion to unsigned.

Unsigned range:

```text
0 → 2³² - 1
```

The bit pattern is interpreted as:

```text
4294967295
```

Therefore:

```text
x = -1
u = 4294967295
```

---

# 18. Why?

The clean mathematical rule for conversion to an unsigned `k`-bit type is:

```text
result = value mod 2ᵏ
```

So:

```text
-1 mod 2³²
```

is:

```text
2³² - 1
```

which equals:

```text
4294967295
```

---

# 19. Same Bits Again

```text
x:
11111111111111111111111111111111
```

Interpret as signed:

```text
-1
```

Interpret as unsigned:

```text
4294967295
```

Same bits.

Different interpretation.

---

# 20. Important `printf` Warning

Don't learn this as:

```c
printf("%d", u);
```

being a valid way to print an unsigned variable.

Correct matching is:

```text
int          → %d
unsigned int → %u
```

So:

```c
printf("%d", x);   // correct if x is int
printf("%u", u);   // correct if u is unsigned int
```

A mismatched `printf` format and argument type is **undefined behavior in standard C**.

### ⭐ GATE IMP

If GATE deliberately gives:

```c
unsigned int x;
printf("%d", x);
```

don't blindly calculate what your particular compiler happens to print.

Think:

> **Format specifier doesn't match argument type → undefined behavior.**

---

# 21. Your `signed char g = 100` Example

```c
#include <stdio.h>

int main() {
    signed char g = 100;

    printf("%d", g);

    return 0;
}
```

Why does `%d` work?

Because:

```text
signed char
    ↓
integer promotion
    ↓
int
    ↓
%d
```

So output:

```text
100
```

This is a great simple example of integer promotion.

---

# 22. Type Conversion

Now distinguish:

### Promotion

Small integer → larger integer type used in expression.

```text
char → int
```

### Conversion

One type is converted into another type.

For example:

```c
int x = 100;
char y = x;
```

Conceptually:

```text
int
 ↓
char
```

---

# 23. Widening vs Narrowing

### Widening

Smaller type → larger type.

```text
char → int
```

Generally, more room is available.

### Narrowing

Larger type → smaller type.

```text
int → char
```

The destination may not be able to represent the value.

Example:

```c
int x = 1200;
signed char y = x;
```

The value doesn't fit in:

```text
-128 → +127
```

So the result can differ from the original value.

---

# 24. Explicit Type Conversion / Cast

You can explicitly request conversion:

```c
int x = 10;
float y = (float)x;
```

Here:

```text
(int) → (float)
```

The syntax:

```c
(type) expression
```

is a cast.

Example:

```c
float x = 10.8;
int y = (int)x;
```

The conversion gives an integer value corresponding to `10.8`, i.e. `10` for this conversion.

---

# 25. Implicit Conversion

C can also convert automatically.

```c
int x = 10;
double y = x;
```

Conceptually:

```text
int 10
  ↓
double 10.0
```

No explicit cast was written.

---

# 26. Conversion vs Promotion

This distinction is important.

### Promotion

A special conversion that happens automatically in certain expressions/contexts.

```text
char → int
```

Example:

```c
char a = 10;
char b = 20;

a + b
```

becomes conceptually:

```text
int a + int b
```

### Type conversion

Broader concept:

```text
int → char
int → unsigned
int → float
float → int
etc.
```

---

# 27. Sign Extension

Suppose we have:

```text
8-bit signed value
```

and want to represent it using more bits.

For signed 2's complement, we **copy the sign bit**.

Example positive:

```text
01010101
```

becomes:

```text
00000000 01010101
```

Example negative:

```text
10110100
```

becomes:

```text
11111111 10110100
```

Why?

Because the numerical value should remain the same.

### ⭐ GATE IMP

```text
MSB = 0
→ add 0s

MSB = 1
→ add 1s
```

That's **sign extension**.

---

# 28. Zero Extension

For an unsigned number, extra bits are filled with zero.

Example:

```text
8-bit:

10110100
```

becomes:

```text
00000000 10110100
```

So:

```text
Unsigned → zero extension
Signed   → sign extension
```

---

# 29. Why Sign Extension Works

Take:

```text
10110100
```

Assume it is signed.

Its value is:

```text
-128 + 32 + 16 + 4
= -76
```

Now extend:

```text
11111111 10110100
```

The extra `1`s preserve the negative value.

So:

```text
10110100 = -76
```

and:

```text
11111111 10110100 = -76
```

The representation got wider, but the value didn't change.

---

# 30. A Complete GATE-Style Chain

Consider:

```c
signed char a = 30;
signed char b = 40;

signed char c = a * b;
```

Think:

### Stage 1

```text
a = signed char
b = signed char
```

### Stage 2 — Promotion

```text
a → int
b → int
```

### Stage 3 — Operation

```text
30 × 40 = 1200
```

### Stage 4 — Assignment

```text
int 1200
   ↓
signed char
```

### Stage 5 — Representation

Assuming 8-bit:

```text
1200
= 00000100 10110000

lower 8 bits:
10110000
```

### Stage 6 — Interpretation

```text
10110000
= -80
```

### Stage 7 — Later use

If passed to `printf`:

```text
signed char -80
      ↓
integer promotion
      ↓
int -80
```

This is the exact kind of chain you should be able to perform mentally.

---

# Questions & Answers

## Q1. What is integer promotion?

**Answer:**

Conversion of certain smaller integer types such as `char` and `short` to `int` or `unsigned int` before certain operations/contexts.

---

## Q2. Why does `char + char` usually produce an `int` result?

Because the operands undergo integer promotion before arithmetic.

```text
char + char
   ↓
int + int
   ↓
int
```

---

## Q3. What is the range of an 8-bit unsigned integer?

```text
0 → 2⁸ - 1
= 0 → 255
```

---

## Q4. What is the range of an 8-bit signed 2's-complement integer?

```text
-2⁷ → 2⁷ - 1

= -128 → +127
```

---

## Q5. What is the 8-bit 2's-complement representation of `-5`?

```text
+5:
00000101

invert:
11111010

+1:
11111011
```

Answer:

```text
11111011
```

---

## Q6. What is `11111111` as an unsigned 8-bit value?

```text
255
```

---

## Q7. What is `11111111` as signed 8-bit 2's complement?

```text
-1
```

---

## Q8. Why can the same bits represent `255` and `-1`?

Because the **interpretation/type is different**.

```text
unsigned → 255
signed 2's complement → -1
```

---

## Q9. What does `%d` mean?

```text
%d → signed decimal integer
```

---

## Q10. What does `%u` mean?

```text
%u → unsigned decimal integer
```

---

## Q11. Why does this work?

```c
signed char g = 100;
printf("%d", g);
```

Because `g` undergoes integer promotion:

```text
signed char → int
```

and `%d` expects a signed integer argument of the appropriate type.

---

## Q12. What happens here?

```c
signed char a = 30;
signed char b = 40;

signed char d = a * b;
```

First:

```text
a → int
b → int
```

Then:

```text
30 × 40 = 1200
```

Then:

```text
1200 → signed char
```

Assuming 8-bit 2's-complement representation:

```text
1200 → 10110000 → -80
```

So:

```text
d = -80
```

---

## Q13. What does this print?

```c
signed char a = 30;
signed char b = 40;

signed char d = a * b;

printf("%d", d);
```

Answer:

```text
-80
```

under the 8-bit signed-char/two's-complement assumptions used in the lecture example.

---

## Q14. What does this print?

```c
signed char a = 30;
signed char b = 40;

printf("%d", a * b);
```

Answer:

```text
1200
```

Why?

Because:

```text
a → int
b → int

int × int
   ↓
1200
```

No narrowing to `signed char` occurs.

---

## Q15. Why are the answers in Q13 and Q14 different?

Because:

```text
Q13:
a*b → 1200 → signed char → -80

Q14:
a*b → int 1200
```

The assignment to `signed char` causes the narrowing conversion.

---

## Q16. What happens here?

```c
int x = -1;
unsigned int u = x;
```

Assuming 32-bit integers:

```text
u = 2³² - 1
  = 4294967295
```

---

## Q17. Why does `-1` become `4294967295`?

Because conversion to unsigned uses modulo arithmetic:

```text
-1 mod 2³²
= 2³² - 1
= 4294967295
```

---

## Q18. What is sign extension?

When increasing the bit width of a signed number, copy the original MSB.

```text
0 → add 0s
1 → add 1s
```

Example:

```text
10110000
```

becomes:

```text
11111111 10110000
```

---

## Q19. What is zero extension?

For unsigned values, add zeros on the left.

```text
10110000
```

becomes:

```text
00000000 10110000
```

---

## Q20. What is narrowing conversion?

Converting from a type with a larger range/representation to a smaller type.

Example:

```c
int x = 1200;
signed char y = x;
```

The destination may not be able to represent `1200`.

---

# 🔥 GATE Practice Set

Try these **without looking at the answers first**.

---

### Q1

For an 8-bit unsigned integer, what is the maximum value?

**Answer:**

```text
2⁸ - 1 = 255
```

---

### Q2

For an 8-bit signed 2's-complement integer, what is the minimum value?

**Answer:**

```text
-2⁷ = -128
```

---

### Q3

Interpret:

```text
11111111
```

as:

1. unsigned
    
2. signed 2's complement
    

**Answer:**

```text
unsigned → 255
signed   → -1
```

---

### Q4

What is:

```text
10110000
```

as an 8-bit signed 2's-complement number?

**Answer:**

```text
-128 + 32 + 16
= -80
```

---

### Q5

What is the 8-bit 2's-complement representation of `-23`?

First:

```text
23 = 00010111
```

Invert:

```text
11101000
```

Add 1:

```text
11101001
```

Answer:

```text
11101001
```

This is also consistent with the retrieved homework's 8-bit `-23` exercise style.

---

### Q6

What is the 32-bit 2's-complement representation of `-23`?

Start with:

```text
11111111 11111111 11111111 11101001
```

Answer:

```text
11111111 11111111 11111111 11101001
```

The corresponding 32-bit exercise also appears in the retrieved homework.

---

### Q7

```c
char a = 10;
char b = 20;

printf("%d", a + b);
```

What is the result?

**Answer:**

```text
30
```

Reason:

```text
char → int
char → int

10 + 20 = 30
```

---

### Q8

```c
signed char a = 30;
signed char b = 40;

signed char c = a * b;

printf("%d", c);
```

Assuming 8-bit signed char, what is the output?

**Answer:**

```text
-80
```

---

### Q9

```c
signed char a = 30;
signed char b = 40;

printf("%d", a * b);
```

Output?

**Answer:**

```text
1200
```

---

### Q10

Why do Q8 and Q9 differ?

**Answer:**

Q8:

```text
a*b → int 1200
     ↓
signed char
     ↓
10110000
     ↓
-80
```

Q9:

```text
a*b → int 1200
```

No narrowing conversion occurs.

---

### Q11

```c
int x = -1;
unsigned int u = x;
```

Assuming 32-bit integers, what is `u`?

**Answer:**

```text
4294967295
```

---

### Q12

What is the core reason?

**Answer:**

Same bit pattern, different interpretation:

```text
11111111...1111

signed   → -1
unsigned → 4294967295
```

---

# 🧠 Final GATE Attack Sheet

When you see a C question from this L2, use this exact procedure:

```text
STEP 1
Identify every variable's type.

STEP 2
Check signed vs unsigned.

STEP 3
Check number of bits if given/assumed.

STEP 4
Look for integer promotion.

STEP 5
Determine the type in which the operation occurs.

STEP 6
Calculate the result.

STEP 7
Check whether the result is assigned to a
smaller/different type.

STEP 8
If narrowing happens, determine the resulting
bit representation/interpretation.

STEP 9
If printf appears, check the format specifier
against the argument type.

STEP 10
Only THEN determine the output.
```

### The pattern in one line:

```text
TYPE
 ↓
PROMOTION
 ↓
OPERATION
 ↓
CONVERSION
 ↓
REPRESENTATION
 ↓
INTERPRETATION
 ↓
OUTPUT
```

And the **three things I would absolutely not forget from L2** are:

> **1. Same bits can mean different values.**  
> **2. `char`/`short` can become `int` before arithmetic.**  
> **3. Always track the type before calculating the output.**

Those three ideas are the backbone of this entire L2.