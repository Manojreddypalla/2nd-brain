# Linux Unit 1 Revision Notes (Exam-Oriented)

Based on your chapter **"The Big Picture"** from _How Linux Works_.

---

# 1. What is Abstraction?

### Definition

Abstraction means **hiding unnecessary details** and focusing only on the important functionality.

### Example

When driving a car:

- You know how to steer and accelerate.
    
- You don't need to know how the engine bolts are attached.
    

### In Linux

Instead of understanding every internal detail:

- Focus on what a component does.
    
- Ignore how it is implemented internally.
    

### Benefits

✅ Easier understanding

✅ Easier troubleshooting

✅ Easier software development

---

# 2. Layers of a Linux System

Linux is divided into **3 major layers**:

```text
+----------------------+
| User Space           |
| (Applications)       |
+----------------------+
| Kernel               |
+----------------------+
| Hardware             |
+----------------------+
```

---

## Hardware Layer

Physical components:

- CPU
    
- RAM
    
- Hard Disk
    
- Network Cards
    

### Function

Provides actual computing resources.

---

## Kernel Layer

### Definition

The **core of the operating system**.

Acts as a bridge between:

```text
Applications <-> Hardware
```

### Responsibilities

1. Process Management
    
2. Memory Management
    
3. Device Management
    
4. System Calls
    

---

## User Space

Contains running programs:

Examples:

- Chrome
    
- Firefox
    
- Shell
    
- Web Servers
    
- Database Servers
    

These are called **Processes**.

---

# 3. Kernel Mode vs User Mode

## Kernel Mode

### Characteristics

- Full hardware access
    
- Full memory access
    
- Most powerful mode
    

### Risk

A mistake can crash the entire system.

### Memory Area

```text
Kernel Space
```

Only kernel can access it.

---

## User Mode

### Characteristics

- Limited permissions
    
- Restricted memory access
    
- Safer execution
    

### Memory Area

```text
User Space
```

---

## Comparison

|Feature|Kernel Mode|User Mode|
|---|---|---|
|Hardware Access|Full|Limited|
|Memory Access|Full|Restricted|
|Can Crash Entire System|Yes|Usually No|
|Used By|Kernel|Applications|

---

# 4. Main Memory (RAM)

### Definition

RAM stores:

- Kernel
    
- Processes
    
- Data
    
- Instructions
    

Everything currently running exists in RAM.

---

## Bit

Smallest storage unit.

```text
0 or 1
```

---

## State

A particular arrangement of bits.

Example:

```text
0110
0001
1011
```

Different bit arrangements = different states.

---

## Image

Physical arrangement of bits in memory.

---

# 5. Kernel Responsibilities

Kernel manages four major areas:

---

## A. Process Management

Kernel decides:

- Which process runs
    
- When it runs
    
- How long it runs
    

---

## B. Memory Management

Kernel tracks:

- Used memory
    
- Free memory
    
- Shared memory
    

---

## C. Device Drivers

Kernel communicates with hardware:

Examples:

- Keyboard
    
- Disk
    
- Network Card
    

---

## D. System Calls

Processes use system calls to request kernel services.

Examples:

- open()
    
- read()
    
- write()
    

---

# 6. Process Management

## What is a Process?

A program currently running in memory.

Examples:

```bash
firefox
chrome
ls
```

---

## Single CPU Reality

Although many programs appear to run simultaneously:

```text
CPU
 |
 +--> Process A
 +--> Process B
 +--> Process C
```

Only **one process runs at a time** on a single-core CPU.

---

## Time Slice

Small CPU time assigned to a process.

```text
A -> B -> C -> A -> B
```

Very fast switching creates the illusion of multitasking.

---

## Multitasking

Running multiple processes seemingly at the same time.

---

## Context Switch

### Definition

Switching CPU control from one process to another.

---

### Steps

1. CPU interrupts current process
    
2. Switches to kernel mode
    
3. Kernel saves process state
    
4. Kernel selects next process
    
5. Kernel loads next process state
    
6. CPU switches back to user mode
    
7. New process runs
    

---

### Exam Definition

**Context Switch = Saving current process state and loading another process state.**

---

# 7. Memory Management

Kernel ensures:

### Rule 1

Kernel memory remains protected.

### Rule 2

Each process gets its own memory.

### Rule 3

Processes cannot access each other's memory.

### Rule 4

Processes can share memory when allowed.

### Rule 5

Read-only memory regions can exist.

### Rule 6

Disk can act as extra memory.

---

# 8. Virtual Memory

### Problem

Processes shouldn't directly access physical RAM.

---

### Solution

Virtual Memory

Each process thinks:

```text
"I own the entire machine."
```

---

## Memory Management Unit (MMU)

Hardware component inside CPU.

### Function

Converts:

```text
Virtual Address
      ↓
Physical Address
```

---

## Page Table

Data structure used for address translation.

### Important Exam Point

```text
MMU uses Page Table
```

to map virtual memory to physical memory.

---

# 9. Device Drivers

### Definition

Software that controls hardware devices.

Examples:

- Printer Driver
    
- Disk Driver
    
- Network Driver
    

---

### Why Needed?

Different hardware uses different interfaces.

Drivers provide a common interface to programs.

---

# 10. System Calls

### Definition

Mechanism used by user processes to communicate with kernel.

---

### Examples

```c
open()
read()
write()
```

---

## Important Process Creation Calls

### fork()

Creates a copy of current process.

```text
Process A
    |
  fork()
    |
Process A + Copy
```

---

### exec()

Replaces current process with a new program.

```text
exec(ls)
```

Current process becomes:

```text
ls
```

---

## Program Execution Flow

When user runs:

```bash
ls
```

Shell performs:

```c
fork()
exec(ls)
```

Flow:

```text
Shell
   |
 fork()
   |
Shell Copy
   |
 exec(ls)
   |
 ls
```

---

# 11. Pseudodevices

### Definition

Devices implemented completely in software.

Appear like real devices.

Example:

```text
/dev/random
```

Provides random numbers.

---

# 12. User Space Structure

Three rough levels:

```text
Applications
     ↑
Utility Services
     ↑
Basic Services
```

---

## Applications

Directly used by users.

Examples:

- Browser
    
- GUI
    
- Office Apps
    

---

## Utility Services

Examples:

- Database
    
- Mail Service
    
- Print Service
    

---

## Basic Services

Examples:

- Logging
    
- Network Configuration
    
- Communication Services
    

---

# 13. Users in Linux

### Definition

Entity that:

- Runs processes
    
- Owns files
    

---

## User ID (UID)

Kernel identifies users using numbers.

Example:

```text
1000
1001
0
```

---

## Username

Human-friendly name.

Example:

```text
manoj
john
root
```

---

# 14. Root User

### Superuser

Special user with unrestricted access.

Can:

✅ Access all files

✅ Kill any process

✅ Modify system settings

---

### UID

```text
0
```

---

### Important Point

Root runs in:

```text
User Mode
```

NOT Kernel Mode.

---

# 15. Groups

### Definition

Collection of users.

Purpose:

```text
Shared Permissions
```

Example:

```text
developers
admins
students
```

Members can share file access.

---

# Quick 2-Mark Questions

### What is abstraction?

Hiding implementation details and exposing only essential functionality.

### What is kernel?

Core part of OS that manages hardware, memory, processes, and system calls.

### What is context switching?

Saving one process state and loading another process state.

### What is virtual memory?

Technique that gives each process its own virtual address space.

### What is MMU?

Memory Management Unit that converts virtual addresses to physical addresses.

### What is a system call?

Interface between user process and kernel.

### Difference between fork() and exec()?

- fork() → creates a copy of process.
    
- exec() → replaces process with new program.
    

### What is a device driver?

Software that controls hardware devices.

### Who is root?

Superuser with UID 0.

---

# One-Page Memory Map

```text
Linux System
│
├── Hardware
│   ├── CPU
│   ├── RAM
│   ├── Disk
│   └── Network
│
├── Kernel
│   ├── Process Management
│   ├── Memory Management
│   ├── Device Drivers
│   └── System Calls
│
└── User Space
    ├── Applications
    ├── Services
    └── Shell

Kernel Mode
    ↓
Full Access

User Mode
    ↓
Restricted Access

Process Creation
fork() → Copy
exec() → Replace

Memory
Virtual Memory
    ↓
MMU
    ↓
Page Table
    ↓
Physical Memory

Root User
UID = 0
Superuser
```

These notes cover almost every exam-worthy concept from Unit 1 in a compact revision format.