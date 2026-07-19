# Module 1.7 — Information Maintenance System Calls

## Definition

> **Information Maintenance System Calls** are system calls used to **retrieve or modify information about processes, the operating system, users, time, and system resources.**

They allow programs to obtain system-related information from the kernel.

---

# Why Do We Need Information Maintenance?

A user program **cannot directly access kernel information**.

Instead, it requests the Operating System using **Information Maintenance System Calls**.

These system calls are commonly used to:

- Get Process ID (PID)
- Get Parent Process ID (PPID)
- Get User ID (UID)
- Get System Time
- Get System Information

---

# Common Information Maintenance System Calls

| System Call | Purpose |
|-------------|---------|
| `getpid()` | Returns Process ID (PID) |
| `getppid()` | Returns Parent Process ID (PPID) |
| `getuid()` | Returns User ID |
| `getgid()` | Returns Group ID |
| `time()` | Returns current system time |
| `uname()` | Returns operating system information |

---

# How It Works

### Step 1

The program requests information.

Example:

```c
getpid();
```

↓

### Step 2

The CPU switches to **Kernel Mode**.

↓

### Step 3

The kernel retrieves the requested information.

↓

### Step 4

The information is returned to the user program.

---

# Example

```text
Program
     │
getpid()
     │
Kernel
     │
Returns PID = 2451
```

---

# Another Example

```text
Program
      │
time()
      │
Kernel
      │
Returns Current Time
```

---

# Why Are These System Calls Useful?

Applications use them to:

- Identify themselves (PID)
- Identify parent processes (PPID)
- Check user permissions
- Display date and time
- Retrieve OS information

---

# Key Points

- Used to retrieve or modify system information.
- Execute in **Kernel Mode**.
- Return information from the kernel to user programs.
- Do not directly manipulate files or devices.

---

# Relationship

```text
Program

↓

Information System Call

↓

Kernel

↓

Returns Information

↓

Program
```

---

# 🎯 GATE Corner

## Must Remember ⭐⭐⭐⭐⭐

- Information Maintenance System Calls retrieve system information.
- `getpid()` returns the Process ID.
- `getppid()` returns the Parent Process ID.
- `time()` returns the current system time.
- `uname()` returns operating system information.
- **Every Information Maintenance System Call causes a Mode Switch.**

---

## GATE Tricks ⚠️

### ❌ Wrong Statement

> `getpid()` creates a process.

**False**

It only returns the Process ID.

---

### ❌ Wrong Statement

> `time()` changes the system clock.

**False**

It retrieves the current system time.

---

### ❌ Wrong Statement

> Information Maintenance System Calls execute in User Mode.

**False**

They execute in **Kernel Mode**.

---

### ✅ Correct Statement

> Information Maintenance System Calls obtain information maintained by the Operating System.

---

## Common MCQs

### Q1

Which System Call returns the Process ID?

A. `fork()`

B. `exec()`

C. `getpid()`

D. `wait()`

✅ **Answer:** **C**

---

### Q2

Which System Call returns the Parent Process ID?

A. `getppid()`

B. `getpid()`

C. `exit()`

D. `kill()`

✅ **Answer:** **A**

---

### Q3

Which System Call is used to obtain the current system time?

A. `clock()`

B. `time()`

C. `sleep()`

D. `timer()`

✅ **Answer:** **B**

---

### Q4

Every Information Maintenance System Call necessarily causes

A. Context Switch

B. Mode Switch

C. Process Creation

D. Scheduling

✅ **Answer:** **B**

---

# Formula Corner 🧮

There are **no mathematical formulas**.

Remember the flow:

```text
Information Request

↓

System Call

↓

Kernel

↓

Return Information
```

---

# One-Line Revision

> **Information Maintenance System Calls retrieve process, user, time, and operating system information from the kernel, and every such system call executes in Kernel Mode, causing a Mode Switch.**