# 🎯 GATE Corner

## Keywords to Remember

- **Linearly Independent**
  - Only trivial solution exists.
  - No vector is a linear combination of the others.
  - Every vector adds a new direction.

- **Linearly Dependent**
  - At least one non-trivial solution exists.
  - One vector is a linear combination of the others.
  - At least one vector is redundant.

---

## Quick Tests

### Test 1: Using the Definition

Solve

$$
c_1\mathbf{v}_1+c_2\mathbf{v}_2+\cdots+c_n\mathbf{v}_n=\mathbf{0}
$$

- Only trivial solution → **Independent**
- Non-trivial solution → **Dependent**

---

### Test 2: Check Multiples

If

$$
\mathbf{v}_2=k\mathbf{v}_1
$$

for some scalar $k$,

then the vectors are **Linearly Dependent**.

---

### Test 3: Linear Combination

If one vector can be written as

$$
\mathbf{v}_3=a\mathbf{v}_1+b\mathbf{v}_2,
$$

then the set is **Linearly Dependent**.

---

## Common GATE Traps

❌ Every set has a trivial solution.

> Having a trivial solution **does not** imply independence.

✅ Correct Statement

> A set of vectors is **linearly independent if and only if the trivial solution is the only solution.**

---

## Frequently Asked GATE Statements

| Statement | True/False |
|-----------|------------|
| Every linearly independent set has only the trivial solution. | ✅ True |
| Every linearly dependent set has a non-trivial solution. | ✅ True |
| Every set of vectors has a trivial solution. | ✅ True |
| Trivial solution implies linear independence. | ❌ False |
| If one vector is a linear combination of others, the set is dependent. | ✅ True |
| If vectors are independent, none can be written as a combination of the others. | ✅ True |

---

## Memory Formula

$$
\boxed{
\begin{aligned}
\text{Only Trivial Solution} &\iff \text{Linearly Independent} \\
\text{Non-Trivial Solution Exists} &\iff \text{Linearly Dependent}
\end{aligned}
}
$$

---

## Previous Year GATE Pattern

GATE commonly asks:
- Conceptual True/False questions.
- Identify whether a given set of vectors is independent or dependent.
- Determine the value of a parameter for independence.
- Find the rank or basis using linear independence.
- Check if one vector is a linear combination of others.