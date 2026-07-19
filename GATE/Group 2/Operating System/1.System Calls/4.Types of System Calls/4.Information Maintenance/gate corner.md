# 🎯 GATE Corner — Information Maintenance System Calls

## Weightage

⭐⭐⭐☆☆ **Moderately Important**

Frequently asked with:

- System Calls
- Process Information
- User Information
- Operating System Information
- Time Management

---

# Must Remember ⭐⭐⭐⭐⭐

Information Maintenance System Calls are used to

- Get Process Information
- Get User Information
- Get System Information
- Get Time Information

---

## Important System Calls

| System Call | Function |
|-------------|----------|
| `getpid()` | Returns Process ID (PID) |
| `getppid()` | Returns Parent Process ID (PPID) |
| `getuid()` | Returns User ID (UID) |
| `getgid()` | Returns Group ID (GID) |
| `time()` | Returns current system time |
| `uname()` | Returns operating system information |

---

# Remember This Concept ⭐⭐⭐⭐⭐

```text
Program

↓

Requests Information

↓

Kernel

↓

Returns Information
```

Unlike Process Control or File Management,

these system calls **only retrieve information**.

---

# Most Asked Concepts

## `getpid()`

- Returns the **Process ID (PID)**
- Does **not** create a process

---

## `getppid()`

- Returns the **Parent Process ID (PPID)**

---

## `time()`

- Returns the current system time
- Does **not** modify the clock

---

## `uname()`

Returns information about the operating system, such as:

- OS Name
- Release Version
- Machine Architecture

---

# ⭐ IMPORTANT GATE FACT

## Every Information Maintenance System Call causes a

```text
Mode Switch
```

because they execute inside the **Kernel**.

Examples:

- `getpid()` ✅ Mode Switch
- `getppid()` ✅ Mode Switch
- `getuid()` ✅ Mode Switch
- `time()` ✅ Mode Switch
- `uname()` ✅ Mode Switch

---

# GATE Tricks ⚠️

### ❌ Wrong Statement

> `getpid()` creates a process.

**False**

It only returns the current Process ID.

---

### ❌ Wrong Statement

> `getppid()` returns the current process ID.

**False**

It returns the **Parent Process ID**.

---

### ❌ Wrong Statement

> `time()` changes the system time.

**False**

It retrieves the current system time.

---

### ❌ Wrong Statement

> Information Maintenance System Calls execute in User Mode.

**False**

They execute in **Kernel Mode**, so every one causes a **Mode Switch**.

---

### ✅ Correct Statement

> Information Maintenance System Calls retrieve information maintained by the Operating System.

---

# Common MCQs

### Q1

Which System Call returns the current Process ID?

A. `fork()`

B. `getpid()`

C. `wait()`

D. `exec()`

✅ **Answer:** **B**

---

### Q2

Which System Call returns the Parent Process ID?

A. `getuid()`

B. `getppid()`

C. `getgid()`

D. `kill()`

✅ **Answer:** **B**

---

### Q3

Which System Call returns the current system time?

A. `clock()`

B. `sleep()`

C. `time()`

D. `alarm()`

✅ **Answer:** **C**

---

### Q4

Every Information Maintenance System Call necessarily causes

A. Context Switch

B. Mode Switch

C. Process Creation

D. Scheduling

✅ **Answer:** **B**

---

# PYQ Keywords

Whenever you see these words, think of **Information Maintenance**:

- PID
- PPID
- UID
- GID
- System Time
- OS Information
- `getpid()`
- `getppid()`
- `time()`
- `uname()`
- Mode Switch

---

# Memory Trick 🧠

```text
Need Information

↓

Ask Kernel

↓

Kernel Returns

↓

Continue
```

---

# 15-Second Revision 🚀

```text
getpid()  → Process ID

getppid() → Parent PID

getuid()  → User ID

time()    → Current Time

uname()   → OS Information

↓

Kernel

↓

Mode Switch ✔
```

---

# Golden Rule ⭐

> **Information Maintenance System Calls retrieve information maintained by the Operating System. Since they execute in Kernel Mode, every one of them always causes a Mode Switch.**