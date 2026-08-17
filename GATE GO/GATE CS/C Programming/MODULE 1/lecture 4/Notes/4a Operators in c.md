 # Lecture 4A — Operators in C Programming

## 1. What is an Operator?

An **operator** is a symbol that tells C to perform some operation on operands.

Example:

```c
a + b
```

Here:

```text
+  → operator
a,b → operands
```

The lecture introduces **Arithmetic, Relational and Logical operators** in this part.

---

# 2. Arithmetic Operators

|Operator|Operation|
|---|---|
|`+`|Addition|
|`-`|Subtraction|
|`*`|Multiplication|
|`/`|Division|
|`%`|Modulus / remainder|

### Example

```c
10 + 5   // 15
10 - 5   // 5
10 * 5   // 50
10 / 5   // 2
10 % 5   // 0
```

---

# 3. Precedence of Arithmetic Operators

The lecture divides arithmetic operators into two priority groups:

### Higher precedence

```text
*   /   %
```

### Lower precedence

```text
+   -
```

So:

```c
a + b * c
```

is interpreted as:

```c
a + (b * c)
```

NOT:

```c
(a + b) * c
```

---

## Same precedence → Left to Right

The operators:

```text
* / %
```

have the same precedence and are evaluated **left → right**.

Similarly:

```text
+ -
```

are evaluated **left → right**.

### Example from lecture

```c
int a = 9 - 12/3 + 3*2 - 1;
```

### Step 1 — Higher precedence

```text
12 / 3 = 4
3 * 2 = 6
```

So:

```text
9 - 4 + 6 - 1
```

### Step 2 — Lower precedence, left → right

```text
9 - 4 = 5
5 + 6 = 11
11 - 1 = 10
```

Therefore:

```text
a = 10
```

### GATE mental model

```text
First → higher precedence
        (* / %)

Then → lower precedence
       (+ -)

Same level → LEFT → RIGHT
```

---

# 4. Relational Operators

Relational operators are used to **compare values**.

|Operator|Meaning|
|---|---|
|`<`|Less than|
|`<=`|Less than or equal|
|`>`|Greater than|
|`>=`|Greater than or equal|
|`==`|Equal to|
|`!=`|Not equal to|

The lecture states these operators associate **left → right**.

---

# 5. Important GATE Concept — Relational Expressions

A relational expression produces a logical result:

```text
TRUE  → 1
FALSE → 0
```

So:

```c
10 < 20
```

gives:

```text
1
```

while:

```c
30 < 20
```

gives:

```text
0
```

---

# 6. The Famous GATE Trap

Consider:

```c
int a = 10, b = 20, c = 30;

if (a < b < c)
    printf("True");
else
    printf("False");
```

Don't read this like mathematics.

C interprets it according to operator associativity:

```c
(a < b) < c
```

The lecture demonstrates this exact question.

### Step 1

```c
a < b
```

```text
10 < 20
= 1
```

### Step 2

Now the expression has become:

```c
1 < c
```

```text
1 < 30
= 1
```

Therefore:

```text
True
```

### 🔥 Pattern

Whenever you see:

```c
a < b < c
```

**immediately put brackets:**

```c
(a < b) < c
```

It is **not mathematical chaining**.

---

# 7. Another GATE Trap

```c
c > b > a
```

Suppose:

```text
c = 30
b = 20
a = 10
```

C evaluates:

```c
(c > b) > a
```

First:

```text
30 > 20
= 1
```

Then:

```text
1 > 10
= FALSE
```

Therefore the result is:

```text
0
```

The lecture demonstrates this as Question 2.

---

# 8. Logical Operators

The lecture introduces three logical operators:

```text
&&   Logical AND
||   Logical OR
!    Logical NOT
```

---

## Logical AND — `&&`

```c
A && B
```

Result is TRUE only when **both** are true.

|A|B|A && B|
|---|---|---|
|1|1|1|
|1|0|0|
|0|1|0|
|0|0|0|

Example from the lecture:

```c
int x = 1, y = 2;

if (x < 10 && y > 0)
    printf("Welcome");
else
    printf("Awesome");
```

Evaluate:

```text
x < 10 → TRUE
y > 0  → TRUE

TRUE && TRUE
= TRUE
```

So:

```text
Welcome
```

---

# 9. Logical OR — `||`

```c
A || B
```

Result is TRUE if **at least one** is true.

|A|B|A \| B|
|---|---|---|
|1|1|1|
|1|0|1|
|0|1|1|
|0|0|0|

Example:

```text
FALSE || TRUE
= TRUE
```

---

# 10. Logical NOT — `!`

`!` reverses the logical value.

```text
!TRUE  → FALSE
!FALSE → TRUE
```

The lecture lists `!` as **right-to-left associative**.

So:

```c
!!x
```

is grouped as:

```c
!(!x)
```

---

# 11. Logical Operator Associativity

This is important for GATE:

|Operator|Associativity|
|---|---|
|`!`|Right → Left|
|`&&`|Left → Right|
|`||

---

# 12. Combining `&&` and `||`

Consider:

```c
(x < 1 && y > 50) || z < 10
```

Because `&&` has higher precedence than `||`, first group it:

```c
(x < 1 && y > 50) || (z < 10)
```

The lecture explicitly demonstrates this grouping.

### Example

```text
x = 10
y = 20
z = 1
```

Then:

```text
x < 1  → FALSE
y > 50 → FALSE
z < 10 → TRUE
```

So:

```text
(FALSE && FALSE) || TRUE
```

First:

```text
FALSE && FALSE
= FALSE
```

Then:

```text
FALSE || TRUE
= TRUE
```

Result:

```text
TRUE
```

---

# 13. Bracketing Logical Expressions

This is a major part of the lecture.

For:

```c
exp1 || exp2 || exp3
```

because `||` associates left-to-right:

```c
(exp1 || exp2) || exp3
```

The lecture repeatedly practices **putting the brackets correctly**.

For:

```c
exp1 && exp2 || exp3
```

because `&&` has higher precedence:

```c
(exp1 && exp2) || exp3
```

For:

```c
exp1 || exp2 && exp3
```

it becomes:

```c
exp1 || (exp2 && exp3)
```

---

# 14. Increment and Decrement Operators

The lecture then moves to:

```text
++   Increment
--   Decrement
```

There are two forms:

```c
++m    // pre-increment
m++    // post-increment

--m    // pre-decrement
m--    // post-decrement
```

---

# 15. What does `++` actually do?

The lecture states:

```c
++m
```

is equivalent to:

```c
m = m + 1
```

or:

```c
m += 1
```

Similarly:

```c
--m
```

is equivalent to:

```c
m = m - 1
```

or:

```c
m -= 1
```

---

# 16. Post-Increment `m++`

Suppose:

```c
n = 5;
x = n++;
```

The important idea:

```text
USE old value
     ↓
x = 5
     ↓
increment
     ↓
n = 6
```

So finally:

```text
x = 5
n = 6
```

The lecture demonstrates exactly this.

### Mental model

```text
x = n++;
     ↑
  use first
  increment later
```

---

# 17. Pre-Increment `++n`

Suppose:

```c
n = 5;
x = ++n;
```

Now:

```text
increment first
     ↓
n = 6
     ↓
use value
     ↓
x = 6
```

Final:

```text
x = 6
n = 6
```

The lecture demonstrates this comparison directly.

### Mental model

```text
x = ++n;
     ↑
 increment first
 use later
```

---

# 18. Pre vs Post — GATE Table

|Expression|First action|Value used|
|---|---|---|
|`++x`|increment|new value|
|`x++`|use|old value|
|`--x`|decrement|new value|
|`x--`|use|old value|

### Easy mental model

```text
++x → CHANGE → USE
x++ → USE → CHANGE

--x → CHANGE → USE
x-- → USE → CHANGE
```

---

# 19. Illegal Increment Operators

The lecture specifically shows:

```c
++2
++(a + b * 2)
```

as illegal because these are not suitable modifiable operands.

Think:

```text
++ needs something whose stored value can be changed.
```

So:

```c
++x       ✓
++2       ✗
++(a+b)   ✗
```

---

# ⭐ 4A — GATE Short Notes

Write this in your notebook:

```text
LECTURE 4A — OPERATORS IN C

1. Arithmetic
   +  -  *  /  %

   Precedence:
   * / %  >  + -

   Same precedence → L → R

2. Relational
   < <= > >= == !=

   Associativity → L → R

   IMPORTANT:
   a < b < c
   = (a < b) < c
   NOT mathematical chaining.

3. Logical
   !   → NOT
   &&  → AND
   ||  → OR

   !   → R → L
   &&  → L → R
   ||  → L → R

   !TRUE  = FALSE
   !FALSE = TRUE

4. Combining logical operators

   && has higher precedence than ||
   
   A && B || C
   = (A && B) || C

   A || B && C
   = A || (B && C)

5. Increment / Decrement

   ++x → increment then use
   x++ → use then increment

   --x → decrement then use
   x-- → use then decrement

   ++x ≈ x = x + 1
   --x ≈ x = x - 1

6. ++ / -- need a modifiable operand.

   ++x       ✓
   ++2       ✗
   ++(a+b)   ✗
```

**That's 4A only.** We stop here before Bitwise Operators — **4B** starts at the next section.