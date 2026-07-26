# Matrix × Vector Multiplication

## Why do we multiply a Matrix with a Vector?

There are two ways to think about matrix multiplication.

1. **Computation View (Row × Column)**
2. **Linear Algebra View (Linear Combination of Columns)** ⭐

The second view is much more important because it explains:
- AX = B
- Linear Combination
- Linear Dependence
- Span
- Column Space
- Basis
- Eigenvectors (later)

---

# Method 1 — Row × Column Multiplication

Suppose

$$
A=
\begin{bmatrix}
2&4\\
3&5
\end{bmatrix}
,\qquad
x=
\begin{bmatrix}
1\\
0
\end{bmatrix}
$$

Multiply each row with the vector.

First row

$$
2(1)+4(0)=2
$$

Second row

$$
3(1)+5(0)=3
$$

Therefore,

$$
Ax=
\begin{bmatrix}
2\\
3
\end{bmatrix}
$$

This is the traditional method.

---

# Method 2 — Linear Combination of Columns ⭐

Instead of looking at rows, look at the **columns** of the matrix.

$$
A=
\left[
\begin{array}{cc}
2&4\\
3&5
\end{array}
\right]
$$

Columns are

$$
a_1=
\begin{bmatrix}
2\\
3
\end{bmatrix}
,\qquad
a_2=
\begin{bmatrix}
4\\
5
\end{bmatrix}
$$

The vector is

$$
x=
\begin{bmatrix}
1\\
0
\end{bmatrix}
$$

Now simply use the entries of **x** as coefficients.

$$
Ax=1a_1+0a_2
$$

So,

$$
Ax=
1
\begin{bmatrix}
2\\
3
\end{bmatrix}
+
0
\begin{bmatrix}
4\\
5
\end{bmatrix}
=
\begin{bmatrix}
2\\
3
\end{bmatrix}
$$

Same answer.

---

# Example 1

$$
A=
\begin{bmatrix}
4&7\\
6&0
\end{bmatrix}
,\qquad
x=
\begin{bmatrix}
1\\
2
\end{bmatrix}
$$

Columns are

$$
a_1=
\begin{bmatrix}
4\\
6
\end{bmatrix}
,\qquad
a_2=
\begin{bmatrix}
7\\
0
\end{bmatrix}
$$

Take the linear combination.

$$
Ax=1a_1+2a_2
$$

$$
=
\begin{bmatrix}
4\\
6
\end{bmatrix}
+
2
\begin{bmatrix}
7\\
0
\end{bmatrix}
=
\begin{bmatrix}
18\\
6
\end{bmatrix}
$$

---

# General Formula

Suppose

$$
A=[a_1\ a_2\ a_3\ ...\ a_n]
$$

and

$$
x=
\begin{bmatrix}
x_1\\
x_2\\
x_3\\
\vdots\\
x_n
\end{bmatrix}
$$

Then

$$
Ax=x_1a_1+x_2a_2+x_3a_3+\cdots+x_na_n
$$

This is the **most important interpretation** of matrix multiplication.

---

# Meaning of AX = B

Suppose

$$
Ax=B
$$

Using the column interpretation,

$$
B=x_1a_1+x_2a_2+\cdots+x_na_n
$$

This means:

- Columns of **A** are the building blocks.
- Entries of **X** are the coefficients.
- **B** is the vector obtained after combining those columns.

---

# Example 2

Suppose

$$
A=
\begin{bmatrix}
1&4&7\\
2&5&8\\
3&6&9
\end{bmatrix}
$$

Columns are

$$
a_1=
\begin{bmatrix}
1\\
2\\
3
\end{bmatrix}
,\qquad
a_2=
\begin{bmatrix}
4\\
5\\
6
\end{bmatrix}
,\qquad
a_3=
\begin{bmatrix}
7\\
8\\
9
\end{bmatrix}
$$

Take

$$
x=
\begin{bmatrix}
2\\
1\\
0
\end{bmatrix}
$$

Then

$$
Ax=2a_1+1a_2+0a_3
$$

$$
=
2
\begin{bmatrix}
1\\
2\\
3
\end{bmatrix}
+
\begin{bmatrix}
4\\
5\\
6
\end{bmatrix}
=
\begin{bmatrix}
6\\
9\\
12
\end{bmatrix}
$$

---

# Reverse Problem (AX = B)

Instead of finding **B**, suppose **B** is already given.

Question:

> Which coefficients produce this vector?

Suppose

$$
B=
\begin{bmatrix}
5\\
7\\
9
\end{bmatrix}
$$

Observe

$$
B=a_1+a_2
$$

Therefore,

$$
x=
\begin{bmatrix}
1\\
1\\
0
\end{bmatrix}
$$

This is exactly what solving

$$
Ax=B
$$

means.

---

# Connection to Linear Dependence

Suppose

$$
Ax=0
$$

Using the column interpretation,

$$
x_1a_1+x_2a_2+\cdots+x_na_n=0
$$

If

$$
x\neq0
$$

then

$$
x_1a_1+x_2a_2+\cdots+x_na_n=0
$$

is a **non-trivial linear combination**.

Therefore,

> The columns of **A** are **linearly dependent**.

---

# Important Results

## Case 1

If

$$
Ax=0
$$

has a **non-trivial solution**

$$
x\neq0
$$

then

> Columns of **A** are **Linearly Dependent**.

---

## Case 2

If

$$
Ax=0
$$

has **only the trivial solution**

$$
x=0
$$

then

> Columns of **A** are **Linearly Independent**.

---

# Key Takeaways

- Matrix × Vector is **not just row × column multiplication**.
- It is a **linear combination of the matrix columns**.
- The entries of **X** act as coefficients.
- **AX = B** asks whether **B** can be formed using the columns of **A**.
- If **AX = 0** has a non-trivial solution, the columns of **A** are linearly dependent.

---

# Memory Trick

> **Matrix × Vector = Mix the columns of the matrix using the vector entries as weights.**

Everything in linear algebra—**AX = B, Span, Column Space, Rank, Basis, and Eigenvectors**—is built on this single idea.