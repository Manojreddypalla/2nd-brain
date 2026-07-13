# Module 1 — System Calls (Quick Revision Notes)

## Definition
A **System Call** is the interface between a **user program** and the **operating system kernel**. It allows applications to request services that require privileged access.

---

# Why System Calls?

Applications cannot directly access:
- Hardware
- Disk
- RAM management
- Network devices
- CPU privileged instructions

Reasons:
- Security
- Protection
- Resource Management
- Hardware Abstraction

Instead:

```text
Application
      │
      ▼
 System Call
      │
      ▼
    Kernel
      │
      ▼
   Hardware
```

---

# User Mode vs Kernel Mode

| User Mode | Kernel Mode |
|------------|-------------|
| Applications run here | Operating System runs here |
| Limited privileges | Full privileges |
| Cannot access hardware directly | Can access all hardware |
| Safer | Privileged |

**System Call = Mode Switch**

```text
User Mode
     ↓
Kernel Mode
     ↓
User Mode
```

---

# System Call Flow

```text
User Program
      │
glibc Wrapper
      │
syscall Instruction
      │
Kernel Mode
      │
Kernel executes request
      │
Return value
      │
Back to User Mode
```

---

# Why Are System Calls Slow?

Because they require:
- Mode switching
- Saving CPU registers
- Parameter validation
- Security checks
- Hardware access

> System Calls are **much slower** than normal function calls.

---

# Types of System Calls

## 1. Process Control
Manage processes.

Examples:
- `fork()`
- `exec()`
- `wait()`
- `exit()`

---

## 2. File Management
Work with files.

Examples:
- `open()`
- `read()`
- `write()`
- `close()`

---

## 3. Device Management
Access hardware devices.

Examples:
- `open()`
- `close()`
- `ioctl()`

---

## 4. Information Maintenance
Get system information.

Examples:
- `getpid()`
- `getuid()`
- `time()`

---

## 5. Communication
Inter-Process Communication (IPC) and networking.

Examples:
- `pipe()`
- `socket()`
- `send()`
- `recv()`
- `shmget()`

---

# Typical File Operation

```text
open()
   ↓
read()
   ↓
write()
   ↓
close()
```

---

# Typical Process Creation

```text
Parent
   │
 fork()
   │
 ├── Parent
 └── Child

Child
   │
 exec()
   │
 New Program

Parent
   │
 wait()
```

---

# Important Terms

| Term | Meaning |
|------|----------|
| User Mode | Restricted execution mode |
| Kernel Mode | Privileged execution mode |
| System Call | Interface to the kernel |
| Mode Switch | CPU changes between User & Kernel Mode |
| glibc | C library providing system call wrappers |
| Privileged Instruction | CPU instruction executable only in Kernel Mode |

---

# Common Linux System Calls

| System Call | Purpose |
|-------------|---------|
| `fork()` | Create child process |
| `exec()` | Replace current process with a new program |
| `wait()` | Wait for child process |
| `open()` | Open a file |
| `read()` | Read from a file/device |
| `write()` | Write to a file/device |
| `close()` | Close a file |

---

# Real Example

Command:

```bash
cat notes.txt
```

Internally:

```text
open()
   ↓
read()
   ↓
write()
   ↓
close()
```

---

# Key Points for GATE

- System Call = Interface between **User Program** and **Kernel**.
- Applications execute in **User Mode**.
- Kernel executes in **Kernel Mode**.
- A system call causes a **Mode Switch**.
- The kernel performs **privileged operations**.
- System calls provide **security**, **protection**, and **hardware abstraction**.
- System calls are **slower than normal function calls** because of mode switching and validation.
- Applications usually invoke system calls through **glibc** (library wrappers), not by directly executing the `syscall` instruction.

---

# One-Minute Memory Map

```text
Need Hardware?
      │
      ▼
System Call
      │
      ▼
Kernel
      │
      ▼
Hardware

Categories
│
├── Process → fork(), exec(), wait()
├── File → open(), read(), write(), close()
├── Device → ioctl()
├── Information → getpid(), getuid()
└── Communication → pipe(), socket(), send(), recv()

User Mode
      ↓
System Call
      ↓
Kernel Mode
      ↓
Return
      ↓
User Mode
```