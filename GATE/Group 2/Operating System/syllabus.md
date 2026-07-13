# Operating Systems (OS) Roadmap - GATE CSE + Placements

> **Goal:** Build a deep understanding of Operating Systems for **GATE**, **placements**, **Linux**, and **systems programming**.

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
              Inter Process Communication
                         │
          Concurrency & Synchronization
                         │
                     Deadlocks
                         │
                  CPU Scheduling
                         │
                  I/O Scheduling
                         │
                Memory Management
                         │
                  Virtual Memory
                         │
                    File Systems
```

---

# Module 0 — Foundations (Prerequisite)

> Learn **why an Operating System exists**.

## Topics

- What is an Operating System?
- Goals of an Operating System
- Services Provided by OS
- Kernel
- User Mode vs Kernel Mode
- Interrupts
- Exceptions
- Traps
- Boot Process (High Level)

---

# Module 1 — System Calls

## Goal

Understand **how user programs communicate with the kernel**.

## Topics

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

### Linux Examples

- fork()
- exec()
- wait()
- open()
- read()
- write()
- close()

---

# Module 2 — Processes

## Goal

Understand how the OS runs programs.

## Topics

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

## Goal

Learn lightweight execution within a process.

## Topics

- Thread
- Process vs Thread
- User Threads
- Kernel Threads
- Multithreading Models
- Thread Libraries
- Advantages of Threads

---

# Module 4 — Inter Process Communication (IPC)

## Goal

Understand how processes communicate.

## Topics

- Why IPC?
- Shared Memory
- Message Passing
- Pipes
- Named Pipes
- Sockets
- Signals

### Linux

- pipe()
- socket()
- shm

---

# Module 5 — Concurrency & Synchronization

## Goal

Prevent multiple processes from corrupting shared data.

## Topics

- Concurrency
- Race Condition
- Critical Section Problem
- Mutual Exclusion
- Peterson Algorithm
- Hardware Synchronization
- Test and Set
- Compare and Swap (CAS)
- Mutex
- Semaphore
- Monitor

### Classical Problems

- Producer Consumer
- Readers Writers
- Dining Philosophers
- Sleeping Barber (Optional)

---

# Module 6 — Deadlocks

## Goal

Understand how processes get stuck forever.

## Topics

- Deadlock
- Coffman Conditions
- Resource Allocation Graph
- Deadlock Prevention
- Deadlock Avoidance
- Banker's Algorithm
- Deadlock Detection
- Deadlock Recovery

---

# Module 7 — CPU Scheduling

## Goal

Learn how the CPU chooses the next process.

## Scheduling Criteria

- CPU Utilization
- Throughput
- Turnaround Time
- Waiting Time
- Response Time

## Algorithms

- FCFS
- SJF
- SRTF
- Priority Scheduling
- Round Robin
- Multilevel Queue
- Multilevel Feedback Queue

---

# Module 8 — I/O Scheduling

## Goal

Learn how disk requests are optimized.

## Topics

- Disk Structure
- Seek Time
- Rotational Latency
- Disk Scheduling

## Algorithms

- FCFS
- SSTF
- SCAN
- LOOK
- C-SCAN
- C-LOOK

---

# Module 9 — Memory Management

## Goal

Understand how RAM is managed.

## Topics

- Address Binding
- Logical Address
- Physical Address
- MMU
- Swapping
- Contiguous Allocation
- Internal Fragmentation
- External Fragmentation
- Paging
- Segmentation

---

# Module 10 — Virtual Memory

## Goal

Run programs larger than physical RAM.

## Topics

- Virtual Memory
- Demand Paging
- Page Fault
- Page Table
- TLB
- Multi-Level Paging
- Inverted Page Table
- Thrashing
- Working Set

## Page Replacement Algorithms

- FIFO
- LRU
- Optimal
- Second Chance (Clock)

---

# Module 11 — File Systems

## Goal

Learn how files are stored on disk.

## Topics

- File Concept
- File Attributes
- File Operations
- Directory Structure
- File Allocation Methods
    - Contiguous
    - Linked
    - Indexed
- Free Space Management
- FAT
- Inode
- Journaling (Basics)
- File Protection
- Access Methods

---

# 📅 Suggested Study Order

| Day | Module |
|------|--------|
| 1 | Foundations + System Calls |
| 2 | Processes |
| 3 | Threads |
| 4 | IPC |
| 5 | Synchronization |
| 6 | Deadlocks |
| 7 | CPU Scheduling |
| 8 | I/O Scheduling |
| 9 | Memory Management |
| 10 | Virtual Memory |
| 11 | File Systems |
| 12 | PYQs + Revision |

---

# ⭐ High Priority for GATE

| Priority | Topic |
|----------|-------|
| ⭐⭐⭐⭐⭐ | Processes |
| ⭐⭐⭐⭐⭐ | Synchronization |
| ⭐⭐⭐⭐⭐ | Deadlocks |
| ⭐⭐⭐⭐⭐ | CPU Scheduling |
| ⭐⭐⭐⭐⭐ | Memory Management |
| ⭐⭐⭐⭐⭐ | Virtual Memory |
| ⭐⭐⭐⭐☆ | IPC |
| ⭐⭐⭐⭐☆ | File Systems |
| ⭐⭐⭐⭐☆ | I/O Scheduling |
| ⭐⭐⭐☆☆ | Threads |
| ⭐⭐⭐☆☆ | System Calls |

---

# 📖 Study Pattern (Every Module)

For every topic:

1. ✅ Intuition (Why does this exist?)
2. ✅ Internal Working (CPU, Memory, Kernel)
3. ✅ Visual Mental Model
4. ✅ Step-by-Step Execution
5. ✅ Linux Commands & Examples
6. ✅ Common GATE Tricks
7. ✅ Placement Interview Questions
8. ✅ PYQs
9. ✅ Revision Notes

---

# 🎯 End Goal

By completing this roadmap, you should be able to:

- Solve **GATE PYQs** confidently.
- Answer **placement interview** questions.
- Understand Linux internals.
- Read systems programming code.
- Understand OS behavior during debugging.
- Build a strong foundation for Computer Architecture, DBMS, Networks, and Cybersecurity.