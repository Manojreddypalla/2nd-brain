# Thread

> **Goal:** Understand what a thread is and why it exists.

---

# What is a Thread?

A **thread** is the **smallest unit of CPU execution (or scheduling)** inside a process.
A thread is the smallest entity that can be scheduled by the operating system.



A process can contain **one or more threads**.

> **Key Idea:** A process owns resources, while a thread executes instructions.

---

# Why Do Threads Exist?

Creating a new process is expensive because the OS must create:

- New Address Space
- New PCB
- New Page Tables
- New Kernel Resources

Instead, multiple threads can execute within the **same process**, sharing most resources.

This makes execution faster and more efficient.

---

# Process vs Thread (Basic Idea)

## Process

- Resource container
- Owns memory and OS resources

## Thread

- Smallest unit of execution inside a process.
- Represents a single flow (sequence) of instructions executed by the CPU.
- Each thread has its own:
  - Program Counter (PC)
  - Registers
  - Stack
- Threads of the same process share:
  - Code
  - Data
  - Heap
  - Open files

---

# Resources Shared by Threads

Threads within the same process share:

- Code (Text Segment)
- Heap
- Global Variables
- Address Space
- Open Files

---

# Resources Private to Each Thread

Each thread has its own:

- Program Counter (PC)
- Registers
- Stack
- Thread State

---

# Internal Working

```text
CPU
 ↓
Scheduler
 ↓
Thread
 ↓
Instructions Execute
```

The CPU executes **threads**, not processes directly.

---

# Why Threads are Lightweight

Since threads share the same process resources:

- No separate address space
- No separate heap
- Faster creation
- Faster context switching
- Faster communication

---

# Real-World Example

## Google Chrome

One process may contain multiple threads:

- UI Thread
- Rendering Thread
- JavaScript Thread
- Network Thread

Each performs a different task while sharing the same process resources.

---

# Advantages

- Faster execution
- Better responsiveness
- Efficient resource sharing
- Lower creation overhead
- Easy communication between threads

---

# Disadvantages

- Synchronization problems
- Race conditions
- Harder debugging
- One faulty thread can affect the entire process

---

# GATE Points ⭐

- Thread = Smallest unit of CPU execution.
- A process can have multiple threads.
- Threads share the process's address space.
- Each thread has its own stack and registers.
- Context switching between threads is cheaper than between processes.

---

# Common GATE Traps ⚠️

❌ CPU executes processes directly.

✔ CPU executes **threads**.

---

❌ Every thread has its own heap.

✔ Heap is shared by all threads in a process.

---

❌ Threads have separate address spaces.

✔ Threads share the same address space.

---

# Quick Revision

- Thread = Execution unit
- Process = Resource owner
- Shared → Code, Heap, Globals, Address Space, Open Files
- Private → PC, Registers, Stack, Thread State
- Thread context switch < Process context switch (less overhead)