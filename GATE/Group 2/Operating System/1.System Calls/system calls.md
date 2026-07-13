# Module 1 — System Calls

## 🎯 Goal
Understand **how user programs communicate with the kernel**.

---

# Big Picture

Every program you run (Chrome, VS Code, Games, etc.) executes in **User Mode** and **cannot directly access hardware**.

Instead, it requests the operating system (kernel) to perform privileged operations.

This request is called a **System Call**.

```text
                You
                 │
                 ▼
          User Program
                 │
          System Call
                 │
                 ▼
        Linux Kernel
                 │
                 ▼
 CPU • RAM • SSD • Network • GPU
```

> **Definition:** A **System Call** is a controlled interface through which a user program requests services from the operating system kernel.

---

# Why Do We Need System Calls?

Imagine if every application could directly control hardware.

Problems:

- Malware could erase your SSD.
- Any application could read another program's memory.
- Programs could crash the entire system.
- Security would not exist.

To prevent this, modern operating systems isolate applications from hardware.

Only the **kernel** can directly access hardware.

Applications must ask the kernel using **system calls**.

---

# Operating System Layers

```text
+-------------------------+
| Applications            |
| Chrome                  |
| VS Code                 |
| Games                   |
+-------------------------+
| C Library (glibc)       |
+-------------------------+
| System Calls            |
+-------------------------+
| Linux Kernel            |
+-------------------------+
| Hardware                |
+-------------------------+
```

Applications never communicate with hardware directly.

---

# User Mode vs Kernel Mode

Modern CPUs have different privilege levels.

For GATE, only remember these two.

## User Mode

Applications execute here.

Allowed:

- Calculations
- Variables
- Loops
- Function calls

Not Allowed:

- Direct disk access
- Memory management
- Device control
- Process scheduling

---

## Kernel Mode

The operating system executes here.

Has permission to:

- Access hardware
- Manage memory
- Schedule processes
- Handle interrupts
- Access devices
- Execute privileged CPU instructions

---

## Visual Representation

```text
            USER MODE

+---------------------------+
| Chrome                    |
| VS Code                   |
| Game                      |
+---------------------------+

        System Call

=============================

          KERNEL MODE

+---------------------------+
| Process Scheduler         |
| Memory Manager            |
| File System               |
| Device Drivers            |
| Network Stack             |
+---------------------------+
```

---

# Why Can't Applications Access Hardware?

Suppose a program wants to read a file.

Without the kernel it would need to know:

- SSD Controller
- SATA/NVMe Protocol
- Filesystem Layout
- Buffer Cache
- Memory Mapping
- Permissions
- Device Drivers

That is impossible for every application.

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

The kernel hides all this complexity.

This is called **Abstraction**.

---

# How a System Call Works

Example:

```c
write(fd, "Hello", 5);
```

Internally:

```text
Program

↓

glibc Wrapper

↓

CPU executes "syscall" instruction

↓

CPU switches to Kernel Mode

↓

Kernel validates request

↓

Kernel performs operation

↓

Kernel returns result

↓

CPU switches back to User Mode

↓

Program continues
```

---

# Step-by-Step Execution

Suppose we execute:

```c
char buffer[100];

read(fd, buffer, 100);
```

---

## Step 1

The program calls

```c
read()
```

---

## Step 2

The C Library (glibc) prepares arguments.

---

## Step 3

CPU executes

```text
syscall
```

This instruction tells the CPU:

> Enter Kernel Mode.

---

## Step 4

CPU switches

```text
User Mode
      ↓
Kernel Mode
```

This is called a **Mode Switch** (Privilege Transition).

---

## Step 5

Kernel receives

- File Descriptor
- Buffer Address
- Number of Bytes

Example:

```text
fd = 3

buffer = 0x7ffd...

count = 100
```

---

## Step 6

Kernel validates everything.

Checks:

- Is file descriptor valid?
- Does user have permission?
- Is memory accessible?
- Is file open?

If anything is invalid:

```text
Return Error
```

---

## Step 7

Kernel performs the operation.

Example:

Reads data from disk into memory.

---

## Step 8

Kernel returns result.

CPU switches back

```text
Kernel Mode
      ↓
User Mode
```

Program continues after the `read()` call.

---

# Why Are System Calls Slow?

Normal function call:

```text
Function

↓

Execute

↓

Return
```

System call:

```text
Save CPU Registers

↓

Switch to Kernel Mode

↓

Validate Parameters

↓

Access Hardware

↓

Restore Registers

↓

Return to User Mode
```

Extra work makes system calls much slower than ordinary function calls.

---

# Types of System Calls

## 1. Process Control

Used to create and manage processes.

Examples:

- fork()
- exec()
- wait()
- exit()

Visual:

```text
Parent

 │

fork()

 │

 ├── Parent

 └── Child
```

---

## 2. File Management

Used to work with files.

Typical flow:

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

## 3. Device Management

Used to interact with hardware devices.

Examples:

- Printer
- Keyboard
- Mouse
- Disk
- Camera

Typical Calls:

- open()
- close()
- ioctl()

---

## 4. Information Maintenance

Retrieve system information.

Examples:

- getpid()
- getuid()
- time()

---

## 5. Communication

Used for communication between processes.

Examples:

- pipe()
- socket()
- send()
- recv()
- shmget()

Used in:

- Networking
- IPC
- Distributed Systems

---

# Real Linux Example

Command:

```bash
cat notes.txt
```

Internally:

```text
open("notes.txt")

↓

read()

↓

write(STDOUT)

↓

close()
```

Almost every Linux command eventually performs system calls.

---

# Mental Model

Whenever a program needs something it **cannot safely do itself**, it asks the kernel.

```text
Need arithmetic?

✔ Do it yourself.

Need file?

✔ System Call

Need memory from OS?

✔ System Call

Need network?

✔ System Call

Need new process?

✔ System Call
```

---

# Connections

Understanding System Calls helps in:

- Operating Systems
- Linux
- Docker
- Containers
- Networking
- Cyber Security
- Device Drivers
- Memory Management
- File Systems

Everything eventually reaches the kernel through system calls.

---

# Key Terms

| Term | Meaning |
|------|----------|
| User Mode | Restricted execution mode for applications |
| Kernel Mode | Privileged execution mode for the operating system |
| System Call | Request made by a program to the kernel |
| Mode Switch | CPU switches between User Mode and Kernel Mode |
| Privileged Instruction | Instruction executable only in Kernel Mode |
| glibc | GNU C Library that provides wrappers around system calls |

---

# GATE Revision

✅ System Call = Interface between User Program and Kernel

✅ Applications run in User Mode

✅ Operating System runs in Kernel Mode

✅ System Calls trigger Mode Switching

✅ Kernel performs privileged operations

Categories:

- Process Control
- File Management
- Device Management
- Information Maintenance
- Communication

Examples:

- fork()
- exec()
- wait()
- open()
- read()
- write()
- close()

Remember:

> **User programs never access hardware directly; they always go through the kernel using system calls.**