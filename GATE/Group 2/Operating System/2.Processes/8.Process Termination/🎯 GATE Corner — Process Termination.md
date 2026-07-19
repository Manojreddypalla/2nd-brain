# 🎯 GATE Corner — Process Termination

## Weightage

- ⭐⭐ Direct questions are rare.
- ⭐⭐⭐ Frequently asked along with **fork(), wait(), Zombie Process, Orphan Process, exit()**.

---

# Must Remember Facts ⭐⭐⭐

### 1. Process Termination

> A terminated process **releases all allocated resources** and is removed from the system.

---

### 2. Resources Released

During termination, the OS releases:

- Main Memory
- CPU Resources
- Open Files
- I/O Resources
- PCB (after cleanup)

---

### 3. Exit Status ⭐⭐⭐

Every process returns an **Exit Status**.

```c
return 0;
```

- `0` → Successful execution
- Non-zero → Error/Abnormal termination

The parent collects it using **wait()**.

---

### 4. Process Removal

The process is **not completely removed immediately**.

The OS first:

- Stores exit status
- Waits for parent to collect it

Only then is the PCB completely removed.

---

### 5. Ways a Process Terminates

- Normal completion (`exit()`)
- Error during execution
- Fatal error (Segmentation Fault, Divide by Zero)
- Killed by another process (`kill`)

---

# GATE Tricks ⚠️

### ❌ Wrong Statement

> A terminated process always disappears immediately.

**False**

It may become a **Zombie Process** until the parent calls **wait()**.

---

### ❌ Wrong Statement

> `wait()` terminates the child process.

**False**

The child has **already terminated**.

`wait()` only collects the child's exit status.

---

### ❌ Wrong Statement

> PCB is deleted before storing exit status.

**False**

The PCB is retained temporarily so the parent can obtain the child's exit status.

---

### ✅ Correct Statement

> Failure of the parent to call `wait()` results in a Zombie Process.

---

# Common MCQs

### Q1

Which system call is used to terminate a process normally?

A. fork()

B. exec()

C. exit()

D. wait()

✅ **Answer:** C

---

### Q2

Which system call collects the child's exit status?

A. fork()

B. exec()

C. wait()

D. kill()

✅ **Answer:** C

---

### Q3

After termination, the process may remain in the process table because

A. CPU is busy

B. Memory is full

C. Parent has not collected exit status

D. Scheduler is waiting

✅ **Answer:** C

---

### Q4

Which of the following is **not** released during process termination?

A. Memory

B. Open Files

C. CPU Resources

D. Parent Process

✅ **Answer:** D

---

# Formula Corner 🧮

There are **no numerical formulas**.

Remember this sequence:

```text
Running
      ↓
exit()
      ↓
Resources Released
      ↓
Exit Status Stored
      ↓
Parent calls wait()
      ↓
PCB Removed
```

---

# PYQ Keywords

Whenever you see these words together, think **Process Termination**:

- exit()
- wait()
- Exit Status
- Zombie Process
- PCB
- Resource Deallocation
- Process Table

---

# 10-Second Revision

- `exit()` → Terminates the process.
- Resources are released.
- Exit status is stored.
- Parent collects it using `wait()`.
- If `wait()` is not called → **Zombie Process**.
- After collection, the process is completely removed.