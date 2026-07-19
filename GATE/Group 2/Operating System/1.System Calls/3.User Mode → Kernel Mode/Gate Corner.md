# 🎯 GATE Corner — User Mode → Kernel Mode

## Weightage

⭐⭐⭐⭐⭐ **Very Important**

Frequently asked with:

- System Calls
- Context Switching
- Interrupts
- Exceptions
- Privileged Instructions
- CPU Modes

---

# Must Remember ⭐⭐⭐⭐⭐

## 1. CPU Modes

The CPU operates in two modes:

| User Mode | Kernel Mode |
|-----------|-------------|
| Restricted Privileges | Full Privileges |
| Runs Applications | Runs Operating System |
| No Direct Hardware Access | Direct Hardware Access |
| Cannot Execute Privileged Instructions | Can Execute Privileged Instructions |

---

## 2. System Call

A **System Call** causes

```text
User Mode

↓

Kernel Mode

↓

OS Service

↓

User Mode
```

This is called a **Mode Switch**.

---

## 3. Mode Switch ⭐⭐⭐

A Mode Switch changes

- CPU Privilege Level

It **does not** change

- Running Process
- PCB
- CPU Context

---

## 4. Context Switch ⭐⭐⭐⭐⭐

A Context Switch changes

- Running Process
- Running Thread

It involves

- Saving Registers
- Restoring Registers
- PCB Access

---

# Mode Switch vs Context Switch ⭐⭐⭐⭐⭐

| Mode Switch | Context Switch |
|-------------|----------------|
| User ↔ Kernel | Process A ↔ Process B |
| Same process continues | Different process/thread executes |
| Changes privilege level | Changes execution context |
| Happens during every system call | Happens during scheduling |
| Faster | Slower (more overhead) |

---

# Most Asked Concept ⭐⭐⭐⭐⭐

```
System Call

↓

Mode Switch

NOT

Context Switch
```

A **System Call always causes a Mode Switch**.

A **Context Switch happens only if the scheduler decides to run another process or thread**.

---

# GATE Tricks ⚠️

### ❌ Wrong Statement

> Every System Call causes a Context Switch.

**False**

Every System Call causes a **Mode Switch**.

A Context Switch may or may not occur.

---

### ❌ Wrong Statement

> Mode Switching changes the running process.

**False**

The **same process** continues after the kernel finishes.

---

### ❌ Wrong Statement

> User Mode can access hardware directly.

**False**

Only Kernel Mode has direct hardware access.

---

### ❌ Wrong Statement

> User programs execute privileged instructions.

**False**

Only the kernel executes privileged instructions.

---

### ✅ Correct Statement

> A System Call temporarily transfers control from User Mode to Kernel Mode.

---

# Common MCQs

### Q1

A System Call causes

A. Context Switch

B. Mode Switch

C. Page Fault

D. Scheduling

✅ **Answer:** **B**

---

### Q2

Which mode has unrestricted access to hardware?

A. User Mode

B. Kernel Mode

C. Guest Mode

D. Virtual Mode

✅ **Answer:** **B**

---

### Q3

Which statement is true?

A. User programs execute privileged instructions.

B. Every System Call changes the running process.

C. Kernel Mode has full privileges.

D. Mode Switching changes the PCB.

✅ **Answer:** **C**

---

### Q4

Which of the following necessarily occurs during a System Call?

A. Context Switch

B. Mode Switch

C. Process Creation

D. CPU Scheduling

✅ **Answer:** **B**

---

# PYQ Keywords

Whenever you see these words, think of this topic:

- User Mode
- Kernel Mode
- Privileged Instructions
- System Call
- Mode Switch
- Context Switch
- CPU Modes
- Interrupt
- Trap
- Exception

---

# 20-Second Revision 🚀

```text
Applications

↓

User Mode

↓

System Call

↓

Kernel Mode

↓

Kernel Executes

↓

User Mode
```

Remember:

```text
System Call

↓

Mode Switch

≠

Context Switch
```

---

# Memory Trick 🧠

```text
Need OS Service

↓

System Call

↓

User → Kernel

↓

Kernel Works

↓

Kernel → User
```

**Golden Rule for GATE**

- **Mode Switch = Privilege changes**
- **Context Switch = Process/Thread changes**