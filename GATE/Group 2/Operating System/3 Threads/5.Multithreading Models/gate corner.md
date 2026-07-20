# GATE Corner ⭐ — Multithreading Models

## Core Concept

> **Multithreading Model = Mapping between User-Level Threads (ULTs) and Kernel-Level Threads (KLTs).**

The mapping determines:

- Parallel execution
- Blocking behavior
- Kernel overhead
- Thread management
- Performance

---

# Quick Mapping

```
Many-to-One

ULT ULT ULT
   │
   ▼
 1 KLT
```

```
One-to-One

ULT ─► KLT

ULT ─► KLT

ULT ─► KLT
```

```
Many-to-Many

ULT ULT ULT ULT
   │  │  │
   ▼  ▼  ▼
 KLT KLT KLT
```

```
Two-Level

Many-to-Many

+

Some ULTs are permanently bound to KLTs.
```

---

# Comparison Table

| Feature | Many-to-One | One-to-One | Many-to-Many | Two-Level |
|----------|-------------|------------|--------------|-----------|
| Mapping | Many : 1 | 1 : 1 | Many : Many | Many : Many + Binding |
| Parallelism | ❌ No | ✅ Yes | ✅ Yes | ✅ Yes |
| Blocking Call | Entire Process | One Thread | One KLT | One KLT |
| Kernel Aware | One KLT Only | Every Thread | Every KLT | Every KLT |
| Context Switch | Fast | Slower | Medium | Medium |
| Kernel Overhead | Low | High | Medium | Medium |
| Complexity | Low | Low | High | Highest |
| Modern Usage | Rare | Linux, Windows, macOS | Rare | Rare |

---

# Must Remember

### Many-to-One

- Many ULTs → One KLT
- Managed mostly by Thread Library
- Fast
- No true parallelism
- One blocking call blocks entire process

---

### One-to-One

- One ULT → One KLT
- Kernel manages every thread
- True parallelism
- Blocking affects only one thread
- Higher kernel overhead

---

### Many-to-Many

- Many ULTs → Multiple KLTs
- Combination of ULT and KLT advantages
- Supports parallelism
- Lower overhead than One-to-One
- Complex implementation

---

### Two-Level

- Same as Many-to-Many
- Some ULTs can be permanently bound to specific KLTs
- Provides guaranteed kernel thread for important user threads

---

# Common GATE Traps ⚠️

❌ Multithreading Model decides CPU Scheduling.

✔ False.

It decides **ULT ↔ KLT mapping**, not FCFS, RR, SJF, etc.

---

❌ One-to-One means one process has one thread.

✔ False.

It means **one User-Level Thread maps to one Kernel-Level Thread.**

---

❌ Many-to-One supports true parallel execution.

✔ False.

Kernel sees only one KLT.

---

❌ Blocking one ULT in Many-to-One blocks only that thread.

✔ False.

Entire process blocks.

---

❌ Linux uses Many-to-One.

✔ False.

Linux uses **One-to-One**.

---

❌ Many-to-Many eliminates kernel overhead completely.

✔ False.

Kernel still manages KLTs, so overhead exists.

---

# PYQ Focus 🎯

- ULT ↔ KLT Mapping
- Blocking behavior
- Parallel execution
- Kernel awareness
- One-to-One vs Many-to-One
- Many-to-Many advantages
- Two-Level model
- Modern OS thread model (Linux/Windows → One-to-One)
- Advantages & disadvantages of each model

---

# Memory Trick 🧠

```
Many-to-One

Fast ✅
Parallel ❌
Blocking ❌


One-to-One

Parallel ✅
Blocking Solved ✅
Overhead High


Many-to-Many

Balanced ⚖️
Complex ⚠️


Two-Level

Many-to-Many

+

Permanent Binding
```