# 4C — Assignment, Conditional & Comma Operators

## 1. Assignment Operators

Basic:

```c
x = 10;
```

Compound assignment operators:

```text
+=  -=  *=  /=  %=
&=  |=  ^=  <<=
>>=
```

They associate **Right → Left**.

---

## 2. Compound Assignment

General form:

```c
v op= exp;
```

is equivalent to:

```c
v = v op (exp);
```

Examples:

```c
x += y + 1;
```

means:

```c
x = x + (y + 1);
```

Similarly:

```c
x *= y/5;
```

means:

```c
x = x * (y/5);
```

### ⚠️ Important

It is **not**:

```c
x = (x * y) / 5;
```

The RHS expression is evaluated first.

---

## 3. Assignment Associativity

Assignment is **Right → Left**.

```c
a = b = 10;
```

means:

```c
a = (b = 10);
```

So:

```text
a = 10
b = 10
```

---

## 4. Conditional Operator `?:`

Syntax:

```c
condition ? expression1 : expression2;
```

Meaning:

```text
condition TRUE  → expression1
condition FALSE → expression2
```

Example:

```c
x = (a > b) ? a : b;
```

→ stores the larger value in `x`.

**Associativity:** Right → Left.

---

## 5. Comma Operator `,`

Expressions separated by comma are evaluated:

```text
LEFT → RIGHT
```

Example:

```c
x = (a = 5, b = 10, a + b);
```

Execution:

```text
a = 5
b = 10
a + b = 15
```

Therefore:

```text
x = 15
```

**Final value = rightmost expression.**

---

# ⭐ GATE 2015 Question in Lecture

The lecture gives a GATE CSE 2015 question involving:

```c
j = 2 * 3 / 4 + 2.0 / 5 + 8 / 5;
k = --j;
```

Important things being tested:

- integer vs floating-point arithmetic
    
- operator precedence
    
- assignment
    
- pre-decrement
    
- `switch`
    
- fall-through between cases
    

The lecture evaluates the first expression to:

```text
j = 2.4
```

and then:

```text
k = --j
```

giving:

```text
j = 1
k = 1
```

The final answer to the question is **10 executions of `printf`**.

---

# 🔥 4C Short Notes

```text
ASSIGNMENT
=  +=  -=  *=  /=  %=  &=  |=  ^=  <<=  >>=

Associativity → RIGHT → LEFT

x += y
→ x = x + y

x *= y/5
→ x = x * (y/5)


CONDITIONAL
condition ? A : B

TRUE  → A
FALSE → B

Associativity → RIGHT → LEFT


COMMA
A, B, C

Evaluation → LEFT → RIGHT
Final value → C


GATE TRAPS:
• Assignment → R → L
• Compound assignment preserves RHS grouping
• Integer vs floating-point arithmetic
• Don't forget switch fall-through
```

**4C done.**