# 🎯 GATE Corner — Parent & Child Process

## Weightage

- ⭐⭐⭐ Very Important
- Frequently asked with:
  - `fork()`
  - `exec()`
  - `wait()`
  - Zombie Process
  - Orphan Process
  - Process Tree

---

# Must Remember Facts ⭐⭐⭐

### 1. Parent Process

> A process that creates another process.

---

### 2. Child Process

> A process created by another (parent) process.

Created using **`fork()`**.

---

### 3. Every Process Has

- Unique PID
- Separate PCB
- Separate Address Space

✅ Parent and child **do NOT** share PID or PCB.

---

### 4. Execution After `fork()`

After `fork()`:

- Parent continues execution.
- Child also starts execution.
- Both execute **concurrently** (independently).

> The execution order is **not guaranteed**.

---

### 5. Process Hierarchy

Processes form a **tree**.

```text
init (PID 1)
     │
     ├── Parent
     │      ├── Child 1
     │      └── Child 2
     │
     └── Another Process
```

---

### 6. Child Inherits

Child initially inherits:

- Open file descriptors
- Environment variables
- Current working directory
- User credentials

But gets its own:

- PID
- PCB
- Address Space

---

# GATE Tricks ⚠️

### ❌ Wrong Statement

> Parent and child have the same PID.

**False**

Every process has a unique PID.

---

### ❌ Wrong Statement

> Parent and child execute one after another.

**False**

They execute **concurrently**.

The scheduler decides who gets the CPU first.

---

### ❌ Wrong Statement

> Child shares the parent's PCB.

**False**

Each process has its own PCB.

---

### ❌ Wrong Statement

> Parent always finishes before the child.

**False**

Either process may finish first.

Execution order is **non-deterministic**.

---

### ✅ Correct Statement

> A child process may itself create more child processes.

---

# Common MCQs

### Q1

Which system call creates a child process?

A. wait()

B. exec()

C. fork()

D. exit()

✅ **Answer:** C

---

### Q2

Parent and child processes have

A. Same PID

B. Different PIDs

C. Same PCB

D. Same Stack

✅ **Answer:** B

---

### Q3

Immediately after `fork()`, the parent and child

A. Execute sequentially

B. Execute independently

C. Parent terminates

D. Child waits automatically

✅ **Answer:** B

---

### Q4

Which of the following is **not** shared between parent and child?

A. PID

B. Open file descriptors

C. Environment variables

D. Current working directory

✅ **Answer:** A

---

# PYQ Keywords

If a question contains these terms together, think of **Parent & Child Process**:

- fork()
- Parent
- Child
- PID
- PCB
- Process Tree
- wait()
- exec()

---

# Formula Corner 🧮

No mathematical formulas.

Remember these logical relationships:

```text
Parent
    │
 fork()
    │
    ▼
 Child
```

```text
Parent PID ≠ Child PID
```

```text
Parent PCB ≠ Child PCB
```

```text
Both execute concurrently
```

---

# One-Line Revision

> **Parent creates the child using `fork()`. Both have different PIDs, separate PCBs and memory, and execute independently.**