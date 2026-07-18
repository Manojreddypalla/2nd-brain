Absolutely. Below are **complete GATE-focused notes** on **Process Control Block (PCB)**. These are written the way I'd expect a serious GATE aspirant to revise them before the exam: concise enough for revision but deep enough to build understanding.

---

# Process Control Block (PCB)

## Definition

A **Process Control Block (PCB)** is a **kernel data structure** that stores all information required by the operating system to **manage, schedule, suspend, resume, and terminate a process**.

> **Key Idea:** The operating system knows a process through its **PCB**, not through the program file.

---

# Why is PCB Needed?

A CPU executes only **one process per core** at a time.

When the CPU switches from one process to another, the OS must remember:

- Which instruction was executing?
    
- What were the CPU register values?
    
- Which files were open?
    
- What was the process state?
    
- Which memory belongs to the process?
    

All this information is stored in the **PCB**.

Without a PCB, multitasking is impossible.

---

# When is PCB Created?

```
Program (Executable)
        ↓
Process Creation (fork/CreateProcess)
        ↓
PCB Created in Kernel
        ↓
Memory Allocated
        ↓
Ready Queue
        ↓
Running
```

A PCB is created **before the process starts executing**.

---

# Where is PCB Stored?

**Kernel Space**

Reason:

- User programs must not modify
    
    - Process state
        
    - Priority
        
    - Registers
        
    - Scheduling information
        
    - Security credentials
        

Therefore, PCB is protected by the kernel.

---

# PCB Contains

## 1. Process Identification

Stores

- Process ID (PID)
    
- Parent PID (PPID)
    
- User ID
    
- Group ID
    

Purpose

- Uniquely identifies the process.
    

---

## 2. Process State

Stores current state.

Possible states:

- New
    
- Ready
    
- Running
    
- Waiting (Blocked)
    
- Terminated
    

Used by the scheduler.

---

## 3. Program Counter (PC)

Stores

> Address of the next instruction to execute.

Without it,

the process would restart from the beginning after every context switch.

---

## 4. CPU Registers

Stores all CPU register values.

Examples

- General-purpose registers
    
- Stack Pointer (SP)
    
- Frame Pointer
    
- Instruction Pointer (Program Counter)
    
- Status/Flag Register
    

Purpose

Restore CPU exactly as it was before interruption.

---

## 5. CPU Scheduling Information

Includes

- Priority
    
- Scheduling class
    
- Time quantum
    
- Queue pointers
    
- CPU affinity (OS dependent)
    

Used by the scheduler.

---

## 6. Memory Management Information

Stores or points to

- Page Table
    
- Segment Table
    
- Base Register
    
- Limit Register
    
- Address Space Information
    

Purpose

Locate the process's memory.

---

## 7. I/O Information

Stores

- Open File Descriptors
    
- Device Information
    
- Pending I/O Requests
    
- Current Working Directory
    

Purpose

Resume I/O correctly after context switching.

---

## 8. Accounting Information

Stores

- CPU time used
    
- Process creation time
    
- User time
    
- Kernel time
    
- Resource usage
    

Useful for monitoring tools like:

- `top`
    
- `ps`
    
- Task Manager
    

---

## 9. Signal / Exception Information

Modern operating systems also maintain

- Pending signals
    
- Signal handlers
    
- Exception status
    

---

# PCB Diagram

```text
+----------------------------------+
| PID                              |
+----------------------------------+
| Process State                    |
+----------------------------------+
| Program Counter                  |
+----------------------------------+
| CPU Registers                    |
+----------------------------------+
| Stack Pointer                    |
+----------------------------------+
| Scheduling Information           |
+----------------------------------+
| Memory Management Information    |
+----------------------------------+
| I/O Status Information           |
+----------------------------------+
| Accounting Information           |
+----------------------------------+
```

---

# Process vs PCB

|Process|PCB|
|---|---|
|Executing program|Kernel data structure|
|Contains code, heap, stack|Contains metadata about process|
|Mostly in user memory|In kernel memory|
|Executes instructions|Managed by OS|

---

# PCB vs Process Context

Process Context

Contains only

- Program Counter
    
- Registers
    
- Stack Pointer
    
- CPU Flags
    

PCB

Contains

- Process Context
    
- Scheduling Info
    
- Memory Info
    
- PID
    
- Open Files
    
- Accounting
    
- Signals
    

**Context ⊂ PCB**

---

# PCB During Context Switching

Suppose

```
CPU

PC = 1000
SP = 7000
R1 = 10
```

Timer interrupt occurs.

Step 1

Save

```
CPU Registers
        ↓
PCB(P1)
```

Scheduler selects P2.

Step 2

Restore

```
PCB(P2)
        ↓
CPU Registers
```

Execution resumes.

This is called

**Context Switching**.

---

# Important Observation

The CPU has

- One Program Counter
    
- One Register Set
    

Each process has

- Saved copy inside its PCB
    

During context switching

```
CPU
 ↓ Save
PCB(P1)

Scheduler

PCB(P2)
 ↓ Restore
CPU
```

---

# PCB Does NOT Store

❌ Program Code

❌ Heap

❌ Stack Data

❌ Variables

Instead,

PCB stores **references/pointers** to these structures.

---

# PCB Contains Pointers To

Modern kernels avoid duplication.

PCB usually points to

- Page Table
    
- Open File Table
    
- Memory Descriptor
    
- Credential Structure
    
- Signal Structure
    
- Thread Information
    

---

# PCB and Scheduler

Scheduler works with PCBs.

Ready Queue

```
PCB1 → PCB2 → PCB3 → PCB4
```

Scheduler selects a PCB.

Context is restored.

Process runs.

---

# PCB Lifecycle

```
Process Created
        ↓
PCB Allocated
        ↓
Ready
        ↓
Running
        ↓
Waiting
        ↓
Ready
        ↓
Running
        ↓
Exit
        ↓
PCB Destroyed
```

---

# PCB in Linux

Linux does not literally call it "PCB."

Linux equivalent:

```c
struct task_struct
```

Windows equivalent

- EPROCESS
    

Concept remains the same.

---

# Threads and PCB

One process

```
PCB
```

may contain

```
Thread 1
Thread 2
Thread 3
```

Each thread has its own execution context.

Modern operating systems therefore use another structure called

**Thread Control Block (TCB).**

---

# Context Switch Overhead

Context switching requires

- Save CPU context
    
- Restore CPU context
    

Therefore

- CPU performs no useful work during switching.
    
- Frequent switching decreases performance.
    

Smaller Time Quantum

↓

More Context Switches

↓

More Overhead

↓

Lower CPU Utilization

---

# Formula Sheet

### Context Switch Time

```
Context Switch Time

=
Save Time

+

Restore Time
```

---

### CPU Utilization

```
CPU Utilization

=

Execution Time
----------------------------
Execution Time + Context Switch Time

×100
```

---

### PCB Memory Requirement

```
PCB Memory

=

PCB Size

×

Number of Processes
```

---

### Register Storage

```
Register Storage

=

Number of Registers

×

Size of Each Register
```

---

# GATE Important Points

✔ PCB is stored in **Kernel Space**.

✔ Every process has exactly **one PCB**.

✔ PCB is created before execution starts.

✔ PCB is destroyed after process termination.

✔ PCB stores **metadata**, not program code.

✔ Context is stored inside PCB.

✔ Scheduler operates using PCBs.

✔ Context switching saves/restores information from PCB.

✔ Linux PCB is implemented as `task_struct`.

✔ Modern PCBs mainly store **pointers** to other kernel structures rather than duplicating all process data.

---

# Frequently Asked GATE MCQs

### Q1

PCB is stored in

A. Heap

B. Stack

C. User Space

D. Kernel Space

**Answer:** D

---

### Q2

Which field allows a process to resume execution?

A. PID

B. Program Counter

C. Priority

D. Process State

**Answer:** B

---

### Q3

PCB stores

A. Program code

B. Heap

C. CPU execution state

D. Executable file

**Answer:** C

---

### Q4

During context switching, the OS primarily saves and restores

A. Entire process memory

B. CPU context

C. Program source code

D. Hard disk contents

**Answer:** B

---

### Q5

Linux implementation of PCB is

A. inode

B. `task_struct`

C. file_struct

D. vm_area_struct

**Answer:** B

---

# One-Page Revision

- **PCB = Kernel data structure representing a process.**
    
- **Stored in Kernel Space.**
    
- **One Process ↔ One PCB.**
    
- Contains: **PID, Process State, Program Counter, CPU Registers, Stack Pointer, Scheduling Info, Memory Info, I/O Info, Accounting Info, Signal Info.**
    
- Stores **metadata and pointers**, not the actual code or heap.
    
- Used for **process management, scheduling, and context switching**.
    
- **Context = CPU execution state**; **PCB = Context + process metadata**.
    
- **Context switching** saves the current CPU state into one PCB and restores another process's state from its PCB.
    
- Linux uses **`task_struct`** as its PCB implementation.
    
- **Frequent context switches increase overhead and reduce CPU utilization.**
    

These notes are sufficient for **GATE**, while also giving you the deeper understanding needed for interviews and operating system internals.