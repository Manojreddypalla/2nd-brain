# User-Level Threads (ULT)

> **Goal:** Understand why User-Level Threads were invented, how they work internally, and their limitations.

---

# Why Were User-Level Threads Created?

Processes are expensive.

Creating or switching a process requires the kernel to:

- Allocate a PCB
- Manage page tables
- Switch address spaces
- Perform system calls

This introduces significant overhead.

Threads solve part of this problem, but if every thread operation still requires entering the kernel, thread management remains expensive.

To reduce this overhead, **User-Level Threads (ULTs)** were introduced.

The idea is simple:

> Let the application manage its own threads instead of asking the operating system every time.

---

# Core Idea

A **User-Level Thread** exists entirely in **user space**.

The operating system **does not know** that multiple threads exist.

To the kernel:

```
Application = One Process
```

Internally, however, the application may have many threads managed by a thread library.

---

# Permission Level

User-Level Threads execute in **User Mode (Ring 3)**.

They **cannot**:

- Access hardware directly
- Modify page tables
- Schedule CPU execution
- Access kernel memory
- Perform privileged instructions

Whenever they need these operations, they must invoke a **system call**, causing a switch to **Kernel Mode (Ring 0)**.

---

# Internal Working

```
CPU
 │
 ▼
Kernel Scheduler
 │
 ▼
One Process
 │
 ▼
User-Level Thread Library
 ├── Thread A
 ├── Thread B
 └── Thread C
```

The kernel schedules **only the process**.

Inside that process, the thread library decides which thread executes.

So there are two schedulers:

- Kernel Scheduler → chooses the process.
- Thread Library → chooses the thread.

---

# Why Is Context Switching Fast?

Since all threads belong to the same process:

- Address space remains the same.
- No page-table switch.
- No kernel mode switch.

The thread library simply changes:

- Program Counter
- Registers
- Stack Pointer

This makes switching extremely fast.

---

# The Biggest Limitation

Suppose Thread A performs:

```c
read(fd, buffer, size);
```

The kernel only knows about the **process**, not its individual threads.

Therefore:

```
Thread A blocks
      ↓
Kernel blocks the Process
      ↓
Thread B also stops
Thread C also stops
```

This is the **blocking problem**, the biggest drawback of User-Level Threads.

---

# Multi-Core CPUs

Suppose the process has:

- Thread A
- Thread B
- Thread C

The kernel sees only **one schedulable entity** (the process).

Therefore, even on a 4-core CPU:

```
Core 1 ✔

Core 2 ✘

Core 3 ✘

Core 4 ✘
```

Only one core can execute the process at a time.

So User-Level Threads **cannot achieve true parallelism**.

...