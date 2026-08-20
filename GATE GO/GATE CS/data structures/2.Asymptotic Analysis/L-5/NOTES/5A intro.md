# DSA — Algorithm Analysis & Asymptotic Analysis

## 1. What is an Algorithm?

An **algorithm** is a finite, well-defined sequence of steps used to solve a problem.

Think:

```text
Input → Algorithm → Output
```

Example:

**Problem:** Find the largest element in an array.

```text
Input:  [4, 8, 2, 9, 1]

Algorithm:
1. Assume first element is largest.
2. Compare it with every remaining element.
3. Update largest whenever a bigger element is found.
4. Return largest.

Output: 9
```

### Important idea

An algorithm is **not the same as a program**.

- **Algorithm** → logical solution
    
- **Program** → implementation of that solution in C/C++/Java/Python etc.
    

---

# 2. Why Analyze an Algorithm?

The important question is:

> **Will my algorithm work efficiently when the input becomes large?**

Two major concerns:

### 1. Time

How much computational work does it require?

```text
Why is my program slow?
```

### 2. Space

How much extra memory does it require?

```text
Why does my program run out of memory?
```

So algorithm analysis mainly studies:

```text
Time Complexity
Space Complexity
```

The lecture introduces this as the practical challenge of handling large inputs.

---

# 3. Measuring Running Time

The obvious idea is:

> Run the program and measure how many seconds it takes.

For example:

```text
Algorithm A → 100 ms
Algorithm B → 500 ms
```

So we might conclude A is better.

### But there's a problem.

The same algorithm can run on different machines:

```text
Laptop A → 100 ms
Laptop B → 300 ms
Laptop C → 50 ms
```

Because machines can have different:

- CPU speeds
    
- instruction sets
    
- memory access speeds
    
- hardware architectures
    

Therefore:

> **Actual running time depends on the machine.**

This is called **a posteriori analysis** when we analyze performance after implementation/execution.

---

# 4. A Priori Analysis

Instead of asking:

> "How many seconds does this program take?"

we ask:

> **"How many basic operations does the algorithm perform?"**

This is much more useful.

For example:

```c
sum = 0;

for(i = 0; i < n; i++)
    sum = sum + a[i];
```

The loop performs approximately `n` iterations.

So we say:

```text
Work ∝ n
```

rather than:

```text
Time = 0.000003 seconds
```

This analysis is called **a priori analysis**.

### Core distinction

|Analysis|Idea|
|---|---|
|A posteriori|Measure actual execution|
|A priori|Analyze algorithm mathematically|

For DSA/GATE, **a priori analysis is the important one**.

---

# 5. Input Size Matters

An algorithm's running time depends on the **size of its input**.

Suppose:

```text
Array = [5, 2, 8, 1, 9]
```

Input size:

```text
n = 5
```

For:

```text
A = [1,2,3,...,n]
```

there are `n` elements.

If an algorithm sorts the array:

```text
Array → Sorting Algorithm → Sorted Array
```

then its complexity is expressed as a function of `n`.

```text
T(n)
```

where:

> `T(n)` = amount of work required for input size `n`.

The lecture uses examples such as linked-list insertion and sorting to motivate input size and running-time analysis.

---

# 6. Step Count

Instead of counting seconds, count **basic operations/steps**.

Example:

```c
sum = 0;

for(i = 1; i <= n; i++)
    sum = sum + a[i];

return sum;
```

Approximate step counting:

```text
sum = 0          → 1
loop operations  → depends on n
sum = sum+a[i]   → n
return           → 1
```

The exact count might look like:

```text
T(n) = 2n + 3
```

The lecture explicitly demonstrates this kind of step-count analysis.

### But...

Do we really want to count every single operation in a 1000-line program?

**No.**

That's where asymptotic analysis comes in.

---

# 7. Example: Nested Loops

Consider:

```c
for(i = 1; i <= m; i++)
{
    for(j = 1; j <= n; j++)
    {
        c[i][j] = a[i][j] + b[i][j];
    }
}
```

Outer loop:

```text
m times
```

Inner loop:

```text
n times for each outer iteration
```

Therefore:

```text
Total = m × n
```

So:

```text
T(m,n) = Θ(mn)
```

If:

```text
m = n
```

then:

```text
T(n) = Θ(n²)
```

The lecture's step-count example derives a linear term from loop-control operations and an `mn` term from the nested body.

---

# 8. Exact Step Count Isn't the Goal

Suppose:

```text
T(n) = 3n + 2
```

Do we really care about the `3` and `2`?

For large `n`:

```text
3n + 2 ≈ n
```

The important thing is that the function grows **linearly**.

Similarly:

```text
n² + n + 10 ≈ n²
```

So asymptotic analysis focuses on the **dominant growth term**.

This is the transition made by the lecture from exact step counting to asymptotic analysis.

---

# 9. Asymptotic Analysis

### Definition

**Asymptotic analysis** studies how the running time/space of an algorithm grows as the input size becomes very large.

We intentionally ignore:

- constants
    
- lower-order terms
    
- machine-dependent details
    

Example:

```text
T(n) = 3n² + 7n + 100
```

Dominant term:

```text
n²
```

Therefore:

```text
T(n) = Θ(n²)
```

### Mental model

Think of it as zooming out.

For small `n`:

```text
3n² + 7n + 100
```

contains many details.

For huge `n`:

```text
n²
```

dominates everything else.

---

# 10. Why Ignore Constants?

Suppose:

```text
T₁(n) = 2n
T₂(n) = 100n
```

Both are:

```text
Θ(n)
```

They differ significantly in actual speed, but both have the **same growth rate**.

Similarly:

```text
2n² + 100n + 500
```

and

```text
100n² + n
```

are both:

```text
Θ(n²)
```

The lecture emphasizes that asymptotic analysis removes unnecessary details and focuses on the significant term.

---

# 11. `T(n)` — Running Time Function

We represent algorithmic work as:

```text
T(n)
```

Example:

```text
T(n) = n² + n
```

For large `n`:

```text
T(n) ≈ n²
```

Therefore:

```text
T(n) = Θ(n²)
```

### Important

`T(n)` doesn't necessarily mean **literal seconds**.

It represents the **growth of computational work** with input size.

---

# 12. Asymptotic Notations

The three major notations are:

```text
Big-O       O
Big-Omega   Ω
Theta       Θ
```

And two stricter notations:

```text
little-o    o
little-omega ω
```

For GATE/DSA, the first three are the most important initially.

---

# 13. Big-O — `O(g(n))`

Big-O gives an **asymptotic upper bound**.

Informally:

> **The algorithm will not grow faster than this bound, up to a constant factor, for sufficiently large `n`.**

Mathematically:

```text
T(n) = O(g(n))
```

means there exist constants `c > 0` and `n₀` such that:

```text
T(n) ≤ c·g(n)
```

for all:

```text
n ≥ n₀
```

The lecture defines Big-O using exactly this eventual upper-bound idea.

### Example

```text
T(n) = 3n + 2
```

We can say:

```text
T(n) = O(n)
```

because eventually:

```text
3n + 2 ≤ 4n
```

for sufficiently large `n`.

---

# 14. Big-O Intuition

Imagine two curves:

```text
        cg(n)
          /
         /
 T(n)   /
       /
------/--------------→ n
     n₀
```

After some point `n₀`, the upper function `cg(n)` stays above `T(n)`.

Therefore:

```text
T(n) = O(g(n))
```

### Example

```text
T(n) = n² + n
```

Since:

```text
n² + n ≤ 2n²
```

for `n ≥ 1`,

```text
T(n) = O(n²)
```

---

# 15. Big-Omega — `Ω(g(n))`

Big-Omega gives an **asymptotic lower bound**.

Informally:

> **The algorithm takes at least this much work asymptotically.**

Mathematically:

```text
T(n) = Ω(g(n))
```

if:

```text
T(n) ≥ c·g(n)
```

for sufficiently large `n`.

The lecture introduces Ω as the lower-bound counterpart to O.

### Example

```text
T(n) = 3n² + 5n
```

Clearly:

```text
T(n) ≥ 3n²
```

Therefore:

```text
T(n) = Ω(n²)
```

---

# 16. Big-Theta — `Θ(g(n))`

Theta gives a **tight asymptotic bound**.

This means:

```text
T(n) = O(g(n))
```

AND

```text
T(n) = Ω(g(n))
```

Therefore:

```text
T(n) = Θ(g(n))
```

Mathematically:

```text
c₁g(n) ≤ T(n) ≤ c₂g(n)
```

for sufficiently large `n`.

The lecture defines Θ as the combination of upper and lower bounds.

### Example

```text
T(n) = 3n² + 5n + 10
```

It is:

```text
O(n²)
```

and:

```text
Ω(n²)
```

Therefore:

```text
Θ(n²)
```

---

# 17. The Most Important Mental Picture

Remember these three like this:

```text
             T(n)

O(g(n))      ↑ Upper bound
             |
             T(n)
             |
Ω(g(n))      ↓ Lower bound
```

More precisely:

```text
O  → T(n) grows NO FASTER than g(n)
Ω  → T(n) grows AT LEAST as fast as g(n)
Θ  → T(n) grows at THE SAME ASYMPTOTIC RATE as g(n)
```

### Example

```text
T(n) = n
g(n) = n²
```

Since:

```text
n ≤ n²
```

for `n ≥ 1`:

```text
T(n) = O(n²)
```

But:

```text
T(n) ≠ Ω(n²)
```

because `n` does **not** eventually grow at least as fast as `n²`.

---

# 18. Quick Revision — Pages 1–18

### Algorithm

```text
Algorithm = sequence of steps to solve a problem
```

### Two major resources

```text
Time → how much computational work?
Space → how much memory?
```

### Running-time analysis

Actual seconds depend on:

```text
Machine
CPU speed
Instruction set
Memory speed
```

Therefore, instead of seconds, analyze **number of operations**.

### Two approaches

```text
A posteriori → execute + measure
A priori     → mathematically analyze
```

### Running-time function

```text
T(n)
```

where `n` represents input size.

### Exact step count

```text
T(n) = 3n + 2
```

Useful for derivation, but tedious.

### Asymptotic analysis

Ignore:

```text
constants
lower-order terms
```

Focus on:

```text
dominant growth
```

Example:

```text
5n² + 3n + 10
       ↓
     n²
```

Therefore:

```text
Θ(n²)
```

### Main notations

|Notation|Meaning|
|---|---|
|`O(g(n))`|Upper bound|
|`Ω(g(n))`|Lower bound|
|`Θ(g(n))`|Tight bound|
|`o(g(n))`|Strictly smaller growth|
|`ω(g(n))`|Strictly larger growth|

### Golden mental model

```text
             Θ(g(n))
        ┌──────────────┐
        │    T(n)      │
        └──────────────┘
       /                \
      /                  \
 Ω(g(n))              O(g(n))
 lower                 upper
```

**The key idea from pages 1–18 is not memorizing O/Ω/Θ.**

It's this:

> **Don't ask "How many seconds does my algorithm take?" Ask "As the input gets bigger, how does the amount of work grow?"**

That shift—from **machine-dependent time → input-dependent growth**—is the foundation for everything that comes next.