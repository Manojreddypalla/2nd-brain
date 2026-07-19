# Module 1.4 — Process Control System Calls

## Definition

> **Process Control System Calls** are system calls used to **create, execute, manage, synchronize, and terminate processes**.

They allow a program to control the **entire lifecycle of a process**.

---

# Why Do We Need Process Control?

An operating system must manage multiple processes efficiently.

Process Control System Calls allow programs to:

- Create new processes
- Execute another program
- Wait for child processes
- Terminate processes
- Obtain process information

Without these system calls, programs **could not manage processes**.

---

# Common Process Control System Calls

| System Call | Purpose |
|-------------|---------|
| `fork()` | Creates a new child process |
| `exec()` | Replaces the current process with a new program |
| `wait()` | Parent waits for a child process to finish |
| `exit()` | Terminates the current process |
| `getpid()` | Returns the Process ID (PID) |
| `getppid()` | Returns the Parent Process ID (PPID) |
| `kill()` | Sends a signal to another process |

---

# Process Lifecycle Using System Calls

```text
Program
    │
    ▼
fork()
    │
    ▼
Parent + Child
    │
    ▼
exec()
    │
    ▼
Running Process
    │
    ▼
wait()
    │
    ▼
exit()
    │
    ▼
Process Terminated
```

---

# Example

Suppose you open Google Chrome.

```text
Chrome Launcher
      │
      ▼
fork()
      │
Creates New Process
      │
      ▼
exec()
      │
Loads Chrome Program
      │
      ▼
Chrome Running
      │
      ▼
exit()
```

---

# How It Works

### Step 1

A process wants to create another process.

↓

### Step 2

It calls

```c
fork()
```

↓

### Step 3

The operating system creates a child process.

↓

### Step 4

The child may execute another program using

```c
exec()
```

↓

### Step 5

The parent waits using

```c
wait()
```

↓

### Step 6

The process finishes using

```c
exit()
```

---

# Why Are These System Calls Important?

They allow the operating system to:

- Manage multitasking
- Run multiple applications
- Synchronize parent and child processes
- Release resources correctly
- Prevent resource leaks

---

# Key Points

- Used to manage processes.
- Execute in **Kernel Mode**.
- Part of the Process Management category of System Calls.
- Work closely with PCB, Scheduler, and Dispatcher.
- Form the complete process lifecycle.

---

# Relationship with Previous Topics

```text
Program

↓

Process Created

↓

fork()

↓

Parent + Child

↓

exec()

↓

Running

↓

wait()

↓

exit()

↓

Zombie / Orphan (if not handled properly)
```

---

# 🎯 GATE Corner

## Must Remember ⭐⭐⭐⭐⭐

- Process Control System Calls manage the **lifecycle of processes**.
- `fork()` creates a child process.
- `exec()` loads a new program into the current process.
- `wait()` synchronizes parent and child.
- `exit()` terminates a process.
- `getpid()` returns the current process ID.
- `kill()` sends signals to processes.

---

## GATE Tricks ⚠️

### ❌ Wrong Statement

> `exec()` creates a new process.

**False**

It **replaces** the current process image.

---

### ❌ Wrong Statement

> `wait()` terminates a child process.

**False**

It **waits** for the child to terminate.

---

### ❌ Wrong Statement

> `fork()` replaces the current process.

**False**

It creates a **new child process**.

---

### ✅ Correct Statement

> Process Control System Calls are used to create, execute, synchronize, and terminate processes.

---

## Common MCQs

### Q1

Which System Call creates a new process?

A. `exec()`

B. `fork()`

C. `wait()`

D. `exit()`

✅ **Answer:** **B**

---

### Q2

Which System Call replaces the current process image?

A. `fork()`

B. `wait()`

C. `exec()`

D. `kill()`

✅ **Answer:** **C**

---

### Q3

Which System Call waits for a child process to finish?

A. `wait()`

B. `exit()`

C. `getpid()`

D. `fork()`

✅ **Answer:** **A**

---

### Q4

Which System Call returns the Process ID?

A. `exec()`

B. `fork()`

C. `getpid()`

D. `kill()`

✅ **Answer:** **C**

---

# Formula Corner 🧮

There are **no mathematical formulas**.

Remember the process lifecycle:

```text
fork()

↓

exec()

↓

wait()

↓

exit()
```

---

# One-Line Revision

> **Process Control System Calls manage the complete lifecycle of a process—from creation (`fork()`) to execution (`exec()`), synchronization (`wait()`), and termination (`exit()`).**