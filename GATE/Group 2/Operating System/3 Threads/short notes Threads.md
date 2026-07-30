# Module 3 — Threads (GATE Short Notes)

> **Exam Goal:** Understand **why threads exist, how they are implemented, and their models.**
> 
> **Memory Trick:**  
> **Process = Resource Container**  
> **Thread = Execution Unit**

---

# 1. Why Threads?

### Problem

A process can execute only **one instruction stream** at a time.

Suppose Chrome has only one execution path.

- Download webpage
    
- Render HTML
    
- Play video
    
- Listen to mouse click
    

Everything happens one after another.

➡️ Slow.

---

### Solution

Split work into multiple execution paths.

Each execution path is a **Thread**.

```
Chrome Process

 ├── UI Thread
 ├── Rendering Thread
 ├── Network Thread
 ├── Audio Thread
 └── GPU Thread
```

All run concurrently.

---

## GATE Point

> Threads improve **responsiveness** and **parallelism**.

---

# 2. Need for Multithreading

Threads are used because they provide:

### 1. Responsiveness

Application doesn't freeze.

Example:

Downloading file while scrolling.

---

### 2. Resource Sharing

Threads share

- Code
    
- Heap
    
- Files
    
- Global Variables
    

No need to duplicate memory.

---

### 3. Economy

Creating a thread is cheaper than creating a process.

Reason:

No new address space.

---

### 4. Scalability

On multicore CPU

```
Core 1 → Thread A

Core 2 → Thread B

Core 3 → Thread C
```

Multiple threads truly execute simultaneously.

---

### GATE Keyword

> **Cheaper Context Switch**

---

# 3. Thread

## Definition

A thread is the **smallest unit of CPU execution.**

Also called

> Lightweight Process (LWP)

---

Each thread has its own

- Program Counter (PC)
    
- Registers
    
- Stack
    

Shares

- Code
    
- Heap
    
- Data
    
- Open Files
    

---

## Diagram

```
Process

+---------------------+
| Code                |
| Heap                |
| Global Data         |
+---------------------+

      Shared

 -------------------------
 | Thread 1             |
 | Stack                |
 | Registers            |
 -------------------------

 -------------------------
 | Thread 2             |
 | Stack                |
 | Registers            |
 -------------------------
```

---

# 4. Process vs Thread

|Process|Thread|
|---|---|
|Heavyweight|Lightweight|
|Own address space|Shared address space|
|Expensive creation|Cheap creation|
|IPC required|Easy communication|
|Slow context switch|Fast context switch|
|Strong isolation|Weak isolation|

---

## GATE Shortcut

**Process = Separate House**

**Thread = Family Members inside same house**

---

# 5. User-Level Threads (ULT)

Managed completely by

> User Thread Library

Kernel doesn't know threads exist.

Kernel sees

```
One Process
```

only.

---

### Advantages

✔ Very fast

✔ No kernel mode switch

✔ Portable

---

### Disadvantages

If one thread blocks,

Entire process blocks.

Cannot utilize multiple CPUs effectively.

---

Diagram

```
Application

Thread A
Thread B
Thread C

↓

Thread Library

↓

Kernel

(Process Only)
```

---

## GATE Fact

Kernel schedules

**Process**

NOT threads.

---

# 6. Kernel-Level Threads (KLT)

Kernel manages every thread.

Kernel knows

```
Thread 1
Thread 2
Thread 3
```

individually.

---

Advantages

✔ True Parallelism

✔ Better Scheduling

✔ Blocking thread doesn't stop others

---

Disadvantages

Context switching is expensive.

Need kernel mode.

---

Diagram

```
Application

T1

↓

Kernel Thread

↓

CPU

T2

↓

Kernel Thread

↓

CPU
```

---

# ULT vs KLT

|ULT|KLT|
|---|---|
|User managed|Kernel managed|
|Fast|Slower|
|No kernel support|Kernel support required|
|No parallel execution|Parallel execution possible|
|Entire process blocks|Only blocked thread waits|

---

# 7. Multithreading Models

These describe how **User Threads map to Kernel Threads**.

---

## (a) Many-to-One

```
User Threads

T1
T2
T3

↓

One Kernel Thread
```

Characteristics

- Very fast
    
- No parallelism
    
- One blocking call blocks all
    

---

## (b) One-to-One

```
T1 → KT1

T2 → KT2

T3 → KT3
```

Every user thread gets a kernel thread.

Advantages

✔ True parallelism

Disadvantages

Large number of kernel threads.

Higher overhead.

---

## (c) Many-to-Many

```
User Threads

T1
T2
T3
T4
T5

↓

KT1
KT2
KT3
```

Many user threads share fewer kernel threads.

Best balance.

---

## (d) Two-Level Model

Like Many-to-Many

But

Some user threads can be permanently bound.

```
T1 → KT1

(Bound)

Others

↓

Shared Kernel Threads
```

Provides flexibility.

---

# Comparison

|Model|Parallelism|Blocking|
|---|---|---|
|Many-to-One|❌|Entire process|
|One-to-One|✔|Single thread|
|Many-to-Many|✔|Single thread|
|Two-Level|✔|Single thread|

---

# 8. Thread Libraries

A library providing APIs for thread creation and management.

Functions

- Create Thread
    
- Exit Thread
    
- Join Thread
    
- Synchronization
    

---

Examples

- POSIX Threads (Pthreads)
    
- Windows Threads
    
- Java Threads
    

---

## GATE Fact

Thread libraries may implement

- User threads
    
- Kernel threads
    
- Hybrid
    

---

# 9. Thread Control Block (TCB)

Equivalent of PCB for threads.

Stores thread information.

---

Contains

- Thread ID
    
- Thread State
    
- Program Counter
    
- Stack Pointer
    
- Registers
    
- Scheduling Information
    
- Priority
    

---

Diagram

```
TCB

Thread ID

State

Registers

Stack Pointer

Program Counter

Priority
```

---

## PCB vs TCB

PCB stores

Entire process information.

TCB stores

Only thread information.

---

# 10. Thread Scheduling Basics

Scheduler selects

Runnable threads.

---

Possible States

```
New

↓

Ready

↓

Running

↓

Blocked

↓

Ready

↓

Terminated
```

---

Scheduling may occur when

- Time slice expires
    
- Thread blocks
    
- Higher priority arrives
    

---

Kernel schedules

Kernel Threads

NOT user threads.

(Except pure ULT implementation.)

---

# 11. Advantages of Threads

✔ Faster context switching

✔ Better CPU utilization

✔ Higher responsiveness

✔ Easy communication

✔ Less memory

✔ Resource sharing

✔ Supports multicore execution

✔ Lower creation cost

---

# 12. Disadvantages of Threads

✖ Synchronization complexity

✖ Race Conditions

✖ Deadlocks

✖ Debugging difficult

✖ One faulty thread may crash whole process

✖ Shared memory increases bugs

---

# ⭐ GATE One-Page Revision

```
Thread
→ Smallest CPU execution unit

Own
→ Stack
→ Registers
→ PC

Share
→ Code
→ Heap
→ Data
→ Files

------------------------------

ULT
✓ Fast
✓ User Managed
✗ No Parallelism
✗ Blocking Problem

------------------------------

KLT
✓ Parallelism
✓ Better Scheduling
✗ Expensive

------------------------------

Models

Many → One
No Parallelism

One → One
Parallelism
High Overhead

Many → Many
Balanced

Two-Level
Many-to-Many + Binding

------------------------------

Thread Library

Pthreads
Windows
Java

------------------------------

TCB

Thread ID
State
PC
Registers
Stack Pointer
Priority

------------------------------

Advantages

Fast
Cheap
Responsive
Shared Memory
Multicore

------------------------------

Disadvantages

Race Condition
Deadlock
Synchronization
Debugging Hard
```

---

# 🎯 Must-Remember GATE Facts

1. **Thread = Smallest schedulable execution unit.**
    
2. **Process owns resources; thread executes instructions.**
    
3. **Threads share Code, Data, Heap, and Files, but each has its own Stack, Registers, and Program Counter.**
    
4. **Context switching between threads is faster than between processes.**
    
5. **ULT is managed in user space; the kernel is unaware of individual threads.**
    
6. **KLT is managed by the kernel and supports true multicore parallelism.**
    
7. **Many-to-One does not support parallel execution.**
    
8. **One-to-One provides maximum parallelism but has higher overhead.**
    
9. **Many-to-Many combines efficiency with parallelism.**
    
10. **TCB is to a thread what PCB is to a process.**