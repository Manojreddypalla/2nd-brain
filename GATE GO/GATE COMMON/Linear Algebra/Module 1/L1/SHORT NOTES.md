Absolutely. For **revision**, you don't need the 92-page version. Here's the **short Obsidian/GATE revision sheet**.

# Linear Algebra — L1C Short Notes

## Scalars, Vectors & Linear Independence

---

## 1. Scalar

A **scalar** is a single number.

$$  
3,;-5,;0,;2  
$$

> Scalar → **one value**

---

## 2. Vector

A vector is an **ordered collection of numbers**.

$$  
v=  
\begin{bmatrix}  
x_1\  
x_2\  
\vdots\  
x_n  
\end{bmatrix}  
$$

### Dimension

Number of components = dimension.

$$  
\begin{bmatrix}  
1\2\3  
\end{bmatrix}  
\in \mathbb R^3  
$$

$$  
\boxed{\mathbb R^n=\text{set of all }n\text{-dimensional real vectors}}  
$$

---

## 3. Vector Operations

### Scalar Multiplication

# $$  
c  
\begin{bmatrix}  
x_1\x_2  
\end{bmatrix}

\begin{bmatrix}  
cx_1\cx_2  
\end{bmatrix}  
$$

### Vector Addition

# $$  
\begin{bmatrix}  
a_1\a_2  
\end{bmatrix}  
+  
\begin{bmatrix}  
b_1\b_2  
\end{bmatrix}

\begin{bmatrix}  
a_1+b_1\a_2+b_2  
\end{bmatrix}  
$$

⚠️ **Vectors must have the same dimension to be added.**

---

# 4. Linear Combination ⭐

A combination of vectors using **scalar multiplication + addition**.

$$  
\boxed{  
c_1v_1+c_2v_2+\cdots+c_nv_n  
}  
$$

Example:

$$  
3v_1-2v_2+5v_3  
$$

Here $3,-2,5$ are the **coefficients**.

---

# 5. Zero Vector

All components are zero.

$$  
\mathbf0=  
\begin{bmatrix}  
0\0\\vdots\0  
\end{bmatrix}  
$$

Important:

$$  
0v=\mathbf0  
$$

$$  
v-v=\mathbf0  
$$

---

# 6. Linear Dependence ⭐⭐⭐

A set is **Linearly Dependent (LD)** if **at least one vector can be represented as a linear combination of the others.**

Example:

$$  
v_1=  
\begin{bmatrix}1\2\end{bmatrix},  
\quad  
v_2=  
\begin{bmatrix}2\4\end{bmatrix}  
$$

Since:

$$  
v_2=2v_1  
$$

$$  
\boxed{{v_1,v_2}\text{ is LD}}  
$$

### Mathematical Test

$$  
c_1v_1+c_2v_2+\cdots+c_nv_n=\mathbf0  
$$

If there exists a **non-trivial solution**:

$$  
\boxed{\text{At least one }c_i\neq0}  
$$

then:

$$  
\boxed{\text{LD}}  
$$

---

# 7. Linear Independence ⭐⭐⭐

A set is **Linearly Independent (LI)** if **no vector can be represented as a linear combination of the others.**

Equivalent test:

$$  
c_1v_1+\cdots+c_nv_n=\mathbf0  
$$

has **only the trivial solution**:

$$  
\boxed{  
c_1=c_2=\cdots=c_n=0  
}  
$$

Therefore:

$$  
\boxed{\text{Only trivial solution}\Rightarrow\text{LI}}  
$$

---

# 8. Trivial vs Non-Trivial

Given:

$$  
c_1v_1+\cdots+c_nv_n=\mathbf0  
$$

### Trivial

$$  
c_1=c_2=\cdots=c_n=0  
$$

➡️ **Does NOT prove LD.**

Every vector set has this solution.

### Non-Trivial

At least one coefficient is non-zero:

$$  
\exists i:c_i\neq0  
$$

➡️ **Proves LD.**

---

# 9. Must-Know Properties ⭐⭐⭐

### Zero Vector

$$  
\boxed{\mathbf0\in S\Rightarrow S\text{ is LD}}  
$$

### LD Set

If:

$$  
S\subseteq T  
$$

and $S$ is LD:

$$  
\boxed{T\text{ is also LD}}  
$$

### LI Set

If $S$ is LI:

$$  
\boxed{\text{Every subset of }S\text{ is LI}}  
$$

⚠️ Converse statements are generally **not true**.

---

# 10. LD vs LI

||**LD**|**LI**|
|---|---|---|
|Non-trivial solution to $\sum c_iv_i=0$|✅|❌|
|Only trivial solution|❌|✅|
|One vector can be made from others|✅|❌|
|Zero vector present|✅|❌|
|Redundancy|Yes|No|

---

# 11. GATE Quick Recognition 🧠

Given vectors, **don't immediately start solving equations**.

Check in this order:

```text
1. Is zero vector present?
       ↓ YES
      LD

2. Is one vector a multiple of another?
       ↓ YES
      LD

3. Can one vector be formed from others?
       ↓ YES
      LD

4. Otherwise solve:
   c₁v₁ + c₂v₂ + ... + cₙvₙ = 0

   Non-trivial solution → LD
   Only trivial solution → LI
```

---

# ⚡ 20-Second Revision

$$  
\boxed{  
\text{Vector}=\text{ordered numbers}  
}  
$$

$$  
\boxed{  
\text{Linear Combination}=\sum c_iv_i  
}  
$$

$$  
\boxed{  
\text{LD}\iff\sum c_iv_i=0  
\text{ has a non-trivial solution}  
}  
$$

$$  
\boxed{  
\text{LI}\iff\sum c_iv_i=0  
\text{ has only the trivial solution}  
}  
$$

$$  
\boxed{  
\mathbf0\in S\Rightarrow S\text{ is LD}  
}  
$$

### 🔥 Core mental model

> **LD = redundancy**  
> **LI = no redundancy**

This is the one I'd keep as your **actual revision note**; the longer version is for learning the topic the first time.