# Operating System Responsibilities (Advanced GATE Notes)

> **Core Idea:** The Operating System (OS) is a **Resource Manager** and a **Control Program**.
> 
> It sits between **Applications** and **Hardware**, deciding **who gets what resource, when, and for how long.**

```
               User
                 │
         User Applications
                 │
      -------------------------
      |   Operating System    |
      -------------------------
      CPU   Memory   Disk   I/O
       │      │       │      │
    Hardware Components
```

---

# 1. Who loads programs into RAM?

## Answer

**Loader + Memory Manager (Operating System)**

### What actually happens?

Suppose you double-click Chrome.

```
Chrome.exe (SSD)
        │
        ▼
Operating System
        │
Creates Process
        │
Allocates Virtual Memory
        │
Loads executable sections
        │
Maps libraries (.dll/.so)
        │
Creates Stack
        │
Creates Heap
        │
Starts Main Thread
        ▼
CPU begins execution
```

### Components involved

- Loader
    
- Process Manager
    
- Memory Manager
    
- Virtual Memory System
    

### Important concepts

- Executable (ELF in Linux, PE in Windows)
    
- Address Space
    
- Stack
    
- Heap
    
- Code Segment
    
- Data Segment
    
- Demand Paging
    
- Page Tables
    

### GATE Point

Loading a program **does NOT necessarily mean loading the entire executable into RAM.**

Modern OSes use

- Demand Paging
    
- Lazy Loading
    

Only required pages are loaded initially.

---

# 2. Who decides which program gets CPU time?

## Answer

**CPU Scheduler**

One of the most important OS components.

Imagine

```
CPU
 │
 ├── Chrome
 ├── VS Code
 ├── Spotify
 ├── Antivirus
 └── Terminal
```

The CPU can execute **only one instruction per core at a time.**

The scheduler decides

- Who runs next?
    
- For how long?
    
- Who waits?
    
- Who gets interrupted?
    

---

## Components

### Long-Term Scheduler

Controls admission of processes.

```
Disk
 │
 ▼
Ready Queue
```

---

### Short-Term Scheduler

Runs every few milliseconds.

Chooses

```
Ready Queue

P1
P2
P3
P4

↓

CPU
```

---

### Dispatcher

Actually performs

- Context Switch
    
- Switch to User Mode
    
- Jump to program
    

---

## Algorithms

- FCFS
    
- SJF
    
- SRTF
    
- Round Robin
    
- Priority Scheduling
    
- Multilevel Queue
    
- Multilevel Feedback Queue
    

---

## GATE Fact

Scheduler chooses.

Dispatcher performs.

---

# 3. Who saves files?

## Answer

**File System**

The OS manages persistent storage.

Applications never directly manipulate disk sectors.

Instead,

```
Application

write()

↓

System Call

↓

File System

↓

Storage Driver

↓

SSD/HDD
```

---

## Responsibilities

Create

Delete

Rename

Move

Copy

Permissions

Ownership

Directory hierarchy

Metadata

---

## Internal Structures

In Linux

```
Inode

contains

File size

Permissions

Owner

Pointers to blocks

Timestamp
```

Directories simply map

```
Filename

↓

Inode Number
```

---

## File System Examples

Linux

- ext4
    
- XFS
    
- Btrfs
    

Windows

- NTFS
    
- FAT32
    
- exFAT
    

---

# 4. Who talks to the keyboard?

## Answer

**Device Driver**

Applications never communicate directly with hardware.

```
Keyboard

↓

Keyboard Controller

↓

Interrupt

↓

Kernel

↓

Keyboard Driver

↓

Input Buffer

↓

Application
```

---

## Sequence

Key Press

↓

Interrupt Generated

↓

CPU switches to Kernel Mode

↓

ISR Executes

↓

Keyboard Driver Reads Data

↓

OS stores event

↓

Application receives key

---

### Important Terms

Interrupt

ISR

Device Driver

Input Buffer

Interrupt Controller

Polling

---

### GATE

Keyboard uses

Interrupt-driven I/O

not continuous polling.

---

# 5. Who displays graphics?

## Answer

Graphics Subsystem + GPU Driver

---

Application

```
Draw Window

↓

GUI API

↓

System Call

↓

Graphics Driver

↓

GPU

↓

Frame Buffer

↓

Monitor
```

---

## Components

Window Manager

Compositor

Graphics Driver

GPU

Frame Buffer

Display Controller

---

## Linux

X11

Wayland

DRM

Mesa

---

## Windows

Win32

DirectX

WDDM

---

### GATE

Graphics rendering is accelerated using GPUs.

---

# 6. Who manages memory?

## Answer

Memory Manager

One of the largest OS components.

---

Responsibilities

Allocate RAM

Free RAM

Protect Memory

Share Memory

Virtual Memory

Paging

Swapping

Memory Mapping

---

Example

```
RAM

+----------------+

OS

Chrome

VS Code

Spotify

Kernel

Free Memory

+----------------+
```

---

Every process thinks it owns memory

```
0x00000000

↓

4 GB

(or larger)

```

Actually,

OS translates

Virtual Address

↓

Physical Address

using

Page Tables

---

## Concepts

Virtual Memory

Physical Memory

Page

Frame

TLB

Page Fault

Demand Paging

Swapping

Memory Protection

---

## GATE

Memory Manager prevents one process from accessing another's memory.

---

# 7. Who handles multiple programs?

## Answer

Process Manager + Scheduler + Memory Manager

This is called

## Multitasking

Example

```
Chrome

VS Code

Spotify

Terminal

Discord

```

appear to run simultaneously.

Actually

```
CPU

Chrome

↓

VS Code

↓

Spotify

↓

Chrome

↓

Discord

↓

Terminal

```

This happens thousands of times per second.

---

## OS Responsibilities

Create Process

Terminate Process

Suspend

Resume

Synchronize

Communicate

Schedule

Context Switch

---

## Components

Process Control Block (PCB)

Ready Queue

Waiting Queue

Scheduler

Dispatcher

Context Switch

---

# The Big Picture

```
                     Operating System

                           │

       ┌───────────────────┼────────────────────┐

       │                   │                    │

 CPU Management      Memory Management     File Management

 Scheduler           Paging                File System

 Dispatcher          Virtual Memory        Inodes

 Context Switch      Swapping              Directories

       │                   │                    │

       ├───────────────────┼────────────────────┤

       │                   │                    │

 Device Management   Process Management   Security

 Drivers             PCB                  Protection

 Interrupts          Threads              Permissions

 DMA                 Scheduling           Isolation

```

# GATE Corner ⭐

### Resource ↔ OS Component

|Resource|OS Component|
|---|---|
|CPU|CPU Scheduler + Dispatcher|
|RAM|Memory Manager|
|Disk Files|File System|
|Keyboard|Device Driver + Interrupt Handler|
|Mouse|Device Driver|
|Printer|Device Driver + Spooler|
|Network|Network Stack + Device Driver|
|GPU|Graphics Subsystem + GPU Driver|
|Multiple Programs|Process Manager + Scheduler|
|Process Creation|Process Manager|
|Memory Protection|MMU + Memory Manager|
|I/O Devices|Device Management|

---

## One-Liner to Remember

> **The Operating System is a collection of specialized managers—Process Manager, CPU Scheduler, Memory Manager, File System, and Device Drivers—that cooperate to abstract hardware, allocate resources efficiently, enforce protection, and provide a convenient execution environment for applications.**

This understanding is foundational for the rest of OS topics you'll study (system calls → processes → scheduling → memory → files → I/O), because each module expands on one of these responsibilities.