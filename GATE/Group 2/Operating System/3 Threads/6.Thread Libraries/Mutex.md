# Mutex (Mutual Exclusion)

> **Goal:** Understand why Mutex was introduced, how it works internally, and how it prevents race conditions.

---

# Why Was Mutex Introduced?

Imagine two threads share the same bank account.

```
Balance = ₹1000
```

Two threads execute simultaneously.

```
Thread A

Withdraw ₹500
```

```
Thread B

Withdraw ₹700
```

Both execute at the same time.

---

# Without Mutex

Both threads read:

```
Balance = 1000
```

Thread A calculates:

```
1000 - 500 = 500
```

Thread B calculates:

```
1000 - 700 = 300
```

Now both write back.

Possible result:

```
Balance = 500
```

or

```
Balance = 300
```

Both are incorrect.

This is called a **Race Condition**.

---

# What is a Race Condition?

A **Race Condition** occurs when:

- Multiple threads access shared data simultaneously.
- At least one thread modifies the data.
- The final result depends on the order (timing) of execution.

---

# Solution

Only **one thread** should access the shared resource at a time.

This principle is called:

> **Mutual Exclusion**

Hence the name:

```
MUTual EXclusion

↓

MUTEX
```

---

# Definition

A **Mutex** is a synchronization mechanism that allows **only one thread at a time** to enter a **Critical Section**.

---

# What is a Critical Section?

A **Critical Section** is the part of a program where shared data or shared resources are accessed.

Example:

```c
balance = balance - 500;
```

Since `balance` is shared, this statement is inside the critical section.

---

# Internal Working

Initially:

```
Mutex = UNLOCKED
```

---

## Step 1

Thread A reaches the critical section.

```
Thread A

↓

Lock Mutex
```

Mutex becomes:

```
LOCKED
```

Thread A enters the critical section.

---

## Step 2

Thread B arrives.

```
Thread B

↓

Lock Mutex
```

But the mutex is already locked.

So Thread B waits.

---

## Step 3

Thread A finishes.

```
Unlock Mutex
```

Mutex becomes available.

---

## Step 4

Thread B acquires the mutex and enters the critical section.

---

# Visualization

Without Mutex

```
Thread A ────────┐

                 ├── Shared Data

Thread B ────────┘
```

Both access simultaneously.

---

With Mutex

```
Thread A

↓

LOCK

↓

Critical Section

↓

UNLOCK

↓

Thread B

↓

LOCK

↓

Critical Section

↓

UNLOCK
```

Only one thread enters at a time.

---

# Mutex APIs (POSIX Threads)

Lock:

```c
pthread_mutex_lock(&mutex);
```

Unlock:

```c
pthread_mutex_unlock(&mutex);
```

---

# Real-World Example

Think of a **single-person bathroom** 🚻.

```
Person A

↓

Locks Door

↓

Uses Bathroom

↓

Unlocks Door
```

While Person A is inside,

```
Person B

↓

Waits
```

Only after the door is unlocked can Person B enter.

The bathroom is the **critical section**.

The lock on the door is the **mutex**.

---

# Advantages

- Prevents race conditions.
- Ensures data consistency.
- Guarantees mutual exclusion.
- Simple to use.

---

# Disadvantages

- Threads may have to wait.
- Incorrect use can cause deadlocks.
- Too many mutexes reduce concurrency.

---

# GATE Corner ⭐

## Must Remember

- **Mutex = Mutual Exclusion.**
- Allows **only one thread** into the critical section at a time.
- Used to protect **shared resources**.
- Prevents **race conditions**.
- A thread must **lock** before entering and **unlock** after leaving.

---

## Common GATE Traps ⚠️

❌ Mutex allows multiple threads to enter the critical section.

✔ False.

Only **one** thread can own a mutex at a time.

---

❌ Mutex prevents deadlocks.

✔ False.

Improper use of mutexes can **cause** deadlocks.

---

❌ Mutex is used only between processes.

✔ False.

Mutexes are primarily used between **threads** (though some implementations support inter-process mutexes).

---

❌ Mutex increases parallel execution inside the critical section.

✔ False.

It intentionally **serializes** access to shared data to keep it correct.

---

# PYQ Focus 🎯

- Race Condition
- Critical Section
- Mutual Exclusion
- Lock vs Unlock
- Mutex vs Semaphore (very common)
- Deadlock due to improper locking