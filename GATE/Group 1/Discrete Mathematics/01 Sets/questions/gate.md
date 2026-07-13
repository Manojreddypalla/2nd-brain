# Module 1 — Sets (GATE PYQs)

> [!info]
> These are important **GATE Previous Year Questions (PYQs)** and GATE-level questions covering the **Sets** topic.

---

# GATE PYQ 1

## Question

Which of the following statements is **always true**?

A)

$$
A \subset B
$$

B)

$$
A \subseteq A
$$

C)

$$
A \times B = B \times A
$$

D)

$$
\varnothing = A
$$

---

### Solution

#### Option A

$$
A \subset B
$$

Not always true.

A proper subset cannot be equal to itself.

❌ False

---

#### Option B

$$
A \subseteq A
$$

Every set is a subset of itself.

✅ Always True

---

#### Option C

$$
A \times B = B \times A
$$

Cartesian Product is **not commutative**.

Example

$$
A=\{1\},\quad B=\{x\}
$$

$$
A\times B=\{(1,x)\}
$$

$$
B\times A=\{(x,1)\}
$$

Clearly,

$$
(1,x)\neq(x,1)
$$

❌ False

---

#### Option D

$$
\varnothing=A
$$

Only true when

$$
A=\varnothing
$$

Not always true.

❌ False

---

> [!success]
> **Answer:** **B**

---

# GATE PYQ 2

## Question

If

$$
|A|=5
$$

then the number of subsets of A is

A) 5

B) 10

C) 25

D) 32

---

### Solution

Power Set Formula

$$
|P(A)|=2^n
$$

Here

$$
n=5
$$

Therefore

$$
|P(A)|=2^5=32
$$

> [!success]
> **Answer:** **D**

---

# GATE PYQ 3

## Question

If

$$
|P(A)|=128
$$

then

$$
|A|
$$

is

A) 5

B) 6

C) 7

D) 8

---

### Solution

Using

$$
|P(A)|=2^n
$$

We get

$$
2^n=128
$$

Since

$$
128=2^7
$$

Therefore

$$
|A|=7
$$

> [!success]
> **Answer:** **C**

---

# GATE PYQ 4

## Question

If

$$
|A|=4
$$

and

$$
|B|=6
$$

then

$$
|A\times B|
$$

is

A) 10

B) 16

C) 24

D) 64

---

### Solution

Formula

$$
|A\times B|=|A||B|
$$

Therefore

$$
4\times6=24
$$

> [!success]
> **Answer:** **C**

---

# GATE PYQ 5

## Question

Let

$$
|A|=20
$$

$$
|B|=15
$$

$$
|A\cap B|=5
$$

Find

$$
|A\cup B|
$$

A) 25

B) 30

C) 35

D) 40

---

### Solution

Using the Inclusion-Exclusion Principle

$$
|A\cup B|
=
|A|
+
|B|
-
|A\cap B|
$$

Substitute the values

$$
20+15-5=30
$$

> [!success]
> **Answer:** **B**

---

# GATE PYQ 6

## Question

If

$$
A\cap B=\varnothing
$$

then A and B are

A) Equal

B) Equivalent

C) Disjoint

D) Universal

---

### Solution

By definition,

$$
A\cap B=\varnothing
$$

means the two sets have no common elements.

Hence,

A and B are **Disjoint Sets**.

> [!success]
> **Answer:** **C**

---

# GATE PYQ 7

## Question

If

$$
|A\times B|=24
$$

and

$$
|A|=3
$$

then

$$
|B|
$$

is

A) 6

B) 7

C) 8

D) 9

---

### Solution

Using

$$
|A\times B|=|A||B|
$$

Substitute the values

$$
24=3|B|
$$

Therefore

$$
|B|=8
$$

> [!success]
> **Answer:** **C**

---

# GATE PYQ 8

## Question

Which statement is **FALSE**?

A)

$$
A\cup\varnothing=A
$$

B)

$$
A\cap A=A
$$

C)

$$
A\times B=B\times A
$$

D)

$$
A\subseteq A
$$

---

### Solution

- A is always true.
- B is always true.
- D is always true.
- Cartesian Product is **not commutative**.

Therefore,

> [!success]
> **Answer:** **C**

---

# GATE PYQ 9

## Question

If

$$
A=\{a,b,c\}
$$

then the number of proper subsets is

A) 6

B) 7

C) 8

D) 9

---

### Solution

Total subsets

$$
2^3=8
$$

Proper subsets exclude the set itself.

Therefore

$$
8-1=7
$$

> [!success]
> **Answer:** **B**

---

# GATE PYQ 10

## Question

Let

$$
U=\{1,2,3,4,5,6,7,8\}
$$

and

$$
A=\{2,4,6\}
$$

Find

$$
A'
$$

A)

$$
\{1,3,5,7,8\}
$$

B)

$$
\{2,4,6\}
$$

C)

$$
\{1,3,5\}
$$

D)

$$
\varnothing
$$

---

### Solution

Complement means

Everything in **U** except the elements of A.

Remove

$$
2,4,6
$$

Remaining elements

$$
\{1,3,5,7,8\}
$$

> [!success]
> **Answer:** **A**

---

# 📌 Concepts Covered

| PYQ | Topic |
|------|-------|
| 1 | Subsets |
| 2 | Power Set |
| 3 | Power Set |
| 4 | Cartesian Product |
| 5 | Inclusion-Exclusion Principle |
| 6 | Disjoint Sets |
| 7 | Cartesian Product |
| 8 | Set Identities |
| 9 | Proper Subsets |
| 10 | Complement |