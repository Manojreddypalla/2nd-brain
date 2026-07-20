# Process vs Thread

> **Goal:** Understand why threads were introduced and how they differ from processes.

---

# Why Were Threads Introduced?

Creating a new **process** is expensive because the OS must create:

- New Address Space
- New PCB
- New Page Tables
- New Kernel Resources

Instead of creating multiple processes for related tasks, the OS allows multiple **threads** inside the same process.

This reduces overhead and improves performance.

---

# Basic Idea

## Process

A **process** is a **resource owner**.

It owns:

- Memory
- Address Space
- Open Files
- OS Resources

---

## Thread

A **thread** is the **smallest unit of CPU execution**.

It executes instructions inside a process.

A process can have one or many threads.

---

# Visual Representation

```text
                 Process

+--------------------------------------+
| Code                                 |
| Heap                                 |
| Global Variables                     |
| Open Files                           |
|                                      |
|  Thread 1                            |
|  Thread 2                            |
|  Thread 3                            |
+--------------------------------------+
```

---

# Memory Organization

## Multiple Processes

```text
Process A
---------
Code
Heap
Stack

------------------

Process B
---------
Code
Heap
Stack
```

Each process has its **own address space**.

---

## Multiple Threads

```text
Process
-------------------------
Code
Heap
Global Variables

-------------------------

Thread 1 → Stack

Thread 2 → Stack

Thread 3 → Stack
```

All threads share the same process memory.

Only their stacks are separate.

---

# Process vs Thread

| Process | Thread |
|----------|---------|
| Heavyweight | Lightweight |
| Resource owner | Execution unit |
| Own address space | Shared address space |
| Own PCB | Own TCB (or thread-specific information) |
| Communication via IPC | Communication through shared memory |
| Creation is slow | Creation is fast |
| Context switching is expensive | Context switching is cheaper |
| Better isolation | Less isolation |

---

# Resource Sharing

## Shared Among Threads

- Code Segment
- Heap
- Global Variables
- Address Space
- Open Files

---

## Private to Each Thread

- Program Counter (PC)
- Registers
- Stack
- Thread State

---

# Context Switching

## Process Context Switch

The OS switches:

- PCB
- Registers
- Program Counter
- Stack
- Address Space
- Page Tables

➡️ Higher overhead

---

## Thread Context Switch

The OS switches:

- Registers
- Program Counter
- Stack Pointer
- Thread State

Memory remains shared.

➡️ Lower overhead

---

# Communication

## Between Processes

Requires **Inter-Process Communication (IPC)**:

- Pipes
- Shared Memory
- Message Queues
- Sockets

---

## Between Threads

Threads communicate through **shared memory** because they already share the same address space.

---

# Real-World Example

## Google Chrome

One Chrome process may contain:

- UI Thread
- Rendering Thread
- Network Thread
- JavaScript Thread

Each thread performs a different task while sharing the browser's resources.

---

# Advantages of Threads over Processes

- Faster creation
- Faster context switching
- Better responsiveness
- Efficient resource sharing
- Easy communication

---

# Disadvantages

- Race conditions
- Synchronization problems
- Harder debugging
- Failure of one thread may affect the entire process

---

# GATE Points ⭐

- Process = Resource owner
- Thread = Smallest unit of CPU execution
- Threads share the same address space
- Each thread has its own stack and registers
- Thread context switch is cheaper than process context switch
- Threads communicate through shared memory
- Processes require IPC for communication

---

# Common GATE Traps ⚠️

❌ Threads have separate address spaces.

✔ Threads share the process's address space.

---

❌ Every thread has its own heap.

✔ Heap belongs to the process and is shared.

---

❌ CPU executes processes directly.

✔ CPU executes **threads**.

---

❌ Processes communicate directly.

✔ Processes require **IPC**.

---

# Quick Revision

```text
Process
--------
• Heavyweight
• Resource owner
• Separate address space
• PCB
• IPC required
• Slow creation
• Expensive context switch

Thread
-------
• Lightweight
• Execution unit
• Shared address space
• TCB
• Shared memory communication
• Fast creation
• Cheap context switch
```