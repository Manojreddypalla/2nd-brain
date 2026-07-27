# Mutual Exclusion

---

# Definition

**Mutual Exclusion (ME)** is a property that ensures **only one process or thread can execute its Critical Section at any given time**.

If one process is inside the Critical Section, all other processes attempting to enter must wait.

---

# Why is Mutual Exclusion Needed?

Suppose two threads update the same variable.

```text
Counter = 10
```

Thread A

```c
counter++;
```

Thread B

```c
counter++;
```

If both enter the Critical Section together,

they may produce

```text
Counter = 11
```

instead of

```text
Counter = 12
```

This is a **Race Condition**.

Mutual Exclusion prevents this.

---

# Working

Without Mutual Exclusion

```text
Thread A

        ↘

     Critical Section

        ↗

Thread B
```

Both enter together.

❌ Race Condition

---

With Mutual Exclusion

```text
Thread A

↓

Critical Section

↓

Leaves

↓

Thread B

↓

Critical Section
```

Only one thread executes at a time.

---

# Real-Life Analogy

Imagine an ATM.

Only one customer can use it.

```text
Customer A

↓

ATM

↓

Customer B waits
```

When Customer A finishes,

Customer B enters.

The ATM is the **Critical Section**.

The queue outside enforces **Mutual Exclusion**.

---

# Characteristics

- One execution unit at a time.
- Protects shared resources.
- Prevents race conditions.
- Foundation of synchronization.
- Applies only to the Critical Section.

---

# How is Mutual Exclusion Achieved?

Common mechanisms:

- Peterson Algorithm
- Test-and-Set
- Compare-and-Swap
- Mutex
- Semaphore
- Monitor

All of these have one goal:

> Ensure that **only one execution unit enters the Critical Section**.

---

# Example

Without synchronization

```c
// Thread A
count++;

// Thread B
count++;
```

Both may execute together.

---

Using a Mutex

```c
lock();

count++;

unlock();
```

Now,

only one thread can update `count` at a time.

---

# Mutual Exclusion vs Synchronization

| Mutual Exclusion | Synchronization |
|------------------|-----------------|
| Prevents simultaneous access | Coordinates execution of tasks |
| Protects Critical Section | Broader concept |
| One process at a time | May involve ordering or signaling |

> Mutual Exclusion is **one aspect of synchronization**.

---

# Advantages

- Prevents race conditions.
- Protects shared data.
- Ensures data consistency.
- Improves program correctness.

---

# Disadvantages

If implemented poorly:

- Waiting
- Blocking
- Reduced parallelism
- Deadlocks (possible)

---

# Important Observation

Mutual Exclusion does **not** guarantee fairness.

Example

```text
Thread A

↓

Critical Section

↓

Leaves

↓

Thread A enters again

↓

Thread B keeps waiting
```

Mutual Exclusion is satisfied,

but Thread B may starve.

This is why we also need:

- Progress
- Bounded Waiting

---

# GATE Points ⭐

- Only one process/thread can execute the Critical Section at a time.
- Prevents race conditions.
- Protects shared resources.
- Does not guarantee fairness.
- One of the three requirements of the Critical Section Problem.

---

# Quick Revision ⭐

- One process at a time.
- Protect Critical Section.
- Prevent Race Condition.
- Not enough alone.
- Needs Progress and Bounded Waiting too.