# 📘 Linux Internals – Day 14: System Calls (Deep Dive)

---

# What is a System Call?

A **System Call (Syscall)** is the interface between **User Space** and **Kernel Space**. It allows an application to request services from the Linux kernel.

> **Definition:** A system call is a controlled mechanism through which a user-space program requests the kernel to perform privileged operations.

---

# Why Do System Calls Exist?

Applications **cannot directly**:

- Read or write disks
    
- Access hardware devices
    
- Allocate kernel memory
    
- Create processes
    
- Send network packets
    
- Modify protected system resources
    

These operations require **kernel privileges**.

Instead, applications request the kernel through **system calls**.

```text
Application
      │
      ▼
 System Call
      │
      ▼
 Linux Kernel
      │
      ▼
 Hardware
```

> **Memory Tip:** Think of the kernel as the **service desk** of the operating system.

---

# User Mode vs Kernel Mode

## User Mode

- Applications execute here.
    
- Limited privileges.
    
- Cannot access hardware directly.
    
- Cannot execute privileged CPU instructions.
    

Examples:

- Chrome
    
- VS Code
    
- Firefox
    
- Your C/C++ program
    

---

## Kernel Mode

- Linux kernel executes here.
    
- Full hardware access.
    
- Manages CPU, memory, filesystem, networking, and devices.
    

Examples:

- Scheduler
    
- Memory Manager
    
- Device Drivers
    
- Filesystem
    
- Network Stack
    

---

# Why Can't Applications Access Hardware Directly?

Without protection:

- Any application could overwrite RAM.
    
- Any program could format your SSD.
    
- Malware could control hardware directly.
    

Modern operating systems isolate applications from hardware for **security and stability**.

---

# System Call Journey

Example:

```c
read(fd, buffer, 100);
```

Internally:

```text
User Program
      │
      ▼
read()
      │
      ▼
glibc (C Library)
      │
      ▼
syscall instruction
═══════════════════════════════
 CPU switches to Kernel Mode
═══════════════════════════════
      │
Kernel validates arguments
      │
Filesystem Driver
      │
Disk
      │
Copy data to user buffer
      │
Return value
═══════════════════════════════
 CPU switches to User Mode
═══════════════════════════════
      │
Program continues
```

---

# Mode Switching

Executing a system call causes the CPU to switch:

```text
User Mode
      │
      ▼
syscall instruction
      │
      ▼
Kernel Mode
      │
      ▼
Kernel executes request
      │
      ▼
Return to User Mode
```

This transition is called a **Mode Switch**.

---

# Steps During a System Call

1. Application calls a library function.
    
2. Library prepares arguments.
    
3. CPU executes the `syscall` instruction.
    
4. CPU switches to Kernel Mode.
    
5. Kernel validates the request.
    
6. Kernel performs the operation.
    
7. Kernel returns a result.
    
8. CPU switches back to User Mode.
    

---

# Common System Calls

|System Call|Purpose|
|---|---|
|`open()`|Open a file|
|`read()`|Read data|
|`write()`|Write data|
|`close()`|Close a file|
|`fork()`|Create a child process|
|`execve()`|Execute a new program|
|`wait()`|Wait for a child process|
|`mmap()`|Map memory|
|`socket()`|Create a socket|
|`connect()`|Connect to a remote host|
|`exit()`|Terminate a process|

---

# Library Function vs System Call

## Library Function

Runs entirely in **User Space**.

Examples:

- `printf()`
    
- `strlen()`
    
- `memcpy()`
    
- `sqrt()`
    

No direct hardware access.

---

## System Call

Runs in **Kernel Space**.

Examples:

- `read()`
    
- `write()`
    
- `fork()`
    
- `execve()`
    
- `socket()`
    

Requires a mode switch.

---

# printf() vs write()

```c
printf("Hello");
```

Actually becomes:

```text
printf()
      │
Format text
      │
      ▼
write()
      │
      ▼
System Call
      │
      ▼
Kernel
```

### Important Points

- `printf()` is **not** a system call.
    
- `printf()` is a **library function**.
    
- `printf()` usually uses `write()` internally.
    
- Printing to the terminal eventually requires a system call.
    

---

# Does Every Library Function Need a System Call?

No.

Example:

```c
strlen("Linux");
```

```text
Memory
   │
Count Characters
   │
Return Length
```

Everything happens in User Space.

No kernel interaction.

---

# When Do We Need System Calls?

System calls are required whenever a program needs the operating system.

Examples:

|Operation|Needs System Call?|
|---|---|
|Read a file|✅ Yes|
|Write to terminal|✅ Yes|
|Create process|✅ Yes|
|Allocate mapped memory|✅ Yes|
|Create socket|✅ Yes|
|Calculate `strlen()`|❌ No|
|Perform arithmetic|❌ No|
|Sort an array in memory|❌ No|

---

# How System Calls Connect to Previous Topics

Every system call passes through several kernel checks.

```text
Application
      │
      ▼
System Call
      │
      ▼
┌─────────────────────────────┐
│ Seccomp                     │
│ Can I make this request?    │
└─────────────────────────────┘
      │
      ▼
┌─────────────────────────────┐
│ Capabilities                │
│ Do I have permission?       │
└─────────────────────────────┘
      │
      ▼
┌─────────────────────────────┐
│ Namespaces                  │
│ Which resources do I see?   │
└─────────────────────────────┘
      │
      ▼
┌─────────────────────────────┐
│ cgroups                     │
│ How much can I use?         │
└─────────────────────────────┘
      │
      ▼
Kernel performs operation
```

---

# Memory Trick

Whenever a process enters the kernel, remember these four questions:

|Feature|Question|
|---|---|
|**Seccomp**|Can I make this request?|
|**Capabilities**|Do I have permission?|
|**Namespaces**|Which resources do I see?|
|**cgroups**|How much can I use?|

---

# Why Are System Calls Important?

System calls:

- Protect hardware from applications.
    
- Provide controlled access to kernel services.
    
- Enable process management.
    
- Enable file access.
    
- Enable networking.
    
- Form the foundation of Linux security.
    

Without system calls, applications cannot interact safely with the operating system.

---

# Useful Commands

## Trace all system calls

```bash
strace ls
```

---

## Trace specific system calls

```bash
strace -e trace=openat,read,write cat /etc/hostname
```

---

## Follow child processes

```bash
strace -f bash -c "echo Hello"
```

---

## Summary of system calls

```bash
strace -c ls
```

---

# Interview Questions

### What is a system call?

Interface between User Space and Kernel Space.

---

### Why are system calls needed?

Applications cannot directly access hardware or privileged kernel resources.

---

### What happens during a system call?

- Switch to Kernel Mode.
    
- Kernel validates request.
    
- Executes operation.
    
- Returns result.
    
- Switches back to User Mode.
    

---

### Difference between a library function and a system call?

Library function executes in User Space.

System call executes in Kernel Space.

---

### Is `printf()` a system call?

No.

It is a library function that usually invokes `write()` internally.

---

### Does every library function invoke a system call?

No.

Functions like `strlen()` and `memcpy()` run entirely in User Space.

---

# Quick Revision

- System Call = Interface between User Space and Kernel Space.
    
- Applications cannot access hardware directly.
    
- `syscall` instruction switches CPU to Kernel Mode.
    
- Kernel validates requests before executing them.
    
- `printf()` is a library function, **not** a system call.
    
- `printf()` usually uses `write()` internally.
    
- Not every library function invokes a system call.
    
- Security mechanisms like **Seccomp**, **Capabilities**, **Namespaces**, and **cgroups** act around the system call boundary.
    

---

# GATE Corner

## Important Concepts

- User Mode vs Kernel Mode
    
- System Call Interface
    
- Mode Switching
    
- Privileged Instructions
    
- Process Management (`fork()`, `execve()`, `wait()`)
    
- File Management (`open()`, `read()`, `write()`)
    
- Memory Management (`mmap()`)
    
- OS Protection Mechanism
    
- Library Functions vs System Calls
    

### Frequently Tested GATE Facts

- `printf()` is **not** a system call.
    
- `read()`, `write()`, `fork()`, `execve()` are system calls.
    
- System calls require a **User Mode → Kernel Mode** transition.
    
- Only the kernel can directly access hardware.
    
- A library function may invoke one or more system calls, but not all library functions do.
    

---

# One-Line Summary

> **A system call is the gateway through which a user-space program safely requests services from the Linux kernel, enabling controlled access to hardware and protected operating system resources.**