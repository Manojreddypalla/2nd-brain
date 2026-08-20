Yep. I went through **pages 1–122** of `L-5,6.pdf`. This is intentionally **mini revision**: intuition + definitions + formulas + the key comparison tricks. No long derivations.

# L-5,6 — Mini Revision Sheet

## Asymptotic Analysis | Pages 1–122

---

## 1. Algorithm

**Algorithm:** A finite set of steps used to solve a task/problem.

### Why analyze algorithms?

We mainly care about:

- **Time** → how running time grows with input size.
    
- **Space** → how memory usage grows with input size.
    

Actual clock time is unreliable because it depends on:

- Computer
    
- Clock speed
    
- Instruction set
    
- Memory access speed
    

Hence, instead of measuring actual seconds, we count **steps/operations**. This is the idea behind **a priori analysis**.

---

# 2. Input Size

Input size = amount of data given to the algorithm.

Examples:

- Array with `n` elements → input size = `n`
    
- Linked list with `n` nodes → input size = `n`
    
- Matrix → input size may involve `m × n`
    
- Graph → commonly represented using `V` vertices and `E` edges.
    

So complexity is generally written as:

$$T(n)$$

or, for two parameters:

$$T(m,n)$$

---

# 3. Step Count

Instead of asking:

> "How many seconds does the algorithm take?"

ask:

> **"How does the number of operations grow as input grows?"**

Example:

$$T(n)=2n+3$$

For large `n`, the important part is:

$$n$$

Therefore:

$$T(n)=\Theta(n)$$

The exact constants/details become less important for asymptotic behavior.

---

# 4. Asymptotic Analysis

### Intuition

As input becomes **very large**, ignore:

- constant factors
    
- lower-order terms
    

and focus on the **dominant growth term**.

Example:

$$T(n)=n^2+n+100$$

For large `n`:

$$n^2 \gg n \gg 100$$

Therefore:

$$T(n)=\Theta(n^2)$$

### Mental model

> **Asymptotic analysis = "What happens when input becomes huge?"**

---

# 5. Main Asymptotic Notations

|Notation|Meaning|
|---|---|
|$O$|Upper bound|
|$\Omega$|Lower bound|
|$\Theta$|Tight bound|
|$o$|Strict upper bound|
|$\omega$|Strict lower bound|

---

# 6. Big-O — $O$

### Intuition

Think:

> **"Algorithm will not grow faster than this bound."**

Definition:

$$T(n)=O(g(n))$$

if there exist constants $c>0$ and $n_0$ such that

$$T(n)\leq c,g(n)$$

for all

$$n\geq n_0$$

### Meaning

$$T(n)\leq c,g(n)$$

So $g(n)$ gives an **asymptotic upper bound**.

Example:

$$3n+2=O(n)$$

because eventually:

$$3n+2\leq cn$$

for some suitable constant $c$.

---

# 7. Big-Omega — $\Omega$

### Intuition

Think:

> **"Algorithm cannot grow slower than this bound."**

Definition:

$$T(n)=\Omega(g(n))$$

if there exist $c>0$ and $n_0$ such that

$$T(n)\geq c,g(n)$$

for all

$$n\geq n_0$$

### Meaning

$$T(n)\geq c,g(n)$$

So $\Omega$ gives an **asymptotic lower bound**.

Example:

$$3n+2=\Omega(n)$$

---

# 8. Theta — $\Theta$

### Intuition

Theta means:

> **"It grows exactly at this asymptotic rate."**

It combines both:

$$T(n)=O(g(n))$$

and

$$T(n)=\Omega(g(n))$$

Therefore:

$$T(n)=\Theta(g(n))$$

Formal:

$$c_1g(n)\leq T(n)\leq c_2g(n)$$

for sufficiently large `n`.

### Memory trick

$$\boxed{\Omega\leq T\leq O}$$

If both sides meet:

$$\boxed{\Theta}$$

---

# 9. Relationship

If:

$$T(n)=\Theta(g(n))$$

then automatically:

$$T(n)=O(g(n))$$

and

$$T(n)=\Omega(g(n))$$

But:

$$T(n)=O(g(n))$$

does **not** mean:

$$T(n)=\Theta(g(n))$$

because `O` only tells you an upper bound.

---

# 10. Little-o — $o$

### Intuition

Strictly smaller growth.

$$T(n)=o(g(n))$$

means:

> $T(n)$ grows strictly slower than $g(n)$.

Definition:

For **every** $c>0$, eventually

$$T(n)<c,g(n)$$

Equivalent intuition:

$$\boxed{\frac{T(n)}{g(n)}\rightarrow0}  
$$

Example:

$$n^2=o(n^3)$$

because:

$$\frac{n^2}{n^3}=\frac1n\rightarrow0  
$$

---

# 11. Little-Omega — $\omega$

### Intuition

Strictly larger growth.

$$T(n)=\omega(g(n))$$

means:

> $T(n)$ grows strictly faster than $g(n)$.

Equivalent:

$$\boxed{\frac{T(n)}{g(n)}\rightarrow\infty}  
$$

Example:

$$n^3=\omega(n^2)  
$$

---

# 12. Symbol Cheat Sheet

Think of the symbols as inequalities:

$$  
\boxed{  
o;:;<\qquad  
O;:;\leq\qquad  
\Theta;:;=\qquad  
\Omega;:;\geq\qquad  
\omega;:;>  
}  
$$

More precisely, these describe **asymptotic growth**, not ordinary point-by-point comparison.

---

# 13. Important Growth Order

For standard positive functions:

$$  
\boxed{  
\log n  
<  
n  
<  
n\log n  
<  
n^2  
<  
n^3  
<  
c^n  
<  
n!  
}  
$$

for constant $c>1$.

Common hierarchy:

$$  
\boxed{  
\log n  
<  
n  
<  
n\log n  
<  
n^2  
<  
n^3  
<  
2^n  
<  
n!  
}  
$$

The lecture's growth graph emphasizes how rapidly exponential functions eventually dominate polynomial and logarithmic functions.

---

# 14. Constants

Constants do not affect asymptotic class.

For constant $c>0$:

$$  
c,f(n)=\Theta(f(n))  
$$

Examples:

$$  
5n=\Theta(n)  
$$

$$  
100n^2=\Theta(n^2)  
$$

Also:

$$  
n^2+1000=\Theta(n^2)  
$$

because the dominant term is $n^2$.

---

# 15. Dominant Term Rule

For polynomial:

$$  
a_kn^k+\cdots+a_1n+a_0  
$$

the highest power dominates.

Therefore:

# $$  
n^3+5n^2+100n+7

\Theta(n^3)  
$$

Similarly:

$$  
n^2+n=\Theta(n^2)  
$$

---

# 16. Exponential Rules

For constants $a,b>1$:

If

$$a>b$$

then

$$a^n>b^n$$

asymptotically.

Also:

$$  
c^n\gg n^k  
$$

for every fixed constant $k$.

So:

$$  
n^{100}=o(2^n)  
$$

Eventually even a huge polynomial loses to an exponential.

---

# 17. Polynomial Comparison

For positive constants $a,b$:

$$  
n^a  
\quad\text{vs}\quad  
n^b  
$$

If:

$$a<b  
$$

then:

$$  
n^a=o(n^b)  
$$

Example:

$$  
n^3=o(n^5)  
$$

The lecture uses this repeatedly when comparing powers.

---

# 18. Logs — Core Intuition

A logarithm asks:

> **"To what power must I raise the base to obtain this number?"**

$$  
\log_b a=x  
\iff  
b^x=a  
$$

Example:

$$  
\log_2 8=3  
$$

because:

$$  
2^3=8  
$$

### Why logarithms grow slowly?

Because exponential growth is extremely fast.

If:

$$  
n=2^{20}  
$$

then:

$$  
\log_2 n=20  
$$

So logarithm **compresses huge numbers**.

---

# 19. Logarithm Formulas

### Product

$$  
\boxed{\log(ab)=\log a+\log b}  
$$

### Quotient

$$  
\boxed{\log\left(\frac ab\right)=\log a-\log b}  
$$

### Power

$$  
\boxed{\log(a^k)=k\log a}  
$$

### Change of base

$$  
\boxed{  
\log_b a=  
\frac{\log a}{\log b}  
}  
$$

### Reciprocal

$$  
\boxed{  
\log_b\frac1a=-\log_ba  
}  
$$

### Base conversion

$$  
\boxed{  
\log_b a=\frac1{\log_a b}  
}  
$$

These formulas are explicitly used throughout the lecture for asymptotic comparisons.

---

# 20. Important Log Facts

For fixed bases $a,b>1$:

$$  
\log_a n=\Theta(\log_b n)  
$$

Therefore:

$$  
\boxed{\text{Log base does not matter asymptotically}}  
$$

Example:

$$  
\log_2 n=\Theta(\log_{10}n)  
$$

because they differ only by a constant factor.

---

# 21. Nested Logs

Very useful:

$$  
\log(n^k)=k\log n  
$$

But:

$$  
\log(\log n)  
$$

is much smaller than:

$$  
\log n  
$$

and:

$$  
(\log n)^k  
$$

is still smaller than:

$$  
n^\epsilon  
$$

for every fixed $\epsilon>0$.

Important lecture comparison:

$$  
\boxed{(\log n)^k=o(n^\epsilon)}  
$$

for fixed $k>0,\epsilon>0$.

---

# 22. Log Comparison Trick

When comparing:

$$  
f(n),g(n)  
$$

you can often take logarithms.

If:

$$  
\log f(n)>\log g(n)  
$$

then:

$$  
f(n)>g(n)  
$$

provided the functions are positive.

If:

$$  
\log f(n)=\log g(n)  
$$

then the log comparison **alone cannot determine** the original asymptotic relation.

This is one of the lecture's main comparison tools.

---

# 23. Cancel Common Terms

When comparing:

$$  
f(n),g(n)  
$$

you can cancel common multiplicative/divisive factors.

Example:

$$  
\frac{n^2\log n}{n\log n}=n  
$$

So the comparison reduces to `n`.

But:

> **Do not blindly ignore additive constants inside expressions.**

The lecture explicitly emphasizes canceling common terms and being careful with constants.

---

# 24. Taking Logs of Exponentials

Very useful GATE trick:

$$  
\log(a^n)=n\log a  
$$

Therefore comparing:

$$  
a^n  
\quad\text{and}\quad  
b^n  
$$

becomes comparing:

$$  
n\log a  
\quad\text{and}\quad  
n\log b  
$$

which reduces to:

$$  
\log a  
\quad\text{vs}\quad  
\log b  
$$

---

# 25. Factorial

Definition:

$$  
n!=n(n-1)(n-2)\cdots2\cdot1  
$$

Growth:

$$  
n! \gg c^n  
$$

for fixed constant $c$.

Also:

$$  
n!<n^n  
$$

because every factor in the factorial is at most `n`.

---

# 26. Stirling's Approximation

Extremely important:

$$  
\boxed{  
n!\sim\sqrt{2\pi n}  
\left(\frac ne\right)^n  
}  
$$

For asymptotic purposes:

$$  
\boxed{  
n!=\Theta\left(\sqrt n\left(\frac ne\right)^n\right)  
}  
$$

The lecture uses Stirling to compare factorial with exponential/polynomial functions.

---

# 27. Log of Factorial

From Stirling:

# $$  
\log(n!)

\Theta(n\log n)  
$$

So:

$$  
\boxed{\log(n!)=\Theta(n\log n)}  
$$

The lecture also derives this using:

# $$  
\log(n!)

\log n+\log(n-1)+\cdots+\log1  
$$

and bounds the sum by approximately:

$$  
n\log n  
$$

---

# 28. Very Useful Factorial Comparison

Since:

$$  
\log(n!)=\Theta(n\log n)  
$$

we know:

$$  
n!=2^{\Theta(n\log n)}  
$$

because:

$$  
2^{n\log_2 n}=n^n  
$$

and factorial is below that but still super-exponential relative to fixed-base $c^n$.

---

# 29. Incomparability

Not every pair of functions can be ordered asymptotically.

Example from the lecture:

$$  
f(n)=n+n\sin n  
$$

or equivalently the discussed oscillating form:

$$  
n^1+n\sin n  
$$

The $\sin n$ term oscillates between:

$$  
-1\leq\sin n\leq1  
$$

so there may be no single eventual relationship with another function.

Thus some functions are **incomparable** under the basic asymptotic relations.

---

# 30. Key Relationship Between Notations

### Tight bound

$$  
f(n)=\Theta(g(n))  
$$

means both:

$$  
f(n)=O(g(n))  
$$

and

$$  
f(n)=\Omega(g(n))  
$$

### Transpose / inverse relationships

If:

$$  
f(n)=O(g(n))  
$$

then:

$$  
g(n)=\Omega(f(n))  
$$

If:

$$  
f(n)=o(g(n))  
$$

then:

$$  
g(n)=\omega(f(n))  
$$

---

# 31. Reflexivity

For suitable functions:

$$  
f(n)=\Theta(f(n))  
$$

$$  
f(n)=O(f(n))  
$$

$$  
f(n)=\Omega(f(n))  
$$

Think:

> A function is always asymptotically comparable to itself.

---

# 32. Symmetry

Theta is symmetric:

$$  
\boxed{  
f(n)=\Theta(g(n))  
\iff  
g(n)=\Theta(f(n))  
}  
$$

But **Big-O is not symmetric**.

For example:

$$  
n=O(n^2)  
$$

but:

$$  
n^2\neq O(n)  
$$

---

# 33. Transitivity

If:

$$  
f(n)=O(g(n))  
$$

and:

$$  
g(n)=O(h(n))  
$$

then:

$$  
\boxed{f(n)=O(h(n))}  
$$

Similarly:

$$  
f=\Omega(g),\quad g=\Omega(h)  
$$

implies:

$$  
f=\Omega(h)  
$$

And:

$$  
f=\Theta(g),\quad g=\Theta(h)  
$$

implies:

$$  
f=\Theta(h)  
$$

The same transitivity pattern applies to $o$ and $\omega$.

---

# 34. Asymptotic Comparison Cheat Method

When asked:

> Compare $f(n)$ and $g(n)$

Use this order:

### Step 1 — Cancel common factors

Simplify.

### Step 2 — Remove constants

Only for asymptotic classification.

### Step 3 — Look at dominant terms

Example:

$$  
n^3+n^2+n  
\rightarrow n^3  
$$

### Step 4 — If difficult, take logarithm

Convert:

$$  
a^n\rightarrow n\log a  
$$

$$  
n!\rightarrow\Theta(n\log n)  
$$

### Step 5 — Compare growth classes

Use:

$$  
\log n<n<n\log n<n^2<n^3<c^n<n!  
$$

This is essentially the "toolbox" approach emphasized in the lecture.

---

# 35. Important Growth Identities

Memorize these:

$$  
\boxed{\log n=o(n)}  
$$

$$  
\boxed{n=o(n\log n)}  
$$

$$  
\boxed{n\log n=o(n^2)}  
$$

$$  
\boxed{n^k=o(c^n)}  
\qquad c>1  
$$

$$  
\boxed{c^n=o(n!)}  
$$

$$  
\boxed{(\log n)^k=o(n^\epsilon)}  
\qquad k>0,\epsilon>0  
$$

---

# 36. Common Transformations

### Polynomial

$$  
n^a=O(n^b)  
\quad\text{if }a\leq b  
$$

### Exponential

$$  
a^n=O(b^n)  
\quad\text{if }a\leq b  
$$

### Log powers

$$  
(\log n)^a=O((\log n)^b)  
\quad\text{if }a\leq b  
$$

### Log vs polynomial

$$  
(\log n)^k=o(n^\epsilon)  
$$

### Polynomial vs exponential

$$  
n^k=o(c^n)  
$$

---

# 37. Important Lecture Result

The lecture demonstrates comparisons such as:

$$  
n^2=O(n^3)  
$$

but:

$$  
n^2\neq O(2^n)  
$$

is **false** — in fact:

$$  
n^2=O(2^n)  
$$

because exponential eventually dominates polynomial.

Likewise:

$$  
n^2=o(n^3)  
$$

and:

$$  
n^2=\Theta(n^2)  
$$

---

# 38. Final GATE Mental Model

When you see:

$$  
f(n)\quad\text{vs}\quad g(n)  
$$

ask:

### ① Are they the same growth?

$$  
f=\Theta(g)  
$$

### ② Is `f` eventually smaller?

$$  
f=O(g)  
$$

possibly:

$$  
f=o(g)  
$$

### ③ Is `f` eventually larger?

$$  
f=\Omega(g)  
$$

possibly:

$$  
f=\omega(g)  
$$

### ④ Can't establish either?

They may be **incomparable**.

---

# ⚡ 30-Second Revision

$$  
\boxed{  
O=\text{upper}  
}  
$$

$$  
\boxed{  
\Omega=\text{lower}  
}  
$$

$$  
\boxed{  
\Theta=\text{tight}  
}  
$$

$$  
\boxed{  
o=\text{strictly smaller}  
}  
$$

$$  
\boxed{  
\omega=\text{strictly larger}  
}  
$$

Growth:

$$  
\boxed{  
\log n  
<  
n  
<  
n\log n  
<  
n^2  
<  
n^3  
<  
2^n  
<  
n!  
}  
$$

Logs:

$$  
\boxed{\log(ab)=\log a+\log b}  
$$

$$  
\boxed{\log(a^k)=k\log a}  
$$

$$  
\boxed{\log(a/b)=\log a-\log b}  
$$

$$  
\boxed{\log_ba=\frac{\log a}{\log b}}  
$$

Factorial:

$$  
\boxed{  
n!\sim\sqrt{2\pi n}\left(\frac ne\right)^n  
}  
$$

and:

$$  
\boxed{\log(n!)=\Theta(n\log n)}  
$$

Core rule:

> **For huge `n`, constants and lower-order terms disappear; the dominant growth decides the asymptotic behavior.**

This captures the **intuition, definitions, formulas, growth hierarchy, comparison tricks, logarithms, factorial/Stirling, and asymptotic properties covered through page 122** of the uploaded lecture.