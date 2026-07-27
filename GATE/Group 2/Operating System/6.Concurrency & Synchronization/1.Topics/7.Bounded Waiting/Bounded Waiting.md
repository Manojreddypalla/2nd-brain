# Bounded Waiting

---

# Definition

**Bounded Waiting** is a property that ensures:

> **After a process requests to enter the Critical Section, there exists a finite limit on the number of times other processes are allowed to enter before it.**

In simple words:

> **No process should wait forever.**

Every waiting process must eventually get a turn.

---

# Why Do We Need Bounded Waiting?

Suppose Process P1 wants to enter the Critical Section.

```text
P1 → Waiting
```

But every time the Critical Section becomes free,

P2 enters.

Then again,

P2 enters.

Then again,

P2 enters.

```text
P2

↓

Critical Section

↓

Leaves

↓

Enters Again

↓

Leaves

↓

Enters Again
```

P1 never gets a chance.

This is called **Starvation**.

Bounded Waiting prevents this.

---

# Without Bounded Waiting

```text
P1 → Waiting

↓

P2 enters

↓

P2 enters

↓

P2 enters

↓

P2 enters

↓

Forever...
```

P1 waits forever.

❌ Starvation

---

# With Bounded Waiting

```text
P1 → Waiting

↓

P2 enters

↓

P3 enters

↓

P1 enters
```

Others may enter first,

but only a **limited number of times**.

Eventually,

P1 must be allowed to enter.

---

# What Does "Bounded" Mean?

"Bounded" means **there is an upper limit**.

Example:

```text
Maximum Waiting = 2
```

If P1 starts waiting,

at most two other processes may enter first.

After that,

P1 **must** enter.

---

# Real-Life Analogy

Imagine standing in a queue at a ticket counter.

```text
P1

↓

P2

↓

P3
```

Correct queue:

```text
P2 served

↓

P3 served

↓

P1 served
```

Wrong queue:

```text
P2 served

↓

New Person arrives

↓

P2 again

↓

Another new person

↓

P2 again

↓

Forever...
```

P1 never reaches the counter.

This violates Bounded Waiting.

---

# Characteristics

- Prevents starvation.
- Guarantees fairness.
- Every waiting process eventually enters.
- Waiting time is finite.
- Completes the Critical Section requirements.

---

# Bounded Waiting vs Progress

### Progress

Question:

> If nobody is inside the Critical Section, who enters next?

Focus:

Selecting the next process.

---

### Bounded Waiting

Question:

> Will a waiting process eventually get its turn?

Focus:

Fairness over time.

---

# Bounded Waiting vs Mutual Exclusion

### Mutual Exclusion

Only one process enters.

---

### Bounded Waiting

Everyone eventually gets a chance.

---

# Example

Suppose:

```text
Critical Section

↓

P2

↓

P3

↓

P2

↓

P3

↓

P2

↓

...
```

P1 waits forever.

Mutual Exclusion is satisfied.

Progress is satisfied.

But

Bounded Waiting is violated.

---

# Why It Matters

Without Bounded Waiting:

- Starvation occurs.
- Some processes may never execute.
- System becomes unfair.
- Long-running applications suffer.

---

# GATE Points ⭐

- Prevents starvation.
- Guarantees fairness.
- Waiting time is finite.
- Places an upper bound on waiting.
- Third requirement of the Critical Section Problem.

---

# Quick Revision ⭐

- No starvation.
- Finite waiting.
- Fair scheduling.
- Everyone gets a turn.