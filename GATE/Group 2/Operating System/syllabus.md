# Operating Systems (OS) Roadmap — GATE CSE 2027 + Placements + Linux

> **Goal:** Master Operating Systems from fundamentals to advanced concepts for **GATE**, **placements**, **Linux internals**, and **systems programming**.

---

# 📚 Official GATE Syllabus

- System Calls
- Processes
- Threads
- Inter-Process Communication (IPC)
- Concurrency & Synchronization
- Deadlock
- CPU Scheduling
- I/O Scheduling
- Memory Management
- Virtual Memory
- File Systems

---

# 🗺️ Learning Roadmap

```text
                 OPERATING SYSTEM
                        │
                Foundations (Basics)
                        │
                 System Calls
                        │
                  Processes
                        │
                   Threads
                        │
         CPU Scheduling
                        │
     Inter Process Communication
                        │
   Concurrency & Synchronization
                        │
                 Deadlocks
                        │
               Memory Management
                        │
               Virtual Memory
                        │
                File Systems
                        │
               I/O Scheduling
```

---

# Module 0 — Foundations

### Goal

Understand **why an Operating System exists** and how it controls hardware.

### Topics

- What is an Operating System?
- Goals of an OS
- Services Provided by OS
- Kernel
- User Mode vs Kernel Mode
- Privileged Instructions
- Interrupts
- Exceptions
- Traps
- Boot Process (High Level)
- Interrupt Handling (Basics)

---

# Module 1 — System Calls

### Goal

Understand how user programs communicate with the kernel.

### Topics

- What is a System Call?
- Why System Calls exist
- User Mode → Kernel Mode
- Types of System Calls
    - Process Control
    - File Management
    - Device Management
    - Information Maintenance
    - Communication
- API vs System Call
- POSIX
- Library Function vs System Call

### Linux

- fork()
- exec()
- wait()
- open()
- read()
- write()
- close()

---

# Module 2 — Processes

### Goal

Understand how the OS executes and manages programs.

### Topics

- Program vs Process
- Process States
- Process Life Cycle
- Process Control Block (PCB)
- Context Switching
- Dispatcher
- Process Creation
- Process Termination
- Parent & Child Process
- fork()
- exec()
- wait()
- Zombie Process
- Orphan Process

---

# Module 3 — Threads

### Goal

Understand lightweight execution inside a process.

### Topics

- Thread
- Process vs Thread
- User-Level Threads
- Kernel-Level Threads
- Multithreading Models
- Thread Libraries
- Thread Control Block (TCB)
- Thread Scheduling Basics
- Advantages & Disadvantages

---

# Module 4 — CPU Scheduling

## Goal

Understand how the Operating System selects the next runnable process or thread.

## 1. Scheduling Concepts

- CPU Burst
- I/O Burst
- CPU-I/O Burst Cycle
- Scheduler (Long-Term, Short-Term, Medium-Term)
- Preemptive vs Non-Preemptive
- Scheduling Queue
- Job Queue
- Ready Queue
- Device Queue
- Dispatcher
- Dispatcher Latency
- Context Switching

## 2. Scheduling Criteria

- CPU Utilization
- Throughput
- Turnaround Time (TAT)
- Waiting Time (WT)
- Response Time (RT)
- Fairness
- Starvation
- Aging

## 3. Scheduling Algorithms

- FCFS
- SJF
- SRTF
- Priority Scheduling
- Round Robin
- Multilevel Queue
- Multilevel Feedback Queue

## 4. Numerical Practice

- Gantt Chart
- Average Waiting Time
- Average Turnaround Time
- Average Response Time
- CPU Utilization Calculations
- Throughput Calculations
- Context Switch Overhead Problems

---

# Module 5 — Inter Process Communication (IPC)

## Goal

Understand how independent processes communicate, synchronize, and exchange data efficiently.

## Part 1 — IPC Foundations

- Why IPC?
- Need for IPC
- Process Isolation
- Cooperating vs Independent Processes
- Communication Models
- Synchronization Basics

## Part 2 — IPC Mechanisms

### Shared Memory

- Concept
- Advantages
- Disadvantages

### Message Passing

- Concept
- Direct vs Indirect Communication
- Blocking vs Non-Blocking Communication
- Buffering (Zero, Bounded, Unbounded)

## Part 3 — IPC Techniques

- Pipes (Anonymous Pipes)
- Named Pipes (FIFO)
- Message Queues
- Shared Memory Segments
- Sockets
- Signals

## Part 4 — Linux IPC System Calls

**Pipes**

- `pipe()`

**Named Pipes**

- `mkfifo()`

**Shared Memory**

- `shmget()`
- `shmat()`
- `shmdt()`
- `shmctl()`

**Message Queues**

- `msgget()`
- `msgsnd()`
- `msgrcv()`
- `msgctl()`

**Sockets**

- `socket()`
- `bind()`
- `listen()`
- `accept()`
- `connect()`
- `send()`
- `recv()`
- `close()`

**Signals**

- `kill()`
- `signal()`
- `sigaction()`
- `raise()`

## GATE Focus ⭐

- Why IPC is needed
- Shared Memory vs Message Passing
- Pipe vs Named Pipe
- Blocking vs Non-Blocking Communication
- Direct vs Indirect Communication
- Synchronization issues in Shared Memory
- Basic understanding of Linux IPC system calls (names and purpose)

---

# Module 6 — Concurrency & Synchronization

### Goal

Prevent race conditions while multiple execution units share resources.

### Topics

- Concurrency
- Parallelism
- Race Condition
- Critical Section
- Mutual Exclusion
- Progress
- Bounded Waiting

### Software Solutions

- Peterson Algorithm
- Bakery Algorithm (Optional)

### Hardware Solutions

- Test-and-Set
- Compare-and-Swap
- Swap Instruction

### Synchronization Tools

- Mutex
- Semaphore
    - Binary Semaphore vs Counting Semaphore
    - Implementation using wait() / signal()
- Monitor
- Condition Variables (Basics)
- Priority Inversion (Basics)

### Classical Problems

- Producer Consumer
- Readers Writers
- Dining Philosophers

---

# Module 7 — Deadlocks

### Goal

Understand why processes wait forever and how to handle it.

### Topics

- Deadlock
- System Resource Model
- Coffman Conditions
- Resource Allocation Graph
- Safe State
- Unsafe State
- Deadlock Prevention
- Deadlock Avoidance
- Banker's Algorithm
- Deadlock Detection
- Deadlock Recovery

---

# Module 8 — Memory Management

### Goal

Understand how RAM is allocated and protected.

### Topics

- Address Binding
- Logical vs Physical Address
- MMU
- Relocation
- Swapping
- Contiguous Allocation
    - First Fit
    - Best Fit
    - Worst Fit
    - Next Fit
- Internal Fragmentation
- External Fragmentation
- Paging
- Segmentation
- Segmentation with Paging (Concept)

### Numerical Practice

- Logical-to-Physical Address Translation
- Fragmentation Calculation (Internal / External)
- Allocation Strategy Comparison Problems (First/Best/Worst Fit)
- Page Number & Offset Calculation
- Segment Table Address Translation

---

# Module 9 — Virtual Memory

### Goal

Allow programs larger than physical RAM to execute efficiently.

### Topics

- Virtual Memory
- Demand Paging
- Page Fault Handling
- Page Table
- TLB
- Multi-Level Paging
- Inverted Page Table
- Working Set
- Thrashing
- Copy-on-Write

### Page Replacement

- FIFO
- Belady's Anomaly
- Optimal
- LRU
- Clock (Second Chance)

### Numerical Practice

- TLB Hit / Miss Ratio Problems
- Effective Memory Access Time (EMAT)
- Page Fault Rate Calculations
- Page Replacement Trace Problems (FIFO / LRU / Optimal)
- Multi-Level Page Table Size Calculations

---

# Module 10 — File Systems

### Goal

Understand how the OS organizes persistent storage.

### Topics

- File Concept
- File Attributes
- File Operations
- Access Methods
- Directory Structures
- File Allocation
    - Contiguous
    - Linked
    - Indexed
- Free Space Management
- FAT
- Inode
- Journaling (Basics)
- File Protection
- Hard Links vs Soft (Symbolic) Links
- Mounting / Unmounting

---

# Module 11 — I/O Scheduling

### Goal

Optimize disk head movement for faster I/O.

### Disk Basics

- Disk Structure
- Seek Time
- Rotational Latency
- Transfer Time
- Access Time

### Disk Scheduling Algorithms

- FCFS
- SSTF
- SCAN
- C-SCAN
- LOOK
- C-LOOK

---

# ⭐ GATE Priority

|Priority|Module|
|---|---|
|⭐⭐⭐⭐⭐|Processes|
|⭐⭐⭐⭐⭐|CPU Scheduling|
|⭐⭐⭐⭐⭐|Synchronization|
|⭐⭐⭐⭐⭐|Deadlocks|
|⭐⭐⭐⭐⭐|Memory Management|
|⭐⭐⭐⭐⭐|Virtual Memory|
|⭐⭐⭐⭐☆|File Systems|
|⭐⭐⭐⭐☆|IPC|
|⭐⭐⭐⭐☆|I/O Scheduling|
|⭐⭐⭐☆☆|Threads|
|⭐⭐⭐☆☆|System Calls|

---

# 📖 Study Pattern (Every Module)

For every topic, follow this sequence:

1. **Intuition** – Why does this concept exist?
2. **Internal Working** – CPU, memory, kernel, hardware interactions.
3. **Visual Mental Model** – Diagrams and execution flow.
4. **Step-by-Step Execution** – What happens internally?
5. **Linux Perspective** – Relevant commands, system calls, and examples.
6. **GATE Corner** – Frequently tested concepts and common traps.
7. **Placement Interview Questions** – Practical interview discussions.
8. **PYQs** – Solve previous GATE questions immediately after the topic.
9. **Revision Notes** – Condense into a one-page summary.

---

## Why this version?

Ordering follows the dependency graph inside an operating system:

- **CPU Scheduling** comes immediately after **Threads**, because scheduling is fundamentally about deciding **which process/thread runs next**.
- **IPC** comes before **Synchronization**, since synchronization mechanisms are used to coordinate communicating processes and threads.
- **I/O Scheduling** is moved to the end. Although GATE lists it separately, understanding disks, files, and storage first makes disk scheduling algorithms much more intuitive.