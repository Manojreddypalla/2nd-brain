# Linear Algebra — Lecture 1: Introduction

> [!info] Lecture Overview
> **Topic:** Introduction to Linear Algebra  
> **Focus:** Motivation, data representation, embeddings, applications, and GATE relevance

---

## 1. Why Study Linear Algebra?

Linear Algebra is important because **real-world data can be represented numerically using vectors and matrices**.

Examples of data:

- Text
- Images
- Audio
- Video
- Numerical / tabular data

The basic idea is:

$$
\text{Raw Data}
\rightarrow
\text{Numerical Representation}
\rightarrow
\text{Vectors / Matrices}
\rightarrow
\text{Machine Processing}
$$

Linear Algebra provides the mathematical tools required to represent and manipulate this data.

---

## 2. Linear Algebra and Machine Learning

> **Linear Algebra is the fuel of Machine Learning.**

Different types of data can be represented using **vectors and matrices**.

The overall idea is:

$$
\boxed{
\text{Real-world Data}
\rightarrow
\text{Numbers}
\rightarrow
\text{Vectors / Matrices}
\rightarrow
\text{Mathematical Operations}
\rightarrow
\text{Machine Learning}
}
$$

---

# 3. Data Representation

## 3.1 One Data Point → Vector

Suppose we have information about a person:

| Height | Age | Weight |
| ---: | ---: | ---: |
| 170 | 24 | 70 |

This single observation can be represented as a vector:

$$
\mathbf{x}
=
\begin{bmatrix}
170\\
24\\
70
\end{bmatrix}
$$

Therefore:

$$
\boxed{
\text{One Data Point}
\rightarrow
\text{Vector}
}
$$

The three components represent three features:

$$
\mathbf{x}
=
\begin{bmatrix}
\text{Height}\\
\text{Age}\\
\text{Weight}
\end{bmatrix}
$$

### Key Idea

Each **feature** becomes a component of the vector.

$$
\text{Data Point}
=
\begin{bmatrix}
\text{Feature}_1\\
\text{Feature}_2\\
\vdots\\
\text{Feature}_n
\end{bmatrix}
$$

---

## 3.2 Multiple Data Points → Matrix

If we have many observations:

$$
\begin{bmatrix}
170 & 24 & 70\\
165 & 45 & 65\\
190 & 28 & 102\\
180 & 34 & 83\\
182 & 30 & 79\\
178 & 67 & 85
\end{bmatrix}
$$

The complete dataset can be represented as a **matrix**.

Therefore:

$$
\boxed{
\text{Multiple Data Points}
\rightarrow
\text{Matrix}
}
$$

### Important Relationship

$$
\boxed{
\text{One Data Point} \rightarrow \text{Vector}
}
$$

$$
\boxed{
\text{Multiple Data Points} \rightarrow \text{Matrix}
}
$$

---

# 4. Vector as a Point in Space

A vector can also be interpreted geometrically.

For example:

$$
\mathbf{x}
=
\begin{bmatrix}
x_1\\
x_2\\
x_3
\end{bmatrix}
$$

can represent a point/vector in **3-dimensional space**.

For the previous example:

$$
\mathbf{x}
=
\begin{bmatrix}
170\\
24\\
70
\end{bmatrix}
$$

The three dimensions correspond to:

- Height
- Age
- Weight

Therefore, a data point with three features can be represented as a point in **3-D space**.

### General Pattern

If a data point has $n$ features:

$$
\mathbf{x}
=
\begin{bmatrix}
x_1\\
x_2\\
\vdots\\
x_n
\end{bmatrix}
$$

then it can be considered as a point in an $n$-dimensional space.

---

# 5. Word Embeddings

## Definition

A **word embedding** represents a word using a numerical vector.

$$
\boxed{
\text{Word}
\rightarrow
\text{Numerical Vector}
}
$$

For example:

$$
\text{king}
\rightarrow
\mathbf{v}_{king}
$$

$$
\text{queen}
\rightarrow
\mathbf{v}_{queen}
$$

$$
\text{man}
\rightarrow
\mathbf{v}_{man}
$$

$$
\text{woman}
\rightarrow
\mathbf{v}_{woman}
$$

The purpose is to convert textual information into a numerical representation that machines can process.

---

## 5.1 Relationships Between Words

The lecture gives the following conceptual relationship:

$$
\boxed{
\text{king}
-
\text{man}
+
\text{woman}
\approx
\text{queen}
}
$$

In vector form:

$$
\boxed{
\mathbf{v}_{king}
-
\mathbf{v}_{man}
+
\mathbf{v}_{woman}
\approx
\mathbf{v}_{queen}
}
$$

### Intuition

Vectors can represent not only individual objects but also **relationships between concepts**.

This demonstrates how vector operations can capture semantic relationships between words.

---

# 6. Embeddings — General Idea

The broader concept is:

$$
\boxed{
\text{Object}
\rightarrow
\text{Numerical Vector}
}
$$

Examples:

$$
\text{Word} \rightarrow \text{Vector}
$$

$$
\text{Image} \rightarrow \text{Vector}
$$

$$
\text{User} \rightarrow \text{Vector}
$$

$$
\text{Movie} \rightarrow \text{Vector}
$$

Once information is represented as vectors, mathematical operations can be performed on it.

---

# 7. Recommendation Systems

Linear Algebra and embeddings can be used in **recommendation engines**.

The lecture discusses information such as:

- Different types of users
- Movie ratings
- Types of movies
- Movies for different audiences

The conceptual pipeline is:

$$
\text{Users + Movies + Ratings}
\rightarrow
\text{Numerical Representation}
\rightarrow
\text{Vectors}
\rightarrow
\text{Relationships}
\rightarrow
\text{Recommendations}
$$

### Intuition

Users and movies can be represented using numerical information.

If two users have similar preferences, their representations can be similar.

Similarly, movies with similar characteristics can have similar representations.

This numerical representation can then be used to generate recommendations.

---

# 8. Applications of Linear Algebra

The lecture mentions several areas where Linear Algebra is heavily used:

- Statistics
- Chemical Physics
- Genomics
- Word Embeddings
- Neural Networks / Deep Learning
- Robotics
- Image Processing
- Quantum Physics

### Important CS Applications

$$
\boxed{
\text{Machine Learning}
+
\text{Deep Learning}
+
\text{NLP}
+
\text{Image Processing}
+
\text{Robotics}
}
$$

---

# 9. Linear Algebra for GATE

According to the lecture:

- Approximately **1–2 questions** appear every year.
- Approximately **2–3 marks**.
- Linear Algebra is considered a relatively **scoring area**.

The lecture also shows GATE questions involving:

- Matrices
- Determinants
- Eigenvalues

### GATE Approach

Do not study Linear Algebra only as definitions.

Use the chain:

$$
\boxed{
\text{Definition}
\rightarrow
\text{Property}
\rightarrow
\text{Formula}
\rightarrow
\text{Relationship}
\rightarrow
\text{GATE Question}
}
$$

---

# 10. Linear Algebra for IIT Interviews

Linear Algebra is also relevant for higher-study interviews.

The lecture connects it with:

- M.Tech
- M.Tech / MS
- Research-oriented programs
- Technical interviews

Therefore, Linear Algebra is useful beyond GATE preparation.

---

# 11. Big Picture

The central idea of this lecture is:

$$
\boxed{
\text{Real-world Data}
\rightarrow
\text{Numerical Data}
\rightarrow
\text{Vectors}
\rightarrow
\text{Matrices}
\rightarrow
\text{Linear Algebra}
}
$$

And then:

$$
\boxed{
\text{Linear Algebra}
\rightarrow
\text{ML / DL / NLP / Image Processing / Robotics}
}
$$

### Mental Model

> **Linear Algebra is a mathematical language for representing and manipulating structured numerical data.**

---

# 12. GATE Quick Revision

### Data Representation

$$
\boxed{
\text{One Data Point}
\rightarrow
\text{Vector}
}
$$

$$
\boxed{
\text{Multiple Data Points}
\rightarrow
\text{Matrix}
}
$$

### Word Embedding

$$
\boxed{
\text{Word}
\rightarrow
\text{Numerical Vector}
}
$$

### Vector Relationship

$$
\boxed{
\mathbf{v}_{king}
-
\mathbf{v}_{man}
+
\mathbf{v}_{woman}
\approx
\mathbf{v}_{queen}
}
$$

### Applications

$$
\boxed{
ML,\ DL,\ NLP,\ Image\ Processing,\ Robotics,\ Statistics
}
$$

### GATE Weightage

$$
\boxed{
\approx 1-2\text{ questions/year}
}
$$

$$
\boxed{
\approx 2-3\text{ marks}
}
$$

---

# 13. What to Remember from Lecture 1

> [!important] Core Takeaways

1. **Data can be represented numerically.**
2. **A single data point can be represented as a vector.**
3. **Multiple data points can be represented as a matrix.**
4. **Words can be represented as vectors using embeddings.**
5. **Vector operations can represent relationships between concepts.**
6. **Linear Algebra is heavily used in ML, DL, NLP, image processing, robotics, etc.**
7. **Linear Algebra is relevant for GATE and technical interviews.**

---

# ⚡ 30-Second Revision

$$
\boxed{
\text{Data}
\rightarrow
\text{Vectors}
\rightarrow
\text{Matrices}
\rightarrow
\text{Linear Algebra}
\rightarrow
\text{Machine Learning}
}
$$

**Vector:** Numerical representation of a data point/object.

**Matrix:** Structured arrangement of numerical data.

**Embedding:** Representation of an object using a numerical vector.

**Word Embedding:**

$$
\text{Word} \rightarrow \text{Vector}
$$

**Key example:**

$$
\mathbf{v}_{king}
-
\mathbf{v}_{man}
+
\mathbf{v}_{woman}
\approx
\mathbf{v}_{queen}
$$