---
tags: [OS, GATE, PCB, Processes]
aliases: [Process Control Block, PCB]
---

# 🧠 Process Control Block (PCB) — Master Connection Map

> **Purpose:** This note is NOT for memorizing the PCB.
> It is a roadmap showing how every field of the PCB connects to the Operating Systems syllabus.

---

# PCB Definition

A **Process Control Block (PCB)** is the kernel's data structure that stores all information required to manage, pause, resume, schedule, and terminate a process.

> Think of it as the **identity card + resume + checkpoint** of a process.

---

# Learning Status

| Topic              | Module                | Status |
| ------------------ | --------------------- | ------ |
| Program vs Process | ✅ Processes           | ⬜      |
| Process States     | ✅ Processes           | ⬜      |
| Process Life Cycle | ✅ Processes           | ⬜      |
| PCB Overview       | ✅ Processes           | ⬜      |
| Program Counter    | ✅ Processes           | ⬜      |
| CPU Registers      | ✅ Processes           | ⬜      |
| Context Switching  | ✅ Processes           | ⬜      |
| CPU Scheduling     | 🔜 Scheduling         | ⬜      |
| Threads            | 🔜 Threads            | ⬜      |
| IPC                | 🔜 IPC                | ⬜      |
| Synchronization    | 🔜 Synchronization    | ⬜      |
| Deadlocks          | 🔜 Deadlocks          | ⬜      |
| Memory Management  | 🔜 Memory Management  | ⬜      |
| Virtual Memory     | 🔜 Virtual Memory     | ⬜      |
| File Systems       | 🔜 File Systems       | ⬜      |
| Signals            | 🔜 Process Management | ⬜      |

---

# PCB Fields

## 1. Process Identification

Contains

- PID
- PPID
- User ID
- Group ID

Connected Topic

→ Program vs Process

Status

⬜

---

## 2. Process State

Examples

- New
- Ready
- Running
- Waiting
- Terminated

Connected Topic

→ Process States

Status

⬜

---

## 3. Program Counter

Stores

- Address of next instruction

Connected Topics

- CPU Registers
- Context Switching

Status

⬜

---

## 4. CPU Registers

Contains

- General Registers
- Stack Pointer
- Program Counter
- Flag Register

Connected Topics

- CPU Architecture
- Context Switching

Status

⬜

---

## 5. CPU Scheduling Information

Contains

- Priority
- Time Quantum
- CPU Burst
- Queue Pointers

Connected Module

→ CPU Scheduling

Topics

- FCFS
- SJF
- SRTF
- Priority
- Round Robin
- Multilevel Queue
- Multilevel Feedback Queue

Status

⬜

---

## 6. Memory Management Information

Contains

- Page Table Pointer
- Segment Table
- Virtual Address Space
- Memory Descriptor

Connected Module

→ Memory Management

Later Topics

- Paging
- Segmentation
- Address Translation
- TLB
- Virtual Memory

Status

⬜

---

## 7. I/O Status Information

Contains

- Open File Table
- Devices
- Pending I/O
- File Descriptors

Connected Module

→ File Systems
→ I/O Management

Status

⬜

---

## 8. Accounting Information

Contains

- CPU Time
- Process Creation Time
- Resource Usage

Connected Module

→ Scheduling
→ OS Accounting

Status

⬜

---

## 9. Process Resources

Contains

- Locks
- Semaphores
- Message Queues
- Shared Memory

Connected Modules

→ IPC
→ Synchronization

Status

⬜

---

## 10. Signal Information

Contains

- Pending Signals
- Signal Handlers

Connected Module

→ Signals

Status

⬜

---

## 11. Thread Information

Contains

- Thread List
- Thread Control Blocks

Connected Module

→ Threads

Status

⬜

---

# Context Switch

Context Switching saves

- Program Counter
- CPU Registers
- Stack Pointer
- CPU Flags

Context Switching restores

- Program Counter
- CPU Registers
- Stack Pointer
- CPU Flags

Connected Topics

- PCB
- Scheduling
- Interrupts
- Timer
- Dispatcher

Status

⬜

---

# Big Picture

```text
Interrupt
      │
      ▼
Kernel
      │
      ▼
Scheduler
      │
      ▼
Dispatcher
      │
      ▼
Context Switch
      │
      ▼
PCB
      │
      ├── Registers
      ├── Program Counter
      ├── Scheduling Info
      ├── Memory Info
      ├── File Info
      ├── Signals
      ├── Resources
      └── Threads
```

---

# Revision Checklist

## Processes

- [ ] Program vs Process
- [ ] PCB
- [ ] Program Counter
- [ ] CPU Registers
- [ ] Context Switching

## Scheduling

- [ ] FCFS
- [ ] SJF
- [ ] SRTF
- [ ] Priority
- [ ] Round Robin
- [ ] Dispatcher

## Memory

- [ ] Paging
- [ ] Segmentation
- [ ] Virtual Memory
- [ ] Page Tables
- [ ] TLB

## Synchronization

- [ ] Mutex
- [ ] Semaphore
- [ ] Monitor

## IPC

- [ ] Pipes
- [ ] Shared Memory
- [ ] Message Queue
- [ ] Socket

## File Systems

- [ ] File Descriptor
- [ ] Open File Table

## Threads

- [ ] TCB
- [ ] User Threads
- [ ] Kernel Threads

## Signals

- [ ] Signal Handling
- [ ] Pending Signals

---

# Final Thought

**The PCB is not one topic.**

It is the **center of the Operating Systems syllabus.**

Every major OS module eventually connects back to one or more fields inside the PCB.