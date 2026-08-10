Absolutely. Here are the **questions/examples actually asked or solved in Lecture 4 PDF**, with their answers and short reasoning. I’m keeping the answers aligned to the PDF rather than adding outside questions.

# Lecture 4 — All Questions & Answers

## 4A — Operators

### Q1. What is the output?

```c
int a = 10, b = 20, c = 30;

if (a < b < c)
    printf("True");
else
    printf("False");
```

### Answer: `True`

Don't treat it as mathematical chaining.

C evaluates:

```c
(a < b) < c
```

First:

```text
10 < 20 → 1
```

Then:

```text
1 < 30 → 1
```

Therefore:

```text
True
```

---

### Q2. What is the output?

```c
int a = 10, b = 20, c = 30;

if (c > b > a)
    printf("True");
else
    printf("False");
```

### Answer: `False`

Evaluation:

```c
(c > b) > a
```

```text
30 > 20 → 1
1 > 10  → 0
```

Therefore:

```text
False
```

---

### Q3. Evaluate:

```c
int a = 9 - 12/3 + 3*2 - 1;
```

### Answer: `10`

First `* /`:

```text
12/3 = 4
3*2  = 6
```

So:

```text
9 - 4 + 6 - 1
```

Then L → R:

```text
9 - 4 = 5
5 + 6 = 11
11 - 1 = 10
```

---

### Q4. Put brackets correctly:

```text
exp1 || exp2 || exp3
```

### Answer

```text
(exp1 || exp2) || exp3
```

Because `||` is left associative.

---

### Q5. Put brackets correctly:

```text
exp1 || exp2 && exp3
```

### Answer

```text
exp1 || (exp2 && exp3)
```

Because:

```text
&& > ||
```

---

### Q6. Put brackets correctly:

```text
exp1 && exp2 || exp3 && exp4
```

### Answer

```text
(exp1 && exp2) || (exp3 && exp4)
```

---

### Q7. Evaluate:

```c
int n = 5;
int x;

x = n++;
printf("%d", x);
printf("%d", n);
```

### Answer

```text
5 6
```

Because post-increment:

```text
x = n++ 

x gets old n → 5
then n becomes → 6
```

---

### Q8. Evaluate:

```c
int n = 5;
int x;

x = ++n;
printf("%d", x);
printf("%d", n);
```

### Answer

```text
6 6
```

Because pre-increment:

```text
n becomes 6
then x gets 6
```

---

### Q9. Are these valid?

```c
++2
++(a + b * 2)
```

### Answer: ❌ Invalid

`++` must operate on a **modifiable variable/lvalue**, not a constant or expression result.

---

# 4B — Bitwise Operators

### Q10. Calculate:

```c
int x = 13;
int y = 25;

z = x & y;
```

Binary:

```text
13 = 0000 1101
25 = 0001 1001
     ---------
&    0000 1001
```

### Answer

```text
z = 9
```

---

### Q11. Why can we use this to check odd/even?

```c
number & 1
```

### Answer

```text
Even → LSB = 0
Odd  → LSB = 1
```

Since:

```text
1 = 0000...0001
```

`number & 1` checks the LSB.

```c
if(number & 1)
    // odd
else
    // even
```

---

### Q12. Calculate:

```text
13 | 25
```

```text
13 = 0000 1101
25 = 0001 1001
     ---------
     0001 1101
```

### Answer

```text
29
```

---

### Q13. Calculate:

```text
13 ^ 25
```

```text
13 = 0000 1101
25 = 0001 1001
     ---------
     0001 0100
```

### Answer

```text
20
```

---

### Q14. Given:

```text
x = 0100 1001 1100 1011
```

Calculate:

```text
x << 3
```

### Answer

```text
0100 1110 0101 1000
```

---

### Q15. Calculate:

```text
x >> 3
```

for:

```text
x = 0100 1001 1100 1011
```

### Answer

```text
0000 1001 0011 1001
```

---

### Q16. What are:

```c
10 << 1
10 >> 1
```

### Answer

```text
10 << 1 = 20
10 >> 1 = 5
```

---

### Q17. How does right shift work for signed and unsigned?

### Answer

```text
Unsigned >> → fill with 0

Signed >> → system dependent
             0 or sign bit
```

---

### Q18. Calculate `~x`:

```text
x = 1001 0110 1100 1011
```

### Answer

Flip every bit:

```text
~x = 0110 1001 0011 0100
```

---

### Q19. What is:

```c
~8
```

### Answer

```text
-9
```

Because in 2's complement:

```text
~x = -x - 1
```

Therefore:

```text
~8 = -8 - 1
   = -9
```

---

### Q20. What is the difference between `!` and `~`?

### Answer

```text
! → logical NOT
~ → bitwise complement
```

Example:

```text
!10 → 0
!0  → 1
```

while `~` flips **every bit**.

---

# 4C — Assignment / Conditional / Comma

### Q21. What does this mean?

```c
x += 3;
```

### Answer

```c
x = x + 3;
```

Similarly:

```text
x -= y → x = x - y
x *= y → x = x * y
x /= y → x = x / y
x %= y → x = x % y
```

---

### Q22. What does this mean?

```c
v op= exp;
```

### Answer

```c
v = v op (exp);
```

Example:

```c
x += y + 1;
```

becomes:

```c
x = x + (y + 1);
```

---

### Q23. What is the correct interpretation?

```c
a = a * n + 1;
```

vs

```c
a *= n + 1;
```

### Answer

```c
a *= n + 1;
```

means:

```c
a = a * (n + 1);
```

NOT:

```c
a = a * n + 1;
```

This is an important compound-assignment trap.

---

### Q24. What happens here?

```c
int a = b = 10;
```

### Answer shown in lecture:

```text
a = 10
b = 10
```

Assignment associates **right → left**.

---

### Q25. Evaluate:

```c
a = 5 < 2 ? 4 : 3;
```

### Answer

Condition:

```text
5 < 2 → FALSE
```

Therefore choose the third expression:

```text
a = 3
```

---

## Q26 — GATE CSE 2008

Given:

```c
a = (x > y) ?
        ((x > z) ? x : z) :
        ((y > z) ? y : z);
```

**Which values of `x, y, z` make `a = 4`?**

Options from the lecture:

```text
A. x = 3, y = 4, z = 2
B. x = 6, y = 5, z = 3
C. x = 6, y = 3, z = 5
D. x = 5, y = 4, z = 5
```

### Answer: **A**

The expression finds the **maximum of x, y and z**.

For A:

```text
x = 3
y = 4
z = 2

max = 4
```

So:

```text
A ✓
```

---

### Q27. Evaluate:

```c
value = (x = 10, y = 5, x + y);
```

### Answer

Comma operator evaluates **left → right**:

```text
x = 10
y = 5
x + y = 15
```

Therefore:

```text
value = 15
```

---

### Q28. Evaluate:

```c
t = x;
x = y;
y = t;
```

If initially:

```text
x = 2
y = 1
t = 3
```

### Answer

Step-by-step:

```text
t = x → t = 2
x = y → x = 1
y = t → y = 2
```

Final:

```text
x = 1
y = 2
t = 2
```

---

# 4C — GATE 2015

### Q29. How many times is `printf` executed?

The lecture's program is essentially:

```c
int i, j, k = 0;

j = 2*3/4 + 2.0/5 + 8/5;

k = --j;

for(i = 0; i < 5; i++)
{
    switch(i+k)
    {
        case 1:
        case 2: printf("%d", i+k);
        case 3: printf("%d", i+k);
        default: printf("%d", i+k);
    }
}
```

### Step 1 — Calculate `j`

```text
2*3/4 = 6/4 = 1       // integer division
2.0/5 = 0.4
8/5   = 1             // integer division
```

Therefore:

```text
j = 1 + 0.4 + 1
  = 2.4
```

Since `j` is `int`:

```text
j = 2
```

Then:

```c
k = --j;
```

so:

```text
j = 1
k = 1
```

### Step 2 — `i+k`

```text
i = 0 → 1
i = 1 → 2
i = 2 → 3
i = 3 → 4
i = 4 → 5
```

Because there are **no `break`s**, switch cases fall through.

Number of executions:

```text
i+k = 1 → 3
i+k = 2 → 3
i+k = 3 → 2
i+k = 4 → 1
i+k = 5 → 1
```

Total:

```text
3 + 3 + 2 + 1 + 1 = 10
```

### ✅ Answer: **10**

---

# 4D — Sequence Points

### Q30. What happens?

```c
a = 5;
x = a++;

printf("%d", x);
printf("%d", a);
```

### Answer

After the semicolon, the increment is complete:

```text
x = 5
a = 6
```

---

### Q31. What happens?

```c
x = a++ + a++;
```

### Answer:

```text
❌ UNDEFINED
```

The same variable `a` is modified multiple times before the required sequencing point.

---

### Q32. What about:

```c
a++ - a++
```

and

```c
a++ * --a
```

### Answer:

```text
Undefined
```

The lecture highlights these as examples where `a` is modified multiple times without appropriate sequencing.

---

### Q33. What about:

```c
i = i++;
```

### Answer:

```text
Undefined
```

The lecture marks it as compiler-dependent/undefined in this context.

---

# 4E — Order / Associativity Example

### Q34. What happens in this `for` loop?

```c
int i, j = 2;

for(i = 0, j >= 0, i <= 5; i++)
{
    printf("%d ", i+j);
    j--;
}
```

The key point demonstrated is that the comma-separated expressions in the `for` condition are evaluated **left → right**, but the **last expression determines the condition's value**.

Here:

```text
i = 0
j >= 0
i <= 5
```

The last expression:

```text
i <= 5
```

controls the loop condition.

---

### Q35. What happens if both conditions are combined with `&&`?

The lecture contrasts this with a condition like:

```c
j >= 0 && i <= 5
```

Now **both conditions must be true**.

```text
j >= 0 → TRUE
i <= 5 → TRUE
```

So the loop continues only while both hold.

---

# 🔥 Final GATE Question List

If you're revising Lecture 4, these are the **must-solve questions** from the PDF:

```text
Q1  → a < b < c
Q2  → c > b > a
Q3  → 9 - 12/3 + 3*2 - 1

Q4  → Bracket exp1 || exp2 || exp3
Q5  → Bracket exp1 || exp2 && exp3
Q6  → Bracket exp1 && exp2 || exp3 && exp4

Q7  → n++
Q8  → ++n
Q9  → illegal ++ operands

Q10 → 13 & 25
Q11 → number & 1
Q12 → 13 | 25
Q13 → 13 ^ 25
Q14 → x << 3
Q15 → x >> 3
Q16 → 10 << 1, 10 >> 1
Q17 → signed vs unsigned >>
Q18 → ~x
Q19 → ~8
Q20 → ! vs ~

Q21 → x += 3
Q22 → v op= exp
Q23 → x *= y + 1
Q24 → int a = b = 10
Q25 → 5 < 2 ? 4 : 3

Q26 → GATE 2008 ternary/max question
Q27 → comma operator
Q28 → sequential assignments

Q29 → GATE 2015 switch + assignment
       Answer = 10

Q30 → a++ across sequence point
Q31 → a++ + a++
Q32 → a++ - a++, a++ * --a
Q33 → i = i++

Q34 → comma expressions in for
Q35 → && condition in for
```

The **highest-priority GATE traps** are:

```text
1. a < b < c
2. Precedence vs associativity
3. ++x vs x++
4. & vs && vs ~ vs !
5. Signed >> vs unsigned >>
6. Compound assignment grouping
7. Ternary nesting
8. Comma operator
9. Sequence-point / undefined expressions
10. Integer + floating-point conversion
11. switch fall-through
```

All of these above are taken from the **Lecture 4 PDF examples/questions**, not an outside question bank.