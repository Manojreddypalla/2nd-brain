# Module 1 — Why System Calls Exist

## Definition

> **System Calls exist to provide a safe and controlled way for user programs to access Operating System services.**

A user program **cannot directly access hardware or kernel resources**, so it must request the Operating System through a **System Call**.

---

# Why Can't User Programs Access Hardware Directly?

User programs run in **User Mode**, which has limited privileges.

For security and stability, they **cannot**:

- Access hardware directly
- Create or terminate processes
- Allocate physical memory
- Read or write disk blocks directly
- Control I/O devices

Only the **Kernel** has these privileges.

---

# The Need for System Calls

Whenever a user program needs an OS service, it requests the kernel through a **System Call**.

Examples:

- Create a process
- Read a file
- Write to a file
- Allocate memory
- Send data over the network

Without System Calls, applications would have **no way** to use OS resources safely.

---

# Real-Life Analogy

Imagine a bank.

```text
Customer
      │
      ▼
Bank Counter
      │
      ▼
Bank Vault
```

- Customer → User Program
- Bank Counter → System Call
- Bank Vault → Kernel

The customer **cannot directly enter the vault**.

Similarly,

A user program **cannot directly access kernel resources**.

---

# Why Not Give Every Program Full Access?

If every program could directly access hardware:

- One program could overwrite another program's memory.
- Any application could delete system files.
- Malware could control hardware.
- A bug in one application could crash the entire operating system.

System Calls prevent these problems.

---

# Services Provided Through System Calls

System Calls allow user programs to:

### Process Management

- Create processes
- Terminate processes
- Wait for child processes

Examples:

```text
fork()
exec()
wait()
exit()
```

---

### File Management

- Open files
- Read files
- Write files
- Close files

---

### Device Management

- Access keyboard
- Access printer
- Access disk
- Access USB devices

---

### Memory Management

- Allocate memory
- Free memory
- Map memory

---

### Communication

- Pipes
- Sockets
- Shared Memory
- Message Queues

---

# Complete Flow

```text
User Program
      │
Needs OS Service
      │
      ▼
System Call
      │
      ▼
Kernel
      │
Performs Operation
      │
Returns Result
      ▼
User Program
```

---

# Advantages of System Calls

- Secure access to hardware
- Protects operating system
- Prevents unauthorized access
- Resource management
- Hardware abstraction
- Controlled communication between applications and kernel

---

# Key Points

- User programs cannot directly access kernel resources.
- System Calls provide controlled access to Operating System services.
- Kernel performs privileged operations.
- Security and stability are the main reasons System Calls exist.

---

# 🎯 GATE Corner

## Must Remember ⭐⭐⭐⭐⭐

- System Calls exist because **User Mode has restricted privileges**.
- Only the **Kernel** can access hardware directly.
- System Calls provide a **controlled interface** between user programs and the kernel.
- Every privileged operation requires a System Call.

---

## GATE Tricks ⚠️

### ❌ Wrong Statement

> User programs can directly access hardware.

**False**

Only the kernel can.

---

### ❌ Wrong Statement

> System Calls exist only for file operations.

**False**

They are used for process management, memory, files, devices, communication, and more.

---

### ❌ Wrong Statement

> Kernel and User Programs have the same privileges.

**False**

Kernel Mode has full privileges.

User Mode has limited privileges.

---

### ✅ Correct Statement

> System Calls protect the operating system by restricting direct hardware access.

---

## Common MCQs

### Q1

Why are System Calls required?

A. To increase CPU speed

B. To allow safe access to kernel services

C. To reduce RAM usage

D. To improve cache performance

✅ **Answer:** **B**

---

### Q2

Which component has direct access to hardware?

A. User Program

B. Compiler

C. Kernel

D. Loader

✅ **Answer:** **C**

---

### Q3

System Calls mainly provide

A. Direct hardware access to applications

B. Controlled access to Operating System services

C. Faster execution of programs

D. Process scheduling only

✅ **Answer:** **B**

---

# Formula Corner 🧮

There are **no mathematical formulas**.

Remember the concept:

```text
User Program
      │
Restricted Access
      │
System Call
      │
Kernel
      │
Hardware
```

---

# One-Line Revision

> **System Calls exist to safely bridge the gap between user programs and the operating system kernel, allowing controlled access to privileged resources.**