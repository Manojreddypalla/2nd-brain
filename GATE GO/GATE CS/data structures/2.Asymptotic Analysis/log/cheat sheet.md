# 📌 Logarithms — GATE CSE Cheat Sheet

## 1. Basic Definition

If:

$$  
a^x=N  
$$

then:

$$  
\boxed{\log_a N=x}  
$$

### Conditions

$$  
\boxed{a>0,\quad a\neq1,\quad N>0}  
$$

---

# 2. Essential Log Rules ⭐⭐⭐

### Product

$$  
\boxed{\log_a(xy)=\log_a x+\log_a y}  
$$

### Division

$$  
\boxed{\log_a\left(\frac{x}{y}\right)=\log_a x-\log_a y}  
$$

### Power

$$  
\boxed{\log_a(x^k)=k\log_a x}  
$$

### Root

$$  
\boxed{\log_a\sqrt[k]{x}=\frac{1}{k}\log_a x}  
$$

### Special values

$$  
\boxed{\log_a1=0}  
$$

$$  
\boxed{\log_aa=1}  
$$

---

# 3. Change of Base ⭐⭐⭐

$$  
\boxed{\log_a x=\frac{\log_bx}{\log_ba}}  
$$

Commonly:

$$  
\boxed{\log_a x=\frac{\ln x}{\ln a}}  
$$

### Important consequence

For constant $a,b>1$:

$$  
\boxed{\log_a n=\Theta(\log_b n)}  
$$

**In asymptotic analysis, log base usually doesn't matter.**

---

# 4. Reciprocal & Chain Identities ⭐⭐⭐

### Reciprocal

$$  
\boxed{\log_ab=\frac{1}{\log_ba}}  
$$

### Chain

$$  
\boxed{\log_ab\cdot\log_bc=\log_ac}  
$$

### Cyclic

$$  
\boxed{\log_ab\cdot\log_bc\cdot\log_ca=1}  
$$

---

# 5. Exponential ↔ Log Conversion ⭐⭐⭐

$$  
\boxed{a^x=2^{x\log_2a}}  
$$

Since $\log_2a$ is constant:

$$  
\boxed{a^n=2^{\Theta(n)}}  
$$

This is useful when comparing exponential functions.

---

# 6. ⭐⭐⭐⭐⭐ GATE Trick: $n^{\log n}$

Convert $n$ into base 2:

$$  
n=2^{\log_2n}  
$$

Therefore:

# $$  
n^{\log_2n}

\left(2^{\log_2n}\right)^{\log_2n}  
$$

$$  
\boxed{n^{\log n}=2^{(\log n)^2}}  
$$

Now compare with:

$$  
2^n  
$$

Since:

$$  
(\log n)^2=o(n)  
$$

we get:

$$  
\boxed{n^{\log n}=o(2^n)}  
$$

### Pattern to recognize

Whenever you see:

$$  
\boxed{n^{f(n)}}  
$$

think:

$$  
\boxed{n^{f(n)}=2^{f(n)\log_2n}}  
$$

---

# 7. Growth Hierarchy ⭐⭐⭐⭐⭐

For GATE, this is the important pattern:

$$  
\boxed{  
\log\log n  
\ll  
\log n  
\ll  
(\log n)^k  
\ll  
n^\epsilon  
\ll  
n  
\ll  
n\log n  
\ll  
n^k  
\ll  
c^n  
\ll  
n!  
}  
$$

where:

$$  
k>0,\qquad \epsilon>0,\qquad c>1  
$$

### Two critical facts

$$  
\boxed{(\log n)^k=o(n^\epsilon)}  
$$

and:

$$  
\boxed{n^k=o(c^n)}  
$$

So even:

$$  
(\log n)^{1000}\ll n^{0.01}  
$$

and:

$$  
n^{100}\ll2^n  
$$

eventually.

---

# 8. Simplifying Logs in Complexity

### Polynomial inside log

$$  
\log(n^k)=k\log n  
$$

Therefore:

$$  
\boxed{\log(n^k)=\Theta(\log n)}  
$$

Example:

$$  
\boxed{\log(n^{100})=\Theta(\log n)}  
$$

### Exponential inside log

$$  
\log(a^n)=n\log a  
$$

Since $\log a$ is constant:

$$  
\boxed{\log(a^n)=\Theta(n)}  
$$

---

# 9. Log of a Product

Example:

$$  
f(n)=n^2 2^n  
$$

Then:

# $$  
\log f(n)

\log(n^2 2^n)  
$$

$$  
=2\log n+n\log2  
$$

Since $n$ dominates $\log n$:

$$  
\boxed{\log f(n)=\Theta(n)}  
$$

**GATE pattern:** after taking a log, identify the dominant term.

---

# 10. Logarithm in Algorithms ⭐⭐⭐

### Binary Search

Repeated halving:

$$  
n\rightarrow\frac n2\rightarrow\frac n4\rightarrow\cdots  
$$

After $k$ steps:

$$  
\frac{n}{2^k}=1  
$$

Therefore:

$$  
2^k=n  
$$

$$  
\boxed{k=\log_2n}  
$$

Hence:

$$  
\boxed{T(n)=\Theta(\log n)}  
$$

### General division

If:

$$  
n\rightarrow\frac n c  
$$

then:

$$  
c^k=n  
$$

so:

$$  
\boxed{k=\log_cn=\Theta(\log n)}  
$$

**Pattern:** repeated division by a constant → **logarithmic complexity**.

---

# 11. Logarithm in Recurrences

Example:

$$  
T(n)=T(n/2)+1  
$$

Expansion:

$$  
T(n)=T(n/2)+1  
$$

$$  
=T(n/4)+2  
$$

$$  
=T(n/8)+3  
$$

After $k$ levels:

$$  
T(n)=T(n/2^k)+k  
$$

Stop when:

$$  
\frac{n}{2^k}=1  
$$

Therefore:

$$  
k=\log_2n  
$$

Hence:

$$  
\boxed{T(n)=\Theta(\log n)}  
$$

---

# 12. Logarithm Monotonicity ⚠️

If:

$$  
a>1  
$$

then $\log_a n$ is increasing:

$$  
x>y\Rightarrow\log_ax>\log_ay  
$$

If:

$$  
0<a<1  
$$

the inequality reverses.

**GATE trap:** check the base when logarithms are involved in inequalities.

---

# 13. Common GATE Traps ⚠️

### ❌ Addition

$$  
\log(a+b)\neq\log a+\log b  
$$

### ❌ Subtraction

$$  
\log(a-b)\neq\log a-\log b  
$$

### ❌ Power

$$  
\log(a^b)\neq(\log a)^b  
$$

Correct:

$$  
\boxed{\log(a^b)=b\log a}  
$$

### ❌ Assuming O is tight

$$  
n=O(n^2)  
$$

but:

$$  
\boxed{n\neq\Theta(n^2)}  
$$

---

# 🎯 GATE Ultra-High-Yield

If you remember only these, you're covered for most GATE asymptotic-log questions:

$$  
\boxed{\log_a(xy)=\log_ax+\log_ay}  
$$

$$  
\boxed{\log_a(x/y)=\log_ax-\log_ay}  
$$

$$  
\boxed{\log_a(x^k)=k\log_ax}  
$$

$$  
\boxed{\log_ax=\frac{\log_bx}{\log_ba}}  
$$

$$  
\boxed{\log_ab=\frac1{\log_ba}}  
$$

$$  
\boxed{\log_ab\cdot\log_bc=\log_ac}  
$$

$$  
\boxed{\log_a(a^x)=x}  
$$

$$  
\boxed{a^{\log_ax}=x}  
$$

$$  
\boxed{\log_an=\Theta(\log_bn)}  
$$

### Growth

$$  
\boxed{  
\log\log n  
\ll  
\log n  
\ll  
(\log n)^k  
\ll  
n^\epsilon  
\ll  
n^k  
\ll  
c^n  
\ll  
n!  
}  
$$

### Most important GATE transformations

$$  
\boxed{n^{\log n}=2^{(\log n)^2}}  
$$

$$  
\boxed{n^k=o(c^n)}  
$$

$$  
\boxed{(\log n)^k=o(n^\epsilon)}  
$$

$$  
\boxed{\log(n^k)=\Theta(\log n)}  
$$

$$  
\boxed{\log(a^n)=\Theta(n)}  
$$

**Mental trigger:**  
**Repeated division → $\log n$**  
**Polynomial → highest power**  
**Log of polynomial → $\log n$**  
**Log of exponential → $n$**  
**Polynomial ≪ exponential**  
**Change of log base → irrelevant for Θ**