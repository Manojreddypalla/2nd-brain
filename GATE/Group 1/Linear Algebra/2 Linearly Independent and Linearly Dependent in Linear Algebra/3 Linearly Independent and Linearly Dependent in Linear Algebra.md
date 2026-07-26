# Linear Dependent and Linear Independent Vectors

## Definition

Suppose we have vectors

$$
\mathbf{v}_1,\mathbf{v}_2,\ldots,\mathbf{v}_n
$$

Consider the equation

$$
c_1\mathbf{v}_1+c_2\mathbf{v}_2+\cdots+c_n\mathbf{v}_n=\mathbf{0}
$$

where $c_1,c_2,\ldots,c_n$ are scalars.

---

# Trivial Solution

The solution

$$
c_1=c_2=\cdots=c_n=0
$$

is called the **trivial solution**.

> **Every set of vectors always has the trivial solution.**

---

# Non-Trivial Solution

If **at least one** coefficient is non-zero, then it is called a **non-trivial solution**.

Example:

$$
2\mathbf{v}_1-3\mathbf{v}_2+\mathbf{v}_3=\mathbf{0}
$$

Since the coefficients are not all zero, this is a **non-trivial solution**.

---

# Linearly Independent Vectors

A set of vectors is **linearly independent** if the equation

$$
c_1\mathbf{v}_1+c_2\mathbf{v}_2+\cdots+c_n\mathbf{v}_n=\mathbf{0}
$$

has **only the trivial solution**

$$
c_1=c_2=\cdots=c_n=0.
$$

## Intuition

- Every vector contributes a **new direction**.
- No vector can be written as a linear combination of the remaining vectors.

---

## Example

$$
\mathbf{v}_1=
\begin{bmatrix}
1\\
0
\end{bmatrix},
\qquad
\mathbf{v}_2=
\begin{bmatrix}
0\\
1
\end{bmatrix}
$$

The equation

$$
c_1\mathbf{v}_1+c_2\mathbf{v}_2=\mathbf{0}
$$

has only

$$
c_1=c_2=0.
$$

Therefore,

$$
\boxed{\text{The vectors are linearly independent.}}
$$

---

# Linearly Dependent Vectors

A set of vectors is **linearly dependent** if the equation

$$
c_1\mathbf{v}_1+c_2\mathbf{v}_2+\cdots+c_n\mathbf{v}_n=\mathbf{0}
$$

has **at least one non-trivial solution**.

That is,

$$
(c_1,c_2,\ldots,c_n)\neq(0,0,\ldots,0).
$$

## Intuition

- At least one vector is **redundant**.
- One vector can be expressed as a linear combination of the others.

---

## Example

$$
\mathbf{v}_1=
\begin{bmatrix}
1\\
2
\end{bmatrix},
\qquad
\mathbf{v}_2=
\begin{bmatrix}
2\\
4
\end{bmatrix}
$$

Observe that

$$
2\mathbf{v}_1-\mathbf{v}_2=\mathbf{0}.
$$

Since the coefficients $(2,-1)$ are **not** all zero, the vectors are

$$
\boxed{\text{Linearly Dependent.}}
$$

---

# Geometric Interpretation

## Linearly Independent

Each vector points in a **new direction**.

Example:

- $(1,0)$ and $(0,1)$

These vectors cannot be obtained from one another.

---

## Linearly Dependent

One vector lies along the direction of another.

Example:

$$
(2,4)=2(1,2)
$$

So the second vector does not add a new direction.

---

# Memory Trick

| Condition | Result |
|-----------|--------|
| **Only trivial solution** | ✅ Linearly Independent |
| **Non-trivial solution exists** | ❌ Linearly Dependent |

---

# Important Notes

- Every set of vectors has the **trivial solution**.
- The trivial solution **alone does not imply independence**.
- A set is **linearly independent only if the trivial solution is the only solution**.
- If one vector can be written as a linear combination of the others, the set is **linearly dependent**.

---

# One-Line Definitions

### Linearly Independent

> No vector can be written as a linear combination of the remaining vectors.

### Linearly Dependent

> At least one vector can be written as a linear combination of the remaining vectors.

---

# GATE Exam Shortcut

$$
\boxed{
\begin{aligned}
\text{Only Trivial Solution} &\Longrightarrow \text{Linearly Independent} \\
\text{Non-Trivial Solution Exists} &\Longrightarrow \text{Linearly Dependent}
\end{aligned}
}
$$