# Advantages & Disadvantages of Threads

> **Goal:** Understand why threads are preferred over processes, what benefits they provide, and the challenges they introduce.

---

# Why Were Threads Introduced?

Creating a new **process** is expensive because every process has its own:

- Address Space
- Page Table
- Open Files
- Heap
- Process Control Block (PCB)

When switching between processes, the operating system must switch all these resources, making process creation and context switching costly.

Threads solve this problem.

Threads belonging to the same process **share most resources**, while maintaining their own execution context.

This makes them lightweight.

---

# Advantages of Threads

## 1. Faster Thread Creation

Creating a thread is much faster than creating a process because:

- No new address space is created.
- No new page tables are required.
- Existing process resources are reused.

```
Process

↓

Shared Resources

↓

New Thread
```

Result:

✅ Lower creation overhead.

---

## 2. Faster Context Switching

During thread context switching:

The operating system changes only:

- Program Counter (PC)
- CPU Registers
- Stack Pointer

Shared resources remain unchanged.

```
Thread A

↓

Save Context

↓

Load Thread B Context

↓

Continue Execution
```

Result:

✅ Faster than process switching.

---

## 3. Shared Memory

Threads share:

- Code Segment
- Heap
- Global Variables
- Open Files

Example:

```
Thread A

↓

Updates Balance

↓

Thread B immediately sees the updated value.
```

No Inter-Process Communication (IPC) is required.

Result:

✅ Fast communication.

---

## 4. Better Responsiveness

If one thread is waiting:

```
Download Thread

↓

Waiting for Internet
```

Other threads continue:

```
UI Thread ✔

Render Thread ✔

Audio Thread ✔
```

Example:

A browser remains responsive while downloading files.

---

## 5. Better CPU Utilization

On multi-core processors:

```
Core 1 → Thread A

Core 2 → Thread B

Core 3 → Thread C

Core 4 → Thread D
```

Multiple threads execute simultaneously.

Result:

✅ Higher throughput.

---

## 6. Resource Sharing

All threads inside a process share:

- Address Space
- Heap
- Global Variables
- Files

This reduces memory consumption compared to creating multiple processes.

---

## 7. Improved Concurrency

Different tasks can execute independently.

Example:

```
Browser

↓

Render Page

↓

Download Images

↓

Execute JavaScript

↓

Play Audio
```

Each task runs in a separate thread.

---

# Disadvantages of Threads

## 1. Race Condition

Since threads share memory,

two threads may modify the same data simultaneously.

Example:

```
Balance = 1000

↓

Thread A Withdraws

↓

Thread B Withdraws
```

Result:

Incorrect final value.

Solution:

Mutex, Semaphore, Locks.

---

## 2. Synchronization Complexity

Shared memory requires synchronization.

Programmers must use:

- Mutex
- Semaphore
- Condition Variables

Improper synchronization causes bugs.

---

## 3. Deadlocks

Improper locking order can cause threads to wait forever.

Example:

```
Thread A

Lock Resource 1

Waiting for Resource 2

↓

Thread B

Lock Resource 2

Waiting for Resource 1
```

Neither thread proceeds.

---

## 4. Difficult Debugging

Threads execute concurrently.

The order of execution changes every run.

A bug may appear:

Today ✔

Tomorrow ❌

Making debugging difficult.

---

## 5. Context Switching Overhead

Although faster than process switching,

context switching still consumes CPU time.

Too many threads can reduce performance.

---

## 6. Thread Failure Can Affect Entire Process

Threads share the same address space.

If one thread corrupts shared memory,

other threads may also fail.

Sometimes the entire process crashes.

---

## 7. Increased Programming Complexity

Developers must think about:

- Shared Resources
- Synchronization
- Locks
- Deadlocks
- Race Conditions

Writing correct multithreaded programs is significantly harder than writing single-threaded programs.

---

# Real-World Example

Google Chrome

Advantages:

- UI remains responsive.
- Downloads continue.
- JavaScript executes.
- Rendering continues.

Disadvantages:

- Shared data requires synchronization.
- Bugs are harder to reproduce.
- Thread synchronization increases complexity.

---

# GATE Corner ⭐

## Must Remember

### Advantages

- Faster thread creation
- Faster context switching
- Shared memory
- Better responsiveness
- Better CPU utilization
- Lightweight
- Improved concurrency

---

### Disadvantages

- Race Conditions
- Synchronization complexity
- Deadlocks
- Difficult debugging
- Context switching overhead
- Shared memory corruption
- Programming complexity

---

# Common GATE Traps ⚠️

❌ Threads have separate address spaces.

✔ False.

Threads share the same process address space.

---

❌ Thread communication requires IPC.

✔ False.

Threads communicate through shared memory.

---

❌ Thread context switching is slower than process switching.

✔ False.

Thread context switching is generally faster because the address space is shared.

---

❌ Threads eliminate synchronization problems.

✔ False.

Threads actually make synchronization necessary because they share resources.

---

❌ More threads always improve performance.

✔ False.

Too many threads increase scheduling and context-switch overhead (oversubscription).

---

# PYQ Focus 🎯

- Advantages over Processes
- Shared Memory
- Race Condition
- Synchronization
- Deadlocks
- Context Switching
- Thread Communication
- Multi-Core Execution
-