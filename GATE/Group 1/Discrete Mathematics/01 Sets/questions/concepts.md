# Module 1 — Sets (Important Q&A)

---

# Q1. What is a Set?

### Answer

A **Set** is a **well-defined collection of distinct objects**.

Example

$$
A=\{1,2,3\}
$$

---

# Q2. What is the difference between a Set and an Ordered Pair?

### Answer

A **Set** ignores order.

$$
\{1,2\}=\{2,1\}
$$

An **Ordered Pair** preserves order.

$$
(1,2)\neq(2,1)
$$

---

# Q3. What is the symbol for Membership?

### Answer

Belongs to

$$
\in
$$

Does not belong

$$
\notin
$$

Example

$$
2\in\{1,2,3\}
$$

---

# Q4. What are the two methods of representing a Set?

### Answer

1. Roster Form

$$
A=\{2,4,6,8\}
$$

2. Set Builder Form

$$
A=\{x\mid x\text{ is even and }x<10\}
$$

---

# Q5. What is the Universal Set?

### Answer

The Universal Set contains **all elements under discussion**.

Notation

$$
U
$$

Example

$$
U=\{1,2,\ldots,10\}
$$

---

# Q6. What is Cardinality?

### Answer

Cardinality is the number of elements in a set.

Notation

$$
|A|
$$

Example

$$
A=\{2,5,9\}
$$

$$
|A|=3
$$

---

# Q7. What is a Subset?

### Answer

A is a subset of B if every element of A belongs to B.

Notation

$$
A\subseteq B
$$

Example

$$
\{1,2\}\subseteq\{1,2,3\}
$$

---

# Q8. Difference between Subset and Proper Subset?

### Answer

Subset

$$
A\subseteq B
$$

Equality allowed.

Proper Subset

$$
A\subset B
$$

Equality NOT allowed.

---

# Q9. Is every set a subset of itself?

### Answer

Yes.

$$
A\subseteq A
$$

Always true.

---

# Q10. Is the Empty Set a subset of every set?

### Answer

Yes.

$$
\varnothing\subseteq A
$$

Always true.

---

# Q11. What is Union?

### Answer

Union contains all unique elements.

Notation

$$
A\cup B
$$

Example

$$
A=\{1,2,3\}
$$

$$
B=\{3,4,5\}
$$

Answer

$$
A\cup B=\{1,2,3,4,5\}
$$

---

# Q12. What is Intersection?

### Answer

Intersection contains common elements.

Notation

$$
A\cap B
$$

Example

$$
A\cap B=\{3\}
$$

---

# Q13. What is Difference?

### Answer

Elements present in A but not in B.

Notation

$$
A-B
$$

Example

$$
\{1,2,3\}-\{2,4\}=\{1,3\}
$$

---

# Q14. What is Complement?

### Answer

Everything in the Universal Set except A.

Notation

$$
A'
$$

Example

$$
U=\{1,2,3,4,5,6\}
$$

$$
A=\{2,4,6\}
$$

$$
A'=\{1,3,5\}
$$

---

# Q15. What are Disjoint Sets?

### Answer

Two sets having no common elements.

Formula

$$
A\cap B=\varnothing
$$

---

# Q16. What is a Power Set?

### Answer

Set containing all subsets.

Notation

$$
P(A)
$$

Example

$$
A=\{a,b\}
$$

$$
P(A)=
\{
\varnothing,
\{a\},
\{b\},
\{a,b\}
\}
$$

---

# Q17. Formula for Power Set?

### Answer

If

$$
|A|=n
$$

then

$$
|P(A)|=2^n
$$

---

# Q18. How many proper subsets does a set have?

### Answer

If

$$
|A|=n
$$

then

$$
2^n-1
$$

---

# Q19. What is Cartesian Product?

### Answer

Set of all ordered pairs.

Notation

$$
A\times B
$$

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

# Q20. Formula for Cartesian Product?

### Answer

If

$$
|A|=m
$$

and

$$
|B|=n
$$

then

$$
|A\times B|=mn
$$

---

# Q21. Is

$$
A\times B=B\times A
$$

always true?

### Answer

No.

Example

$$
A=\{1\}
$$

$$
B=\{x\}
$$

$$
A\times B=\{(1,x)\}
$$

$$
B\times A=\{(x,1)\}
$$

Hence,

$$
A\times B\neq B\times A
$$

---

# Q22. Why does a Power Set have

$$
2^n
$$

subsets?

### Answer

Each element has two choices.

- Include
- Exclude

Therefore

$$
2\times2\times2\cdots=2^n
$$

---

# Q23. Which formulas are most important for GATE?

### Answer

Power Set

$$
|P(A)|=2^n
$$

Proper Subsets

$$
2^n-1
$$

Cartesian Product

$$
|A\times B|=|A||B|
$$

Disjoint Sets

$$
A\cap B=\varnothing
$$

Every Set

$$
A\subseteq A
$$

Empty Set

$$
\varnothing\subseteq A
$$

---

# Q24. What are the most common GATE mistakes?

### Answer

❌

$$
(1,2)=\{1,2\}
$$

False.

---

❌

$$
A\times B=B\times A
$$

False.

---

❌

$$
A\subset A
$$

Always true.

False.

---

❌

Power Set = All combinations.

False.

Power Set = **All subsets**.