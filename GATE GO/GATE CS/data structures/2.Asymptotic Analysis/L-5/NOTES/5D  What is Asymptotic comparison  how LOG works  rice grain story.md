# 📌 Asymptotic Analysis — GATE Notes

### Pages 50–66 | Obsidian Ready

> **Continuation of Pages 42–49:** These pages deepen the meaning of asymptotic comparison, dominant terms, growth rates, logarithms, and exponential growth.

---

## 1. Relationship Between O, Ω and Θ

For two functions $f(n)$ and $g(n)$:

### Big-O

$$  
f(n)\le c,g(n)  
$$

$$  
\boxed{f(n)=O(g(n))}  
$$

→ $f$ grows **no faster than** $g$.

### Big-Ω

$$  
f(n)\ge c,g(n)  
$$

$$  
\boxed{f(n)=\Omega(g(n))}  
$$

→ $f$ grows **at least as fast as** $g$.

### Big-Θ

$$  
f(n)=O(g(n))\quad\text{and}\quad f(n)=\Omega(g(n))  
$$

$$  
\boxed{f(n)=\Theta(g(n))}  
$$

→ Same asymptotic growth rate.

---

# 2. Important Property

If:

$$  
\boxed{f(n)=\Theta(g(n))}  
$$

then automatically:

$$  
\boxed{f(n)=O(g(n))}  
$$

and

$$  
\boxed{f(n)=\Omega(g(n))}  
$$

But the reverse is important:

If only:

$$  
f(n)=O(g(n))  
$$

you **cannot** conclude:

$$  
f(n)=\Theta(g(n))  
$$

Example:

$$  
n=O(n^2)  
$$

but:

$$  
n\ne\Theta(n^2)  
$$

---

# 3. When Can $O$ and $\Omega$ Both Be True?

If:

$$  
f(n)=O(g(n))  
$$

and

$$  
f(n)=\Omega(g(n))  
$$

then:

$$  
\boxed{f(n)=\Theta(g(n))}  
$$

The lecture illustrates this as the situation where the two functions are asymptotically equal.

---

# 4. "Asymptotically Larger" — Important Idea ⭐

When comparing functions, we eventually **ignore constants and insignificant terms**.

Example:

$$  
2n^2+3  
$$

For very large $n$:

$$  
\boxed{2n^2+3\approx n^2}  
$$

because:

- constant multiplier $2$ does not affect growth class
    
- $+3$ becomes insignificant compared with $n^2$
    

Thus:

$$  
\boxed{2n^2+3=\Theta(n^2)}  
$$

---

# 5. Dominant Term

Consider:

$$  
n^2+100000  
$$

For small $n$, the constant may look significant.

But as $n\rightarrow\infty$:

$$  
n^2\gg100000  
$$

Therefore:

$$  
\boxed{n^2+100000=\Theta(n^2)}  
$$

### Rule

For polynomial expressions:

$$  
an^k+\cdots  
$$

the **highest power of $n$** dominates.

$$  
\boxed{an^k+\text{lower-order terms}=\Theta(n^k)}  
$$

---

# 6. Comparing Polynomial Growth

For sufficiently large $n$:

$$  
n^2<n^3  
$$

Therefore:

$$  
\boxed{n^2=O(n^3)}  
$$

Likewise:

$$  
n<n^2<n^3  
$$

The higher polynomial degree eventually dominates the lower degree.

---

# 7. Exponential Growth

Expressions such as:

$$  
2^n,\quad3^n,\quad5^n  
$$

are **exponential functions**.

Their growth eventually becomes much faster than polynomial functions.

For example:

$$  
n^3<2^n  
$$

for sufficiently large $n$.

Therefore:

$$  
\boxed{n^3=O(2^n)}  
$$

---

## Important Comparison

$$  
\boxed{n^k<c^n}  
$$

eventually, for fixed:

$$  
k>0,\quad c>1  
$$

So:

$$  
\boxed{\text{Polynomial}<\text{Exponential}}  
$$

for sufficiently large $n$.

---

# 8. Constant Multipliers in Exponents

The lecture compares expressions such as:

$$  
2^n,\qquad 2^{3n}  
$$

Notice:

$$  
2^{3n}=(2^3)^n=8^n  
$$

So changing the constant **inside the exponent** can dramatically change the growth rate.

This is different from multiplying the entire function by a constant.

### Compare

$$  
3n^2=\Theta(n^2)  
$$

but:

$$  
2^{3n}\neq\Theta(2^n)  
$$

because:

$$  
2^{3n}=8^n  
$$

which grows much faster than $2^n$.

---

# 9. Growth-Rate Graph

The lecture's graph visually compares:

$$  
\log n,\ n,\ n\log n,\ n^2,\ n^3,\ 2^n  
$$

As $n$ increases:

```text
Slow
 ↑
 │ log n
 │
 │ n
 │
 │ n log n
 │
 │ n²
 │
 │ n³
 │
 │ 2ⁿ
 ↓
Fast
```

The exponential curve eventually shoots upward dramatically.

---

# 10. Practical Meaning of Growth Rates

The table on page 63 demonstrates how dramatically running times differ as $n$ becomes large.

### General hierarchy

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
1.5^n  
<  
2^n  
<  
n!  
}  
$$

### Key idea

An algorithm that is:

$$  
O(n)  
$$

can remain practical for very large $n$.

But:

$$  
O(2^n)  
$$

or:

$$  
O(n!)  
$$

becomes impractical extremely quickly.

---

# 11. Logarithm Basics ⭐

### Definition

$$  
\boxed{\log_b a=x\iff b^x=a}  
$$

Example:

$$  
\log_2 8=3  
$$

because:

$$  
2^3=8  
$$

---

# 12. Logarithm Properties

### Product Rule

$$  
\boxed{\log(ab)=\log a+\log b}  
$$

### Power Rule

$$  
\boxed{\log(a^n)=n\log a}  
$$

### Division Rule

$$  
\boxed{\log\frac{a}{b}=\log a-\log b}  
$$

### Reciprocal

$$  
\boxed{\log(1/a)=-\log a}  
$$

These identities are shown on page 64.

---

# 13. Change of Base Formula ⭐

One of the most important formulas:

$$  
\boxed{  
\log_b a=\frac{\log_c a}{\log_c b}  
}  
$$

Most commonly:

$$  
\boxed{  
\log_b a=\frac{\log a}{\log b}  
}  
$$

where the numerator and denominator use the **same base**.

### Example

# $$  
\log_2 8

\frac{\log 8}{\log2}  
=3  
$$

---

# 14. Very Important Log Identity

From:

$$  
a=b^{\log_b a}  
$$

we get:

$$  
\boxed{\log_b a}  
$$

as the exponent needed to convert $b$ into $a$.

The lecture also shows:

$$  
\boxed{  
\log_b c=\frac{\log_b a}{\log_b?}  
}  
$$

through the change-of-base relationship; the clean general form to retain is:

$$  
\boxed{\log_b a=\frac{\log_c a}{\log_c b}}  
$$

---

# 15. Logarithm vs Polynomial

A key asymptotic fact:

$$  
\boxed{(\log n)^k=o(n^\epsilon)}  
$$

for fixed $(k>0,\epsilon>0)$.

In simpler GATE terms:

$$  
\boxed{\log n<n^c}  
$$

for any fixed $(c>0)$, eventually.

So:

$$  
\log n<n<n^2<n^3  
$$

---

# 16. Chessboard + Rice Example 🍚

Page 66 introduces the classic **chessboard and grains of rice** example.

Suppose:

- First square → $1=2^0$ grain
    
- Second square → $2=2^1$
    
- Third square → $4=2^2$
    
- Fourth square → $8=2^3$
    

Each square **doubles** the previous amount.

So square $k$:

$$  
\boxed{2^{k-1}}  
$$

grains.

For a 64-square chessboard, the last square contains:

$$  
\boxed{2^{63}}  
$$

grains.

The total is:

$$  
1+2+4+\cdots+2^{63}  
$$

Using geometric series:

$$  
\boxed{2^{64}-1}  
$$

This demonstrates how **exponential growth becomes enormous extremely quickly**.

---

# 🎯 GATE CORNER

### Must Know

$$  
\boxed{\Theta=O+\Omega}  
$$

More precisely:

$$  
\Theta(g)=O(g)\cap\Omega(g)  
$$

### Dominant term

$$  
\boxed{n^2+100000=\Theta(n^2)}  
$$

### Growth

$$  
\boxed{  
\log n<n<n\log n<n^2<n^3<c^n<n!  
}  
$$

for appropriate fixed $(c>1)$.

### Logs

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
\boxed{\log_b a=\frac{\log a}{\log b}}  
$$

### Exponential

$$  
\boxed{c^{an}=(c^a)^n}  
$$

and exponential growth eventually dominates every fixed-degree polynomial.

---

## ⚡ 30-Second Revision

> **Ignore constants → focus on growth.**
> 
> **Highest polynomial power dominates.**
> 
> $$  
> n^2+1000=\Theta(n^2)  
> $$
> 
> **Polynomial < Exponential**
> 
> $$  
> n^k<2^n  
> $$
> 
> **Logarithms grow very slowly.**
> 
> $$  
> \log n<n  
> $$
> 
> **Change of base:**
> 
> $$  
> \boxed{\log_b a=\frac{\log a}{\log b}}  
> $$
> 
> **Chessboard rice → exponential growth:**
> 
> $$  
> \boxed{1+2+\cdots+2^{63}=2^{64}-1}  
> $$