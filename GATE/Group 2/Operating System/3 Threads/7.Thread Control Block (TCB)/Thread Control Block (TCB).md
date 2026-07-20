# Thread Control Block (TCB)

> **Goal:** Understand why a Thread Control Block (TCB) is needed, what information it stores, how it is used during thread scheduling and context switching, and how it differs from a Process Control Block (PCB).

---

# Why Was Thread Control Block (TCB) Introduced?

Imagine a CPU executing a thread.

```
CPU

↓

Thread A
```

Suddenly,

- Time quantum expires
- Higher priority thread arrives
- Thread blocks for I/O

The CPU must stop executing Thread A.

Question:

> **How will the CPU remember where Thread A stopped?**

Without saving its execution state,

Thread A would restart from the beginning every time it gets the CPU.

This is impossible.

Therefore, the operating system stores the thread's execution context in a special data structure called the **Thread Control Block (TCB).**

---

# Definition

A **Thread Control Block (TCB)** is a kernel (or thread-library, for ULTs) data structure that stores all the information required to suspend and later resume a thread.

Think of it as the thread's **identity card + resume point**.

---

# Core Idea

```
Thread

↓

Running

↓

Interrupted

↓

Save State

↓

Thread Control Block (TCB)

↓

Later

↓

Restore State

↓

Continue Execution
```

The TCB allows a thread to continue exactly where it left off.

---

# Why Doesn't the CPU Store This Information?

The CPU has registers.

Example:

```
Program Counter

Registers

Stack Pointer
```

But registers can hold information for **only the currently running thread**.

When switching to another thread,

those values would be overwritten.

Therefore,

before switching,

the operating system copies those values into the TCB.

---

# What Does a TCB Store?

A typical Thread Control Block contains:

## 1. Thread ID (TID)

Unique identifier for the thread.

Example:

```
Thread ID = 101
```

Used by the operating system to identify the thread.

---

## 2. Thread State

Current execution state.

Examples:

- New
- Ready
- Running
- Waiting (Blocked)
- Terminated

---

## 3. Program Counter (PC)

Stores the address of the next instruction to execute.

Example:

```
Next instruction

↓

0x00401320
```

When resumed,

execution starts from this instruction.

---

## 4. CPU Registers

Stores register values.

Example:

```
RAX

RBX

RCX

...

```

Without saving registers,

all intermediate calculations would be lost.

---

## 5. Stack Pointer (SP)

Each thread has its own stack.

The Stack Pointer tells the CPU where the thread's stack currently is.

---

## 6. Scheduling Information

Used by the scheduler.

Includes:

- Priority
- Time Quantum
- Scheduling Class
- CPU Affinity (in some OSes)

---

## 7. Thread Status Information

Additional execution information such as:

- Waiting event
- Signals
- Flags

---

# Internal Working

Suppose:

```
CPU

↓

Thread A
```

Time quantum expires.

---

## Step 1

Operating System interrupts execution.

---

## Step 2

Kernel saves:

- Program Counter
- Registers
- Stack Pointer
- State

inside:

```
TCB of Thread A
```

---

## Step 3

Scheduler selects:

```
Thread B
```

---

## Step 4

Kernel loads Thread B's:

- Registers
- Program Counter
- Stack Pointer

from:

```
TCB of Thread B
```

---

## Step 5

CPU resumes Thread B exactly where it had stopped.

---

# Context Switching Using TCB

```
CPU

↓

Running Thread A

↓

Save Context

↓

TCB A

↓

Load Context

↓

TCB B

↓

CPU

↓

Running Thread B
```

Without the TCB,

context switching would not be possible.

---

# Relationship with Context Switching

**Context Switching = Save Current Thread + Load Next Thread**

The TCB is where the context is stored.

```
Save Context

↓

TCB

↓

Restore Context
```

---

# Relationship with PCB

A process contains resources like:

- Address Space
- Open Files
- Heap
- Global Variables

These are stored in the **Process Control Block (PCB).**

Each thread inside that process has its own:

- Program Counter
- Registers
- Stack
- Scheduling Info

These are stored in its **Thread Control Block (TCB).**

---

# PCB vs TCB

| Feature | PCB | TCB |
|----------|-----|-----|
| Represents | Process | Thread |
| Stores | Process resources | Thread execution context |
| Address Space | ✔ | ❌ (shared via PCB) |
| Open Files | ✔ | ❌ (shared) |
| Heap | ✔ | ❌ (shared) |
| Global Variables | ✔ | ❌ (shared) |
| Program Counter | ❌ | ✔ |
| CPU Registers | ❌ | ✔ |
| Stack Pointer | ❌ | ✔ |
| Scheduling Info | Process-level | Thread-level |

---

# Memory Relationship

```
Process

│
├── PCB
│
├── Shared Code
├── Shared Heap
├── Shared Files
├── Shared Globals
│
├── Thread A
│      │
│      ├── Stack
│      └── TCB
│
├── Thread B
│      │
│      ├── Stack
│      └── TCB
│
└── Thread C
       │
       ├── Stack
       └── TCB
```

---

# Real-World Analogy

Think of a **movie paused on Netflix**.

The movie itself is the **process**.

The exact playback position (1 hour 23 minutes), language, subtitles, etc., represent the thread's current execution state.

When you resume,

Netflix remembers exactly where you stopped.

The **saved playback state** is like the **TCB**.

---

# Advantages

- Enables context switching.
- Allows independent thread scheduling.
- Preserves execution state.
- Makes multitasking possible.

---

# GATE Corner ⭐

## Must Remember

- **TCB stores a thread's execution context.**
- One thread has one TCB.
- Context switching saves to one TCB and restores from another.
- Every thread has its own:
  - Program Counter
  - Registers
  - Stack Pointer
  - Scheduling Information
- Threads of the same process share the PCB but have separate TCBs.

---

## Common GATE Traps ⚠️

❌ TCB stores process resources.

✔ False.

Process resources belong to the PCB.

---

❌ Threads in the same process share a TCB.

✔ False.

Each thread has its own TCB.

---

❌ Context switching only changes the Program Counter.

✔ False.

It restores the entire execution context:
- PC
- Registers
- Stack Pointer
- State

---

❌ TCB stores the process address space.

✔ False.

The address space belongs to the PCB and is shared by all threads in the process.

---

# PYQ Focus 🎯

- TCB vs PCB
- Context Switching
- Thread Scheduling
- Thread State
- Program Counter
- CPU Registers
- Stack Pointer
- Execution Context