# 4E — Short-Circuiting, Order of Evaluation & Associativity

## 1. Short-Circuit Evaluation

Only applies to **logical operators** `&&` and `||`.

### `&&`

```c
A && B
```

If `A` is **false**, `B` is **NOT evaluated**.

```text
FALSE && anything
→ FALSE
→ RHS skipped
```

### `||`

```c
A || B
```

If `A` is **true**, `B` is **NOT evaluated**.

```text
TRUE || anything
→ TRUE
→ RHS skipped
```

---

## 2. Classic GATE Trap

```c
int a = 1, b = 1, c = 1;

if (a == b || c++)
    printf("%d", c);
```

Evaluate from **left → right**:

```text
a == b
1 == 1 → TRUE
```

Therefore:

```text
TRUE || c++
```

Since `||` short-circuits:

```text
c++ → NOT evaluated
```

So:

```text
c = 1
```

### Remember

```text
A || B
If A = TRUE → don't evaluate B

A && B
If A = FALSE → don't evaluate B
```

---

# 3. Order of Evaluation

For logical `&&` and `||`, evaluation is **left → right**.

But don't confuse:

```text
Associativity
vs
Order of evaluation
```

For GATE questions, **first determine the grouping using precedence/associativity**, then evaluate according to the rules applicable to the operators.

---

## 4. Precedence vs Associativity vs Evaluation

### Precedence

Answers:

> **Which operator gets grouped first?**

Example:

```c
a + b * c
```

```text
a + (b * c)
```

### Associativity

Answers:

> **If operators have the same precedence, how are they grouped?**

Example:

```c
a || b || c
```

```text
(a || b) || c
```

because `||` associates **left → right**.

### Order of evaluation

Answers:

> **Which operand/expression is actually evaluated first?**

This is where **short-circuiting** becomes important.

---

# 5. How to Solve GATE Expressions

The lecture gives this approach:

```text
STEP 1 → Put brackets using precedence/associativity

STEP 2 → Evaluate from left → right

STEP 3 → Check short-circuiting

        TRUE  || X → X not evaluated
        FALSE && X → X not evaluated
```

---

# ⭐ 4E — GATE Short Notes

```text
SHORT-CIRCUITING

&& :
FALSE && X → X NOT evaluated

|| :
TRUE || X → X NOT evaluated


IMPORTANT:
Logical && and || → evaluate L → R


Example:

a == b || c++

If a == b is TRUE:
→ c++ is NOT executed.


PRECEDENCE:
Determines grouping.

ASSOCIATIVITY:
Determines grouping when same precedence.

ORDER OF EVALUATION:
Determines what actually gets evaluated.


GATE METHOD:

1. Put brackets according to precedence +
   associativity.

2. Evaluate L → R.

3. Check short-circuiting.

   TRUE || X  → skip X
   FALSE && X → skip X
```

### 🔥 Most important pattern

```text
TRUE || anything
       ↓
   DON'T TOUCH RHS


FALSE && anything
       ↓
   DON'T TOUCH RHS
```

That's **4E done**.