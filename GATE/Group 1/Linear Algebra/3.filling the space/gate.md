# 📘 Filling the Space (Span)

> **Definition**
>
> A set of vectors **fills (spans) a vector space** if **every vector** in that space can be written as a **linear combination** of those vectors.

---

# 💡 Intuition

Imagine **$$R^2$$** as an empty sheet of paper.

Vectors are directions you can move.

- Enough **independent directions** → Reach every point → **Fill the space**
- Not enough directions → Reach only some points → **Do NOT fill the space**

---

# Example 1 — Fills $$R^2$$ ✅

Given

$$
v_1=(1,0), \qquad
v_2=(0,1)
$$

Take any vector

$$
(5,-3)
$$

Then

$$
(5,-3)=5(1,0)-3(0,1)
$$

Since **every vector** can be represented,

✅ The vectors **fill $$R^2$$**.

---

# Example 2 — Also Fills $$R^2$$ ✅

Given

$$
(1,2),\quad(2,5)
$$

These vectors are **linearly independent**.

Hence every vector

$$
(a,b)
$$

can be written as

$$
(a,b)=\alpha(1,2)+\beta(2,5)
$$

for some scalars $$\alpha,\beta$$.

Therefore,

✅ They also **fill $$R^2$$**.

---

# Example 3 — Does NOT Fill $$R^2$$ ❌

Given

$$
(1,2),\quad(3,6)
$$

Observe

$$
(3,6)=3(1,2)
$$

Both vectors point in the **same direction**.

You can only move along one line.

For example,

$$
(2,1)
$$

cannot be generated.

❌ Hence they **do not fill $$R^2$$**.

---

# Example 4 — Filling $$R^3$$ ✅

Standard vectors

$$
(1,0,0),\quad
(0,1,0),\quad
(0,0,1)
$$

Take

$$
(2,5,-1)
$$

Then

$$
(2,5,-1)
=
2(1,0,0)
+
5(0,1,0)
-
(0,0,1)
$$

Hence,

✅ These vectors **fill $$R^3$$**.

---

# Why Linear Independence Matters

Independent vectors provide **new directions**.

Dependent vectors **repeat an existing direction**.

Example

$$
(1,0),\quad(2,0)
$$

The second vector is just a multiple of the first.

So there is only **one direction**.

Therefore,

❌ They cannot fill $$R^2$$.

---

# Relationship

```text
Linear Independence
        │
        ▼
Different Directions
        │
        ▼
Can Reach Every Vector
        │
        ▼
Fill (Span) the Space
```

---

# General Rule

| Space | Minimum Independent Vectors Needed |
|-------|-----------------------------------:|
| $$R^1$$ | 1 |
| $$R^2$$ | 2 |
| $$R^3$$ | 3 |
| $$R^n$$ | n |

---

# Important Results ⭐

### Theorem 1

Any **$$n$$ linearly independent vectors** in

$$
R^n
$$

fill the entire space.

---

### Theorem 2

If the number of vectors is greater than the dimension,

$$
m>n
$$

then

$$
m \text{ vectors in } R^n
$$

are **always linearly dependent**.

---

### Theorem 3

If vectors are **linearly dependent**,

they **cannot** be the minimum set required to fill the space.

---

# Basis Connection ⭐

**Basis = Smallest set that fills the space.**

Example

Basis of $$R^2$$

$$
\{(1,0),(0,1)\}
$$

Example

$$
\{(1,0),(0,1),(3,5)\}
$$

Still fills $$R^2$$

❌ Not a basis (extra vector is **redundant**).

---

# Memory Trick 🧠

Imagine navigating a city.

- **1 direction** → Move on a line ($$R^1$$)
- **2 independent directions** → Reach anywhere on a plane ($$R^2$$)
- **3 independent directions** → Reach anywhere in space ($$R^3$$)

👉 **Enough independent directions = Filling the Space.**

---

# GATE Corner 🎯

- **Span = Filling the Space**
- **Independent vectors provide new directions**
- **Dependent vectors repeat existing directions**
- **2 independent vectors fill $$R^2$$**
- **3 independent vectors fill $$R^3$$**
- **$$n$$ independent vectors fill $$R^n$$**
- **More than $$n$$ vectors in $$R^n$$ ⇒ Linearly Dependent**