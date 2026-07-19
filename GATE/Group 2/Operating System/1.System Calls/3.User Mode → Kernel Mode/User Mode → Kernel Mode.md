# Module 1 — User Mode → Kernel Mode

## Definition

> **User Mode** is the restricted execution mode where user applications run.

> **Kernel Mode** is the privileged execution mode where the Operating System executes and has complete access to system resources.

Whenever a user program requests an OS service through a **System Call**, the CPU switches from **User Mode** to **Kernel Mode**.

---

# Why Are There Two Modes?

Modern operating systems use two execution modes for:

- Security
- Protection
- Stability
- Controlled hardware access

Without separate modes, any application could:

- Access hardware directly
- Modify system memory
- Crash the entire operating system

---

# User Mode

## Characteristics

- Limited privileges
- Cannot access hardware directly
- Cannot execute privileged instructions
- Runs application programs

Examples:

- Chrome
- VS Code
- Calculator
- Games

---

# Kernel Mode

## Characteristics

- Full privileges
- Direct hardware access
- Executes privileged instructions
- Controls CPU, Memory, Files, Devices

Examples:

- Process creation
- Memory management
- File system operations
- Device drivers

---

# Comparison

| User Mode | Kernel Mode |
|------------|-------------|
| Limited privileges | Full privileges |
| No direct hardware access | Direct hardware access |
| Runs applications | Runs Operating System |
| Restricted memory access | Can access entire memory |
| Cannot execute privileged instructions | Can execute privileged instructions |

---

# Mode Switching

Suppose a program wants to read a file.

```text
User Program
      │
read()
      │
System Call
      ▼
Kernel Mode
      │
Kernel reads file
      │
Returns data
      ▼
User Mode
```

The CPU automatically changes modes during the System Call.

---

# Complete Flow

```text
Application
(User Mode)
      │
Need OS Service
      │
System Call
      │
Mode Switch
      ▼
Kernel Mode
      │
Kernel Executes Request
      │
Return Result
      │
Mode Switch
      ▼
User Mode
      │
Application Continues
```

---

# When Does Mode Switching Occur?

Mode switching occurs during:

- System Calls
- Hardware Interrupts
- Exceptions
- Traps

Example:

```text
printf()

↓

write()

↓

Kernel Mode

↓

Display Output

↓

Back to User Mode
```

---

# Why Is Mode Switching Necessary?

The kernel performs operations that user programs cannot perform safely.

Examples:

- Reading from disk
- Creating a process
- Allocating memory
- Accessing network devices
- Communicating with hardware

---

# Does Mode Switching Mean Context Switching?

**No.**

A **Mode Switch** changes only the CPU's privilege level.

A **Context Switch** changes the currently executing process or thread.

---

# Mode Switch vs Context Switch ⭐⭐⭐⭐⭐

| Mode Switch | Context Switch |
|--------------|----------------|
| User Mode ↔ Kernel Mode | One Process/Thread → Another |
| Same process continues | Different process/thread executes |
| Changes privilege level | Changes execution context |
| Happens during every System Call | Happens during scheduling |
| Faster | More expensive |

---

# Example

### Mode Switch

```text
Chrome

↓

read()

↓

Kernel Mode

↓

Reads File

↓

Back to Chrome
```

**Same process continues.**

---

### Context Switch

```text
Chrome Running

↓

Scheduler

↓

VS Code Running
```

**Different process starts executing.**

---

# Key Points

- Applications execute in User Mode.
- Operating System executes in Kernel Mode.
- System Calls cause User Mode → Kernel Mode switch.
- After the service completes, execution returns to User Mode.
- Every Context Switch involves saving/restoring process state.
- Every System Call causes a Mode Switch, but **not every System Call causes a Context Switch**.

---

# 🎯 GATE Corner

## Must Remember ⭐⭐⭐⭐⭐

- User Mode = Restricted privileges.
- Kernel Mode = Full privileges.
- System Calls trigger a Mode Switch.
- Mode Switch ≠ Context Switch.
- Hardware can only be accessed in Kernel Mode.

---

## GATE Tricks ⚠️

### ❌ Wrong Statement

> User programs execute in Kernel Mode.

**False**

They execute in **User Mode**.

---

### ❌ Wrong Statement

> Every System Call causes a Context Switch.

**False**

Every System Call causes a **Mode Switch**.

A Context Switch occurs only if the scheduler switches execution to another process/thread.

---

### ❌ Wrong Statement

> User Mode can directly access hardware.

**False**

Only Kernel Mode can.

---

### ✅ Correct Statement

> A System Call changes the CPU from User Mode to Kernel Mode.

---

## Common MCQs

### Q1

Applications normally execute in

A. Kernel Mode

B. User Mode

C. Supervisor Mode

D. Interrupt Mode

✅ **Answer:** **B**

---

### Q2

Which mode has direct access to hardware?

A. User Mode

B. Kernel Mode

C. Guest Mode

D. Virtual Mode

✅ **Answer:** **B**

---

### Q3

Which event always causes a Mode Switch?

A. System Call

B. Context Switch

C. Cache Miss

D. Page Replacement

✅ **Answer:** **A**

---

### Q4

Which statement is correct?

A. Mode Switch changes the running process.

B. Context Switch changes only privilege level.

C. Mode Switch changes CPU privilege level.

D. User Mode has full hardware access.

✅ **Answer:** **C**

---

# Formula Corner 🧮

There are **no mathematical formulas**.

Remember the flow:

```text
User Mode
      │
System Call
      ▼
Kernel Mode
      │
OS Service
      ▼
User Mode
```

---

# Memory Trick 🧠

```text
Need OS Service

↓

System Call

↓

User Mode

↓

Kernel Mode

↓

Kernel Executes

↓

User Mode
```

---

# One-Line Revision

> **A System Call temporarily switches the CPU from User Mode to Kernel Mode so the operating system can safely perform privileged operations, then returns control to User Mode.**