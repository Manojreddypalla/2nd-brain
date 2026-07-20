# Kernel-Level Threads (KLT)

> **Goal:** Understand why Kernel-Level Threads were introduced, how they work internally, and why modern operating systems use them.

---

# Why Were Kernel-Level Threads Introduced?

User-Level Threads (ULTs) solved the problem of expensive process creation and switching.

However, they introduced two major limitations:

## Problem 1 — Blocking System Calls

Consider a process containing three threads:

```
Process P

├── Thread A
├── Thread B
└── Thread C
```

Thread A executes:

```c
read(fd, buffer, size);
```

The **kernel does not know Thread A exists**.

It only knows:

```
Process P
```

Therefore, when Thread A blocks,

```
Thread A ❌
Thread B ❌
Thread C ❌
```

The **entire process becomes blocked**.

---

## Problem 2 — No True Parallelism

Suppose:

- CPU = 8 Cores
- Process = 10 User-Level Threads

The kernel sees only:

```
Process P
```

It schedules only one execution context.

Therefore,

```
Core 1 → Running

Core 2 → Idle

Core 3 → Idle

Core 4 → Idle
```

The application cannot utilize multiple CPU cores simultaneously.

---

# Solution

To overcome these problems, thread management was moved into the **Operating System Kernel**.

Instead of managing threads in user space,

the kernel now creates, schedules and manages every thread.

This is called a **Kernel-Level Thread (KLT).**

---

# Definition

A **Kernel-Level Thread (KLT)** is a thread that is:

- Created by the kernel
- Managed by the kernel
- Scheduled by the kernel

The operating system is aware of every thread individually.

---

# Core Idea

```
Application

↓

Thread A

Thread B

Thread C

↓

Kernel

↓

CPU Scheduler

↓

CPU
```

Unlike User-Level Threads,

the kernel directly schedules every thread.

---

# Permission Levels

Kernel-Level Threads **do NOT execute entirely in Kernel Mode.**

This is a common misconception.

## Application Code

Runs in

```
User Mode (Ring 3)
```

## Kernel Responsibilities

Runs in

```
Kernel Mode (Ring 0)
```

Kernel manages:

- Thread Creation
- Thread Scheduling
- Context Switching
- Blocking
- Wake-up
- Thread Termination

Whenever a privileged operation is needed,

```
User Mode

↓

System Call

↓

Kernel Mode

↓

Kernel performs operation

↓

Return to User Mode
```

---

# Internal Working

## Step 1

Application requests a new thread.

Example:

```c
pthread_create();
```

---

## Step 2

System call enters the kernel.

---

## Step 3

Kernel creates:

- Thread Control Block (TCB)
- Kernel Stack
- Thread ID (TID)
- Scheduling Information

---

## Step 4

Thread enters the Ready Queue.

---

## Step 5

Kernel Scheduler selects the thread.

---

## Step 6

Dispatcher performs Context Switch.

---

## Step 7

CPU begins executing the thread.

---

# Context Switching

Suppose:

```
CPU

↓

Thread A
```

Time quantum expires.

Kernel saves Thread A:

- Registers
- Program Counter
- Stack Pointer

inside the Thread Control Block.

Kernel loads Thread B's context.

CPU resumes:

```
Thread B
```

Since the kernel participates,

context switching is **slower than User-Level Threads**.

---

# Blocking System Calls

Suppose Thread A executes:

```c
read();
```

Kernel blocks only:

```
Thread A
```

Other threads remain runnable.

```
Thread A ❌

Thread B ✔

Thread C ✔
```

This is the biggest advantage of Kernel-Level Threads.

---

# Multi-Core Execution

Suppose:

```
Thread A

Thread B

Thread C

Thread D
```

Kernel knows every thread.

It can schedule:

```
Core 1 → Thread A

Core 2 → Thread B

Core 3 → Thread C

Core 4 → Thread D
```

This enables **true parallelism**.

---

# Why Kernel-Level Threads are Slower

Every major thread operation requires kernel intervention.

Examples:

- Thread Creation
- Thread Termination
- Context Switching
- Scheduling

Each operation performs:

```
User Mode

↓

Kernel Mode

↓

Kernel Work

↓

User Mode
```

These mode switches increase overhead.

---

# Advantages

- Kernel knows every thread.
- True parallel execution on multiple CPU cores.
- Better CPU utilization.
- Blocking affects only one thread.
- Better responsiveness.
- Independent scheduling.

---

# Disadvantages

- Slower thread creation.
- Slower context switching.
- More kernel overhead.
- More memory required (kernel stack + kernel data structures).
- More complex implementation.

---

# Real-World Example

Google Chrome

```
Chrome Process

├── UI Thread
├── Render Thread
├── Network Thread
└── JavaScript Thread
```

If:

```
Network Thread

↓

Waiting for Internet
```

Kernel blocks only that thread.

Meanwhile,

```
UI Thread ✔

Render Thread ✔

JavaScript Thread ✔
```

Browser continues working smoothly.

---

# GATE Corner ⭐

## Must Remember

- Kernel manages every thread.
- Kernel Scheduler schedules individual threads.
- Kernel knows every thread.
- Thread operations require kernel intervention.
- Blocking one thread does NOT block the entire process.
- True parallelism is possible.
- Application code still executes in User Mode.

---

## Common GATE Traps ⚠️

❌ Kernel-Level Threads execute entirely in Kernel Mode.

✔ Application code executes in User Mode.

Kernel Mode is entered only when kernel services are required.

---

❌ Blocking one thread blocks the entire process.

✔ Only the calling thread blocks.

---

❌ Kernel-Level Threads cannot utilize multiple CPU cores.

✔ They support true parallel execution.

---

❌ Thread scheduling is done by a thread library.

✔ Thread scheduling is performed by the Kernel Scheduler.

---

# Quick Revision

```
Kernel-Level Threads

Managed By      → Kernel

Kernel Aware    → Yes

Scheduling      → Kernel Scheduler

Context Switch  → Slower

Blocking Call   → Blocks only one thread

Parallelism     → Yes

CPU Cores       → Multiple

Mode            → User Mode (execution)
                  Kernel Mode (management)
```

---

# PYQ Focus 🎯

- ULT vs KLT
- Blocking System Calls
- Thread Scheduling
- Context Switching
- Multi-Core Execution
- User Mode vs Kernel Mode
- Advantages & Disadvantages
- Kernel Awareness