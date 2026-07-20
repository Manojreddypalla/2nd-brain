# GATE Corner ⭐

## Must Remember

- **Kernel-Level Threads (KLTs)** are **created, managed, and scheduled by the operating system kernel**.
- The **kernel is aware of every thread** individually.
- Each thread is treated as an independent **schedulable entity**.
- Modern operating systems (Linux, Windows, macOS) use **Kernel-Level Threads**.

---

## Scheduling

- **Kernel Scheduler** selects **individual threads**, not just processes.
- Every thread has its own scheduling information.
- Threads can have different priorities and states.

---

## Context Switching

During a thread context switch, the kernel saves and restores:

- Program Counter (PC)
- CPU Registers
- Stack Pointer (SP)
- Thread State

Since the kernel participates,

➡️ **Context switching is slower than User-Level Threads.**

---

## Blocking System Calls

If one thread performs a blocking system call:

```
Thread A → read()

↓

Only Thread A blocks

↓

Thread B ✔

Thread C ✔
```

Reason:

The kernel knows every thread separately.

---

## Multi-Core Execution

Kernel-Level Threads support **true parallelism**.

Example:

```
Core 1 → Thread A

Core 2 → Thread B

Core 3 → Thread C

Core 4 → Thread D
```

Each core can execute a different thread simultaneously.

---

## Privilege Level

Application code executes in:

- **User Mode (Ring 3)**

Kernel manages:

- Thread Creation
- Scheduling
- Context Switching
- Termination

in

- **Kernel Mode (Ring 0)**

---

## Performance

| Operation | Kernel-Level Thread |
|-----------|---------------------|
| Creation | Slower |
| Context Switch | Slower |
| Scheduling | Kernel |
| Blocking | Only calling thread blocks |
| Parallelism | Yes |

---

## Advantages

- Kernel awareness of every thread
- Independent scheduling
- True multi-core execution
- Better CPU utilization
- Better responsiveness
- Blocking affects only one thread

---

## Disadvantages

- Higher creation overhead
- Higher context-switch overhead
- More kernel memory usage
- Frequent user ↔ kernel mode switches

---

# Common GATE Traps ⚠️

❌ Kernel-Level Threads execute entirely in Kernel Mode.

✔ Application code executes in **User Mode**. Only thread management occurs in **Kernel Mode**.

---

❌ Blocking one thread blocks the entire process.

✔ Only the blocked thread stops. Other threads continue.

---

❌ Kernel-Level Threads cannot run on multiple cores.

✔ They support **true parallelism**.

---

❌ Thread scheduling is performed by a user-level library.

✔ Scheduling is performed by the **Kernel Scheduler**.

---

❌ Every process has only one Kernel-Level Thread.

✔ A process can have **multiple Kernel-Level Threads**.

---

# PYQ Focus 🎯

Questions are commonly asked on:

- ULT vs KLT comparison
- Kernel awareness
- Blocking system calls
- Context switching overhead
- Thread scheduling
- Multi-core execution
- User Mode vs Kernel Mode
- Advantages and disadvantages of Kernel-Level Threads
