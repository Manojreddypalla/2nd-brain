# Linux Internals – Day 13: Seccomp (Secure Computing Mode)

_Obsidian Notes (Linux Internals + GATE CSE Perspective)_

---

# Seccomp (Secure Computing Mode)

## Definition

**Seccomp (Secure Computing Mode)** is a Linux kernel security feature that **restricts the system calls a process is allowed to make**.

Instead of allowing a process to invoke every Linux system call, Seccomp creates a **whitelist** (or filter) of permitted system calls and blocks the rest.

> **Goal:** Reduce the kernel attack surface by preventing unnecessary system calls.

---

# Why Was Seccomp Introduced?

Every user-space program communicates with the Linux kernel using **system calls**.

Examples:

- `read()`
    
- `write()`
    
- `open()`
    
- `close()`
    
- `socket()`
    
- `fork()`
    
- `execve()`
    
- `mount()`
    
- `ptrace()`
    

A normal Linux kernel exposes **hundreds of system calls**.

However, most applications only need a small subset.

### Example

A web server needs:

- Read files
    
- Accept network connections
    
- Send responses
    

It does **not** need to:

- Reboot the machine
    
- Mount filesystems
    
- Load kernel modules
    
- Debug other processes
    

Allowing access to every system call unnecessarily increases the attack surface.

---

# Problem Without Seccomp

Suppose an attacker exploits a web server.

The attacker may attempt dangerous system calls like:

- `mount()`
    
- `ptrace()`
    
- `kexec_load()`
    
- `reboot()`
    

Even if many fail because of permissions, the attacker can still attempt to reach those kernel interfaces.

Every reachable kernel interface is a potential attack vector.

---

# Solution: Seccomp

Seccomp blocks unnecessary system calls before they are executed.

Instead of:

```text
Application
│
├── read()
├── write()
├── socket()
├── mount()
├── ptrace()
├── reboot()
└── ...
```

Seccomp creates a filter:

```text
Application
│
├── ✔ read()
├── ✔ write()
├── ✔ socket()
├── ✔ open()
├── ❌ mount()
├── ❌ ptrace()
├── ❌ reboot()
└── ❌ kexec_load()
```

Blocked system calls never reach the kernel implementation.

---

# Mental Model

Imagine the Linux kernel is a building with many doors.

Each door represents one system call.

Without Seccomp:

```text
Kernel

🚪 read()

🚪 write()

🚪 socket()

🚪 mount()

🚪 ptrace()

🚪 reboot()

🚪 clone()

🚪 ...
```

Every process can knock on every door.

With Seccomp:

```text
Kernel

✔ read()

✔ write()

✔ socket()

✔ open()

❌ mount()

❌ ptrace()

❌ reboot()

❌ kexec_load()
```

Many doors are permanently locked for that process.

---

# How Seccomp Works

When a process invokes a system call:

```text
Application
      │
      ▼
System Call
      │
      ▼
Seccomp Filter
      │
      ├── Allowed
      │         │
      │         ▼
      │   Continue Execution
      │
      └── Blocked
                │
                ▼
      System Call Denied
```

Seccomp operates **before** the kernel executes the requested system call.

---

# Attack Surface

## Definition

**Attack Surface** is the total number of interfaces that an attacker can potentially exploit.

Suppose Linux provides:

```text
450 System Calls
```

A web server only needs:

```text
35 System Calls
```

Without Seccomp:

```text
Attacker can attempt

450 kernel interfaces
```

With Seccomp:

```text
Attacker can attempt

35 kernel interfaces
```

Smaller attack surface = Better security.

---

# Seccomp vs Linux Capabilities

These two concepts are often confused.

They solve different problems.

---

## Seccomp

Answers:

> **"Can this process invoke this system call?"**

Example:

```text
mount()

↓

Seccomp

↓

❌ Blocked
```

The system call never executes.

---

## Capabilities

Answers:

> **"This system call is allowed, but does this process have the required privilege?"**

Example:

```text
mount()

↓

Seccomp

✔ Allowed

↓

Capability Check

↓

CAP_SYS_ADMIN ?

↓

❌ Missing

↓

Permission Denied
```

---

# Difference Between Seccomp and Capabilities

|Seccomp|Capabilities|
|---|---|
|Controls **system calls**|Controls **privileges**|
|Blocks unnecessary kernel interfaces|Grants fine-grained root permissions|
|Applied before execution|Checked during privileged operations|
|Reduces attack surface|Implements Principle of Least Privilege|
|Security sandbox|Privilege management|

---

# Why Are Both Needed?

They provide **Defense in Depth** (layered security).

Example:

A process calls:

```text
mount()
```

Step 1:

```text
Seccomp

↓

Is mount() allowed?

↓

No

↓

Blocked
```

If allowed:

```text
Seccomp

↓

Yes

↓

Capability Check

↓

CAP_SYS_ADMIN ?

↓

No

↓

Permission Denied
```

Both layers protect the system in different ways.

---

# Defense in Depth

Modern Linux security uses multiple independent protection layers.

```text
Application
      │
      ▼
Seccomp
      │
      ▼
Capabilities
      │
      ▼
File Permissions
      │
      ▼
SELinux / AppArmor (Optional)
      │
      ▼
Linux Kernel
```

If one layer fails, another still protects the system.

---

# Seccomp in Docker

Docker automatically applies a **default Seccomp profile**.

It allows commonly required system calls like:

- `read()`
    
- `write()`
    
- `open()`
    
- `close()`
    
- `socket()`
    
- `accept()`
    

It blocks many dangerous system calls that typical containers do not need.

Examples include operations related to:

- Kernel loading
    
- System reboot
    
- Swapping
    
- Certain debugging interfaces
    

This limits what compromised containers can do.

---

# Seccomp in Real Software

Many applications use Seccomp sandboxes.

Examples:

- Docker
    
- Kubernetes
    
- Chrome
    
- Firefox
    
- Flatpak
    
- Snap
    

These applications allow only the system calls required for normal operation.

---

# Relationship with Previous Topics

|Feature|Controls|
|---|---|
|Namespaces|What a process can **see**|
|cgroups|How much resources a process can **use**|
|Capabilities|What privileged operations a process can **perform**|
|Seccomp|Which **system calls** a process may invoke|

---

# Linux Security Stack

```text
                Process
                   │
                   ▼
        Namespaces
 (Isolation of Resources)
                   │
                   ▼
            cgroups
 (Resource Limitation)
                   │
                   ▼
         Capabilities
 (Privilege Management)
                   │
                   ▼
            Seccomp
 (System Call Filtering)
                   │
                   ▼
             Linux Kernel
```

These four mechanisms form the core of container security.

---

# Practical Example

Suppose an attacker exploits Nginx.

The attacker executes:

```c
mount(...)
```

### Case 1

Seccomp blocks `mount()`.

```text
Application

↓

mount()

↓

Seccomp

↓

❌ Blocked
```

The kernel never executes the system call.

---

### Case 2

Seccomp allows `mount()`.

```text
Application

↓

mount()

↓

Seccomp

↓

✔ Allowed

↓

Capability Check

↓

CAP_SYS_ADMIN ?

↓

❌ Missing

↓

Permission Denied
```

Even though the system call exists, the process lacks the required privilege.

---

# Commands

## View Available System Calls

```bash
ausyscall --dump | head
```

Alternative:

```bash
grep "^#define __NR_" /usr/include/asm-generic/unistd.h | head
```

Shows Linux system call definitions.

---

## Trace Every System Call

```bash
strace ls
```

Displays every system call made by `ls`.

---

## Trace File-Related System Calls

```bash
strace -e trace=file cat /etc/hostname
```

Shows only file operations.

---

## Trace Network System Calls

```bash
strace -e trace=network ping -c 1 8.8.8.8
```

Displays networking-related system calls.

---

## Compare Commands

```bash
strace echo "Hello"
```

vs

```bash
strace ls /
```

Observe how even simple commands generate many system calls.

---

# Why is `strace` Important?

`strace` intercepts and displays every system call a process makes.

It is one of the most useful Linux debugging tools because it reveals how applications interact with the kernel.

---

# Interview Questions

### What is Seccomp?

A Linux kernel feature that restricts the system calls a process is allowed to invoke.

---

### Why is Seccomp useful?

It reduces the kernel attack surface by blocking unnecessary system calls.

---

### Is Seccomp the same as Capabilities?

No.

- Seccomp filters **which system calls** are allowed.
    
- Capabilities determine **whether a privileged operation is permitted** after the system call reaches the kernel.
    

---

### Why do containers use Seccomp?

Containers typically need only a small subset of Linux system calls. Blocking the rest improves security and limits the impact of vulnerabilities.

---

### What tool shows system calls made by a program?

`strace`

---

# Quick Revision

- Every application communicates with the kernel through **system calls**.
    
- Linux provides hundreds of system calls.
    
- Most applications require only a small subset.
    
- Seccomp filters which system calls a process may invoke.
    
- Blocking unnecessary system calls reduces the attack surface.
    
- Seccomp is commonly used by Docker, Kubernetes, Chrome, Firefox, and other sandboxed applications.
    
- Seccomp and Capabilities solve different security problems and work together.
    
- `strace` is used to observe real system calls.
    

---

# 🟨 GATE CSE Corner (Operating Systems Connection)

Although **Seccomp itself is Linux-specific** and **not directly part of the GATE syllabus**, it reinforces several important Operating Systems concepts.

## 1. System Calls ⭐⭐⭐⭐⭐

This is the strongest connection.

Every Seccomp filter operates on **system calls**, which are a core GATE topic.

Remember:

```text
User Space
      │
System Call
      │
Kernel Space
```

Seccomp sits exactly at this boundary.

---

## 2. User Mode vs Kernel Mode ⭐⭐⭐⭐⭐

Applications execute in **User Mode**.

Privileged operations require entering **Kernel Mode** through system calls.

Seccomp filters these transitions before the kernel executes them.

---

## 3. Protection and Security ⭐⭐⭐⭐☆

Seccomp is a practical implementation of **Protection** in Operating Systems.

Related GATE topics:

- Protection vs Security
    
- Principle of Least Privilege
    
- Controlled access to kernel resources
    
- Attack surface reduction
    

---

## 4. Process Management ⭐⭐⭐☆☆

Seccomp filters are associated with individual processes.

Different processes can have different Seccomp policies.

This extends the idea that each process has its own execution context.

---

## 5. Containerization (Beyond GATE)

Modern containers rely on four Linux primitives:

- **Namespaces** → Isolation
    
- **cgroups** → Resource control
    
- **Capabilities** → Privilege control
    
- **Seccomp** → System call filtering
    

Understanding these gives strong practical intuition for modern Operating Systems, cloud computing, and DevOps.

---

# Final Summary

```text
Namespaces   → What can the process SEE?

cgroups      → How much can the process USE?

Capabilities → What privileged operations can it PERFORM?

Seccomp      → Which SYSTEM CALLS can it MAKE?
```

> **Memory Tip:** Think of the kernel as a secure building:
> 
> - **Namespaces** decide _which rooms you can see_.
>     
> - **cgroups** decide _how many resources you may consume inside_.
>     
> - **Capabilities** decide _which restricted rooms you're authorized to enter_.
>     
> - **Seccomp** decides _which doors you're even allowed to knock on_.
>