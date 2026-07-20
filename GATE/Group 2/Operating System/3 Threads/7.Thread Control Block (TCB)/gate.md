# GATE Corner ⭐ — Thread Control Block (TCB)

## Core Concept

> **Thread Control Block (TCB)** is a data structure that stores the **execution context** of a thread so that it can be suspended and resumed correctly.

---

# Must Remember

- Every **thread has its own TCB**.
- **One TCB ↔ One Thread**
- Used during **Context Switching**.
- Stores **thread-specific information**, **not process resources**.
- Enables the kernel (or thread library in ULTs) to resume a thread from exactly where it stopped.

---

# Information Stored in TCB

| Information | Purpose |
|-------------|---------|
| Thread ID (TID) | Identifies the thread |
| Thread State | Ready, Running, Waiting, etc. |
| Program Counter (PC) | Next instruction to execute |
| CPU Registers | Stores current register values |
| Stack Pointer (SP) | Points to the thread's stack |
| Scheduling Information | Priority, Time Quantum, etc. |

---

# Context Switching

```
Running Thread A

↓

Save Context

↓

TCB A

↓

Scheduler selects Thread B

↓

Load Context

↓

TCB B

↓

Running Thread B
```

> **TCB is the storage location for a thread's execution context.**

---

# TCB vs PCB

| PCB | TCB |
|-----|-----|
| Represents a Process | Represents a Thread |
| One per Process | One per Thread |
| Stores Process Resources | Stores Execution Context |
| Address Space | Program Counter |
| Open Files | Registers |
| Heap | Stack Pointer |
| Global Variables | Thread State |
| Process ID (PID) | Thread ID (TID) |

---

# Memory Organization

```
Process

│
├── PCB
│
├── Shared Code
├── Shared Heap
├── Shared Data
│
├── Thread A
│      ├── Stack
│      └── TCB
│
├── Thread B
│      ├── Stack
│      └── TCB
│
└── Thread C
       ├── Stack
       └── TCB
```

---

# Common GATE Traps ⚠️

❌ Threads of the same process share one TCB.

✔ False.

Each thread has its **own TCB**.

---

❌ TCB stores the process address space.

✔ False.

Address space belongs to the **PCB**.

---

❌ Context Switching saves only the Program Counter.

✔ False.

It saves the complete execution context:
- Program Counter
- Registers
- Stack Pointer
- Thread State

---

❌ PCB and TCB store the same information.

✔ False.

PCB stores **process-level information**.
TCB stores **thread-level information**.

---

❌ TCB is used only during thread creation.

✔ False.

It is continuously used for:
- Scheduling
- Context Switching
- Resuming execution
- Managing thread state

---

# PYQ Focus 🎯

- PCB vs TCB
- Context Switching
- Execution Context
- Program Counter
- CPU Registers
- Stack Pointer
- Thread States
- Thread Scheduling