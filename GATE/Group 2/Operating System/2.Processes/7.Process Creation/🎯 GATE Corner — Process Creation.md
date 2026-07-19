# 🎯 GATE Corner — Process Creation

## Weightage

- ⭐ Direct questions are less common.
- ⭐⭐⭐ Indirect questions are very common through **fork(), exec(), wait(), PCB, Process States, Scheduling**.

---

# Must Remember Facts

### 1. Process Creation

> **Process Creation is the process of creating a new process by the Operating System.**

---

### 2. Every Process Gets

- Unique PID
- PCB
- Address Space
- CPU Registers
- Resources (Memory, Files, I/O)

---

### 3. Initial State

A newly created process enters the

> **Ready State**

NOT Running.

The CPU Scheduler decides when it executes.

---

### 4. Who Creates a Process?

Possible sources:

- User starts a program
- Parent process creates child (`fork()`)
- Operating System
- System initialization (boot)

---

### 5. Resources Inherited by Child

Generally, the child inherits:

- Open file descriptors
- Environment variables
- Current working directory
- User credentials

The child gets its **own PID** and **own PCB**.

---

# Important GATE Tricks

### ❌ Wrong Statement

> A newly created process immediately starts execution.

**False**

It first enters the **Ready Queue**.

---

### ❌ Wrong Statement

> Parent and child have the same PID.

**False**

Every process has a unique PID.

---

### ❌ Wrong Statement

> PCB is shared between parent and child.

**False**

Each process has its own PCB.

---

### ✅ Correct Statement

> Child process receives its own address space and PCB.

---

# Process Creation Flow

```text
Program

↓

OS Creates Process

↓

Assign PID

↓

Create PCB

↓

Allocate Memory

↓

Allocate Resources

↓

Ready Queue

↓

CPU Scheduler

↓

Dispatcher

↓

Running
```

---

# Frequently Asked MCQs

### Q1

Which structure stores information about a newly created process?

A. Page Table

B. PCB

C. Stack

D. Ready Queue

✅ **Answer:** PCB

---

### Q2

Immediately after creation, a process is usually in

A. Running

B. Waiting

C. Ready

D. Suspended

✅ **Answer:** Ready

---

### Q3

Which component assigns CPU after process creation?

A. Compiler

B. Loader

C. Scheduler + Dispatcher

D. PCB

✅ **Answer:** Scheduler + Dispatcher

---

### Q4

Every process must have

A. Same PID

B. Unique PID

C. Shared PID

D. No PID

✅ **Answer:** Unique PID

---

# Formula Corner 🧮

There are **no mathematical formulas** for Process Creation in GATE.

Instead, remember these logical relationships:

```text
Program
        +
OS
        ↓
Process
```

```text
Process
      ↓
PID + PCB + Memory + Resources
```

```text
New
 ↓
Ready
 ↓
Running
```

---

# 10-Second Revision

- OS creates the process.
- Unique PID assigned.
- PCB created.
- Memory & resources allocated.
- Process enters **Ready Queue**.
- Scheduler selects it.
- Dispatcher gives it the CPU.

---

# PYQ Keywords

If you see these words together, think **Process Creation**:

- PID
- PCB
- Ready Queue
- Parent Process
- Child Process
- `fork()`
- `exec()`
- `wait()`
- Scheduler
- Dispatcher