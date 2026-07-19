# 🎯 GATE Corner — Process Control System Calls

## Weightage

⭐⭐⭐⭐⭐ **Very Important**

Frequently asked with:

- System Calls
- Processes
- Parent & Child Process
- Process Creation
- Process Termination
- Zombie Process
- Orphan Process

---

# Must Remember ⭐⭐⭐⭐⭐

Process Control System Calls are used to

- Create processes
- Execute programs
- Synchronize processes
- Terminate processes
- Get process information
- Send signals to processes

---

## Important System Calls

| System Call | Function |
|-------------|----------|
| `fork()` | Creates a child process |
| `exec()` | Replaces current process image |
| `wait()` | Waits for child process |
| `exit()` | Terminates current process |
| `getpid()` | Returns Process ID (PID) |
| `getppid()` | Returns Parent Process ID (PPID) |
| `kill()` | Sends a signal to another process |

---

# Remember This Sequence ⭐⭐⭐⭐⭐

```text
fork()

↓

exec()

↓

wait()

↓

exit()
```

This represents the **typical lifecycle of a process**.

---

# Most Asked Concepts

### `fork()`

- Creates a **new child process**
- Parent and child execute independently
- Returns twice (once in parent, once in child)

---

### `exec()`

- **Does NOT create** a new process
- Replaces the current program
- PID remains the same

---

### `wait()`

- Parent blocks until the child terminates
- Prevents Zombie processes

---

### `exit()`

- Terminates the current process
- Returns an exit status to the parent

---

# GATE Tricks ⚠️

### ❌ Wrong Statement

> `exec()` creates a new process.

**False**

It replaces the current process image.

---

### ❌ Wrong Statement

> `wait()` kills the child process.

**False**

It only waits for the child to terminate.

---

### ❌ Wrong Statement

> `fork()` replaces the current program.

**False**

`fork()` creates a child process.

---

### ❌ Wrong Statement

> `getpid()` returns the Parent PID.

**False**

It returns the **current process PID**.

---

### ✅ Correct Statement

> Process Control System Calls manage the lifecycle of processes.

---

# Common MCQs

### Q1

Which System Call creates a child process?

A. `exec()`

B. `wait()`

C. `fork()`

D. `exit()`

✅ **Answer:** **C**

---

### Q2

Which System Call replaces the current process image?

A. `fork()`

B. `exec()`

C. `kill()`

D. `wait()`

✅ **Answer:** **B**

---

### Q3

Which System Call blocks the parent until the child terminates?

A. `exit()`

B. `wait()`

C. `fork()`

D. `getpid()`

✅ **Answer:** **B**

---

### Q4

Which System Call returns the current Process ID?

A. `getpid()`

B. `getppid()`

C. `wait()`

D. `kill()`

✅ **Answer:** **A**

---

### Q5

Which of the following belongs to the **Process Control** category?

A. `read()`

B. `write()`

C. `fork()`

D. `open()`

✅ **Answer:** **C**

---

# PYQ Keywords

Whenever you see these words, think of **Process Control**:

- Process Creation
- Parent Process
- Child Process
- PID
- PPID
- `fork()`
- `exec()`
- `wait()`
- `exit()`
- `kill()`
- Zombie
- Orphan

---

# Memory Trick 🧠

```text
Create

↓

Run

↓

Wait

↓

Terminate
```

Translate it into system calls:

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

# 15-Second Revision 🚀

```text
Process Control

↓

fork()   → Create

exec()   → Replace Program

wait()   → Synchronize

exit()   → Terminate

getpid() → Process ID

kill()   → Send Signal
```

---

# Golden Rule ⭐

> **If a system call is related to creating, executing, managing, synchronizing, or terminating a process, it belongs to the _Process Control_ category.**