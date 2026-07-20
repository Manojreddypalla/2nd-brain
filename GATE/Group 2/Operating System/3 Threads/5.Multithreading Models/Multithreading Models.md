# Multithreading Models

> **Goal:** Understand how User-Level Threads (ULTs) are mapped to Kernel-Level Threads (KLTs), why different models were created, and their trade-offs.

---

# Why Were Multithreading Models Introduced?

We have learned two types of threads:

## User-Level Threads (ULT)

- Managed by Thread Library
- Very Fast
- Kernel is unaware

Problems:
- Blocking system calls block the entire process
- No true parallelism

---

## Kernel-Level Threads (KLT)

- Managed by Kernel
- Kernel schedules every thread
- Supports parallel execution

Problems:
- Thread creation is expensive
- Context switching is expensive
- Frequent system calls

---

Neither approach is perfect.

| ULT | KLT |
|------|------|
| Fast | Slower |
| Cheap | Expensive |
| No parallelism | Parallelism |
| Kernel unaware | Kernel aware |

OS designers wanted to combine the advantages of both.

This led to **Multithreading Models**.

---

# Core Idea

A multithreading model defines:

> **How User-Level Threads are mapped to Kernel-Level Threads.**

Think of it as:

```
Application Threads
        │
        ▼
Kernel Threads
        │
        ▼
CPU
```

Different mappings produce different performance and behavior.

---

# Types of Multithreading Models

There are four classical models:

1. Many-to-One
2. One-to-One
3. Many-to-Many
4. Two-Level Model

---

# 1. Many-to-One Model

## Structure

```
ULT A ─┐
ULT B ─┼────► KLT 1 ─────► CPU
ULT C ─┘
```

Many User Threads map to **one** Kernel Thread.

---

## Internal Working

Application creates:

```
Thread A
Thread B
Thread C
```

Thread Library schedules them.

Kernel only sees:

```
One Kernel Thread
```

CPU executes that Kernel Thread.

Kernel has **no idea** multiple user threads exist.

---

## Advantages

- Very fast thread creation
- Very fast context switching
- Low memory usage
- No kernel involvement during thread scheduling

---

## Disadvantages

### Blocking Problem

```
Thread A

↓

read()

↓

Kernel blocks KLT

↓

All ULTs stop
```

---

### No Parallel Execution

```
8 CPU Cores

↓

Kernel sees

1 KLT

↓

Only one core executes
```

---

## GATE Points

✔ Kernel schedules only one KLT.

✔ Thread library schedules ULTs.

✔ No true parallelism.

✔ One blocking call blocks entire process.

---

# 2. One-to-One Model

## Structure

```
ULT A ─────► KLT A

ULT B ─────► KLT B

ULT C ─────► KLT C
```

Each User Thread has one corresponding Kernel Thread.

---

## Internal Working

Application creates:

```
3 Threads
```

Kernel also creates:

```
3 Kernel Threads
```

Scheduler can execute any thread independently.

---

## Advantages

- True parallelism
- Independent scheduling
- Blocking affects only one thread
- Better responsiveness

---

## Disadvantages

Every user thread requires:

- Kernel Thread
- Kernel Stack
- Scheduling Data

More threads mean:

- More memory
- Higher creation cost
- Slower context switching

---

## GATE Points

✔ Linux uses this model.

✔ Windows uses this model.

✔ Most modern operating systems use this model.

---

# 3. Many-to-Many Model

## Structure

```
ULT A ─┐
ULT B ─┼────► KLT 1
ULT C ─┤
ULT D ─┘

          KLT 2

↓

CPU
```

Many User Threads are mapped onto **multiple** Kernel Threads.

Example:

```
100 ULTs

↓

8 KLTs

↓

8 CPU Cores
```

---

## Internal Working

Thread Library schedules User Threads.

Kernel schedules Kernel Threads.

Both cooperate.

```
ULT Scheduler

↓

KLT Scheduler

↓

CPU
```

---

## Advantages

- Supports parallelism
- Lower kernel overhead
- Better scalability
- Blocking one KLT does not stop all threads

---

## Disadvantages

- Very complex implementation
- Two schedulers must cooperate
- Difficult to maintain

---

# 4. Two-Level Model

Very similar to Many-to-Many.

Difference:

Some User Threads can be **bound permanently** to specific Kernel Threads.

Example:

```
ULT A ─────► KLT A

ULT B ─┐
ULT C ─┴────► Shared KLT
```

Useful when certain threads require guaranteed execution.

---

# Comparison Table

| Feature | Many-to-One | One-to-One | Many-to-Many | Two-Level |
|----------|-------------|------------|--------------|-----------|
| ULT : KLT | Many : 1 | 1 : 1 | Many : Many | Many : Many + Binding |
| Parallelism | ❌ | ✅ | ✅ | ✅ |
| Blocking | Whole Process | One Thread | Limited | Limited |
| Kernel Overhead | Low | High | Medium | Medium |
| Complexity | Low | Low | High | Highest |
| Modern Usage | Rare | Linux, Windows | Rare | Rare |

---

# Why Modern OS Use One-to-One?

Because modern CPUs have:

- Multiple Cores
- Large Memory
- Fast Context Switching

Performance gained from true parallelism outweighs kernel overhead.

Therefore,

Linux, Windows, and macOS primarily use **One-to-One**.

---

# GATE Corner ⭐

## Must Remember

- **Multithreading Model = Mapping between ULTs and KLTs.**
- Mapping determines:
  - Parallelism
  - Blocking behavior
  - Performance
  - Kernel overhead

---

## Common GATE Traps ⚠️

❌ Multithreading model defines scheduling algorithm.

✔ False.

It defines **thread mapping**, not FCFS/RR/Priority scheduling.

---

❌ One-to-One means one process has one thread.

✔ False.

It means **one User Thread maps to one Kernel Thread**.

---

❌ Many-to-One supports parallel execution.

✔ False.

Kernel sees only one KLT.

---

❌ Linux uses Many-to-One.

✔ False.

Linux uses **One-to-One**.

---

# PYQ Focus 🎯

- ULT ↔ KLT Mapping
- Blocking behavior
- Parallel execution
- Kernel awareness
- One-to-One vs Many-to-One
- Many-to-Many advantages
- Which model Linux/Windows use