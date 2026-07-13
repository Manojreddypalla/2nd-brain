# 📌 Module 1 — Sets (Mini Notes)

> [!summary]
> **Sets** are the foundation of Discrete Mathematics.
>
> Relations → Functions → Graphs → Probability → Automata → Databases

---

# 1. Definition

A **Set** is a **well-defined collection of distinct objects**.

✔ Well-defined → Membership is unambiguous.

✔ Distinct → No duplicates.

✔ Unordered → Order doesn't matter.

---

# 2. Membership

$$
\in
$$

Belongs to

$$
\notin
$$

Does not belong to

Example

$$
1\in\{1,2,3\}
$$

$$
4\notin\{1,2,3\}
$$

---

# 3. Representation

### Roster Form

$$
A=\{2,4,6,8\}
$$

### Set Builder Form

$$
A=\{x\mid x\text{ is even and }x<10\}
$$

---

# 4. Types of Sets

- Empty Set → $\varnothing$
- Singleton Set → {7}
- Finite Set
- Infinite Set
- Equal Sets
- Equivalent Sets
- Universal Set → $U$
- Disjoint Sets

---

# 5. Cardinality

Number of elements.

Notation

$$
|A|
$$

Example

$$
|\{1,2,3\}|=3
$$

---

# 6. Subsets

Subset

$$
A\subseteq B
$$

Proper Subset

$$
A\subset B
$$

Important

- Every set is a subset of itself.

$$
A\subseteq A
$$

- Empty set is a subset of every set.

$$
\varnothing\subseteq A
$$

---

# 7. Set Operations

### Union

Everything

$$
A\cup B
$$

---

### Intersection

Common elements

$$
A\cap B
$$

---

### Difference

Only elements in A

$$
A-B
$$

---

### Complement

Everything except A

$$
A'
$$

---

### Disjoint Sets

No common elements

$$
A\cap B=\varnothing
$$

---

# 8. Venn Diagram

Remember

- Entire region → Union
- Overlap → Intersection
- Outside A → Complement
- No overlap → Disjoint

---

# 9. Power Set

Set of **all subsets**.

Notation

$$
P(A)
$$

Formula

$$
|P(A)|=2^n
$$

---

# 10. Cartesian Product

Forms **ordered pairs**.

Notation

$$
A\times B
$$

Formula

$$
|A\times B|
=
|A|\times|B|
$$

Remember

- First element → A
- Second element → B

Example

$$
A=\{1,2\}
$$

$$
B=\{x,y\}
$$

$$
A\times B=
\{
(1,x),
(1,y),
(2,x),
(2,y)
\}
$$

---

# ⭐ Must Remember

- Sets ignore **order**.
- Sets ignore **duplicates**.
- Ordered pairs preserve **order**.
- Complement depends on the **Universal Set**.
- Power Set contains **all subsets**.
- Cartesian Product creates **ordered pairs**.

---

# ⚡ Formula Sheet

$$
|A|
$$

Cardinality

---

$$
|P(A)|=2^n
$$

Power Set

---

$$
|A\times B|
=
|A|\times|B|
$$

Cartesian Product

---

$$
A\cap B=\varnothing
$$

Disjoint Sets

---

$$
A\subseteq A
$$

Every set is a subset of itself.

---

$$
\varnothing\subseteq A
$$

Empty set is a subset of every set.

---

# 🚨 Common Mistakes

❌

$$
(1,2)=\{1,2\}
$$

✔ False

---

❌

$$
\{1,2\}\neq\{2,1\}
$$

✔ False

---

Power Set ≠ All combinations

✔ Power Set = **All subsets**

---

# 🎯 GATE Focus

⭐⭐⭐⭐⭐ Power Set

⭐⭐⭐⭐⭐ Subsets

⭐⭐⭐⭐⭐ Cartesian Product

⭐⭐⭐⭐⭐ Set Operations

⭐⭐⭐⭐⭐ Venn Diagrams

⭐⭐⭐⭐⭐ Inclusion–Exclusion (Next Topic)