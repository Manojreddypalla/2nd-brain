## Definition

> **Process Creation** is the mechanism by which the Operating System creates a new process and allocates the required resources for its execution.

Whenever a program needs to execute, the OS creates a **process**.

---

# Why is a Process Created?

A new process may be created for several reasons:

- User starts an application (Chrome, VS Code, etc.)
- An existing process creates another process.
- System services (daemons) start during boot.
- OS creates background processes.

---

# Process Creation Steps

When a new process is created, the OS performs the following steps:

### 1. Assign a Process ID (PID)

Every process gets a unique **PID**.

Example:

```text
Chrome  → PID 1201
VS Code → PID 1202
```

---

### 2. Create PCB

The OS creates a **Process Control Block (PCB)** containing:

- PID
- Process State
- Program Counter
- CPU Registers
- Scheduling Information
- Memory Information
- Open Files

---

### 3. Allocate Memory

The OS allocates memory for:

- Code (Text Segment)
- Data Segment
- Heap
- Stack

```text
+----------------+
| Code           |
+----------------+
| Data           |
+----------------+
| Heap           |
+----------------+
| Stack          |
+----------------+
```

---

### 4. Allocate Resources

The process may receive:

- CPU time
- Main Memory
- Open Files
- I/O Devices
- Network Resources

---

### 5. Load Program

The executable program is loaded from secondary storage into RAM.

```text
Disk
  │
  ▼
RAM
```

---

### 6. Add to Ready Queue

Initially, the process is **Ready**.

It waits in the **Ready Queue** until the CPU Scheduler selects it.

```text
New
 │
 ▼
Ready Queue
 │
 ▼
Running
```

---

# Process Creation Flow

```text
Program
   │
   ▼
OS Creates Process
   │
   ├── Assign PID
   ├── Create PCB
   ├── Allocate Memory
   ├── Allocate Resources
   ├── Load Program
   └── Place in Ready Queue
           │
           ▼
      Scheduler Selects
           │
           ▼
        Running
```

---

# Parent and Child Process

A process can create another process.

```text
Parent Process
      │
      ▼
Child Process
```

The child receives its own:

- PID
- PCB
- Memory Space

This is commonly done using **fork()** (Unix/Linux).

---

# Key Points

- Every process has a unique PID.
- OS creates a PCB for every process.
- Memory and resources are allocated during creation.
- New process enters the **Ready** state after creation.
- Scheduler decides when it will execute.

---

# GATE Corner 🎯

## Must Remember

- Process creation is handled by the Operating System.
- Every new process gets a unique PID.
- PCB is created during process creation.
- New process is placed in the **Ready Queue**.
- Process creation involves memory and resource allocation.

---

## Common MCQs

### Q1

Which data structure is created when a process is created?

A. Page Table

B. PCB

C. Ready Queue

D. Stack

✅ **Answer: B**

---

### Q2

After creation, a process generally enters which state?

A. Running

B. Waiting

C. Ready

D. Terminated

✅ **Answer: C**

---

### Q3

Which of the following is **not** performed during process creation?

A. Assign PID

B. Create PCB

C. Allocate Memory

D. Execute Process Immediately

✅ **Answer: D**

---

# Quick Revision

```text
Process Creation

↓

Assign PID

↓

Create PCB

↓

Allocate Memory

↓

Allocate Resources

↓

Load Program

↓

Ready Queue

↓

Scheduler

↓

Running
```

## Keywords

- Process Creation
- PID
- PCB
- Memory Allocation
- Resource Allocation
- Ready Queue
- Parent Process
- Child Process