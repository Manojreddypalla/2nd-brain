# Module 2 — Process Termination

## Definition

> **Process Termination** is the process by which the Operating System removes a process from execution and releases all the resources allocated to it.

Once terminated, the process no longer exists in the system.

---

# Why Does a Process Terminate?

A process may terminate due to:

### 1. Normal Completion

The program finishes successfully.

Example:

```text
printf("Hello");
return 0;
```

---

### 2. Error Exit

The process detects an error and exits.

Example:

```text
File not found
Memory allocation failed
```

---

### 3. Fatal Error

The OS forcibly terminates the process.

Examples:

- Divide by zero
- Segmentation Fault
- Illegal Instruction
- Stack Overflow

---

### 4. Killed by Another Process

Another process or the user terminates it.

Examples:

```bash
kill PID
kill -9 PID
```

---

# What Happens During Process Termination?

The Operating System performs the following steps:

### 1. Stop Process Execution

CPU stops executing the process.

↓

### 2. Release Memory

The OS frees:

- Code Segment
- Data Segment
- Heap
- Stack

↓

### 3. Close Open Files

All open files and I/O resources are released.

↓

### 4. Remove PCB

The Process Control Block (PCB) is deleted.

↓

### 5. Notify Parent Process

The exit status is stored until the parent collects it using **wait()**.

↓

### 6. Remove Process from Process Table

The process is completely removed from the system.

---

# Process Termination Flow

```text
Running Process
       │
       ▼
Termination Requested
       │
       ▼
Stop Execution
       │
       ▼
Release Memory
       │
       ▼
Close Files
       │
       ▼
Remove PCB
       │
       ▼
Notify Parent
       │
       ▼
Process Deleted
```

---

# Exit Status

Every process returns an **Exit Status**.

Example:

```c
return 0;
```

Common values:

- `0` → Successful execution
- Non-zero → Error occurred

The parent process reads this value using **wait()**.

---

# Important Note

If the parent **does not** collect the exit status immediately,

the terminated process becomes a

> **Zombie Process**

until the parent calls **wait()**.

(Studied in the Zombie Process topic.)

---

# Key Points

- Process termination frees system resources.
- PCB is removed.
- Memory is deallocated.
- Files are closed.
- Exit status is returned to the parent.
- Process is removed from the process table.

---

# 🎯 GATE Corner

## Must Remember

- Process termination releases all allocated resources.
- PCB is removed after termination.
- Exit status is collected by the parent using **wait()**.
- Failure to collect exit status results in a **Zombie Process**.

---

## Common MCQs

### Q1

Which structure is removed after process termination?

A. Page Table

B. PCB

C. Ready Queue

D. Cache

✅ **Answer:** PCB

---

### Q2

Which system call is used by the parent to collect the child's exit status?

A. fork()

B. exec()

C. wait()

D. exit()

✅ **Answer:** wait()

---

### Q3

A process has terminated, but its parent has not collected its exit status. The process is called:

A. Orphan

B. Zombie

C. Waiting

D. Ready

✅ **Answer:** Zombie

---

### Q4

Which resource is released during process termination?

A. Memory

B. Open Files

C. PCB

D. All of the Above

✅ **Answer:** D

---

# Formula Corner 🧮

There are **no mathematical formulas** for Process Termination.

Remember this lifecycle instead:

```text
Running
      ↓
exit() / Error / kill()
      ↓
Terminate
      ↓
Release Resources
      ↓
Parent collects status (wait())
      ↓
Process Removed
```

---

# One-Line Revision

> **Process Termination = Stop execution + Free resources + Delete PCB + Return exit status.**

---

# Keywords

- Process Termination
- exit()
- Exit Status
- PCB
- wait()
- Zombie Process
- Resource Deallocation
- Process Table