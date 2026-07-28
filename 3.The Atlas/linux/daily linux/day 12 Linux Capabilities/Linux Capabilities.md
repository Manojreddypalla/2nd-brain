# Linux Internals – Day 12: Linux Capabilities

_Obsidian Notes (GATE + Systems Perspective)_

---

# Linux Capabilities

## Definition

Linux Capabilities are a **fine-grained privilege mechanism** that divides the traditional **root (UID 0)** privileges into smaller, independent permissions.

Instead of giving a process **complete root access**, Linux grants only the capabilities required to perform specific privileged operations.

> **Goal:** Follow the **Principle of Least Privilege (PoLP)** by giving processes only the permissions they actually need.

---

# Why Were Capabilities Introduced?

## Traditional Linux Privilege Model

There were only two privilege levels:

- 👑 Root (UID 0)
    
- 👤 Normal User
    

If a program required **one privileged operation**, it had to run as **root**, gaining every administrative privilege.

### Example

A web server only needs to:

- Listen on Port 80
    

But running it as root also allows it to:

- Reboot the system
    
- Mount filesystems
    
- Change kernel settings
    
- Load kernel modules
    
- Read sensitive files
    

If the server is compromised, the attacker gains unrestricted root access.

---

# Solution: Linux Capabilities

Linux splits root privileges into independent capabilities.

Instead of:

```text
Root
│
├── Everything
```

We now have:

```text
Root Privileges
│
├── CAP_NET_BIND_SERVICE
├── CAP_NET_RAW
├── CAP_SYS_TIME
├── CAP_SYS_BOOT
├── CAP_CHOWN
├── CAP_SETUID
└── ...
```

Each process receives only the capabilities it needs.

---

# Principle of Least Privilege (PoLP)

Every application should receive:

- Only the permissions required
    
- Nothing extra
    

Benefits:

- Reduces attack surface
    
- Limits damage after exploitation
    
- Improves container security
    
- Better system isolation
    

---

# Mental Model

Think of **root** as a **master key**.

```text
Master Key

✔ Door A
✔ Door B
✔ Door C
✔ Door D
✔ Door E
```

Linux Capabilities split the master key into smaller keys.

```text
CAP_NET_BIND_SERVICE → Door A

CAP_SYS_TIME → Door B

CAP_SYS_BOOT → Door C
```

A process carries only the required keys.

---

# How Linux Checks Permissions

When a process performs a privileged operation:

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
Check Required Capability
      │
      ├── Present → Allow
      └── Missing → Permission Denied
```

> **Important:** Capabilities are **not system calls**. They are permission checks performed by the kernel during privileged system calls.

---

# Process Capabilities

Every running process maintains capability sets.

Conceptually:

```text
Process

PID: 2048

Capabilities

✔ CAP_NET_BIND_SERVICE

✔ CAP_CHOWN

✘ CAP_SYS_BOOT

✘ CAP_SYS_ADMIN
```

Different processes have different capabilities.

---

# File Capabilities

Capabilities can also be attached to executable files.

Example:

```bash
getcap /usr/bin/ping
```

Output:

```text
/usr/bin/ping cap_net_raw=ep
```

When the executable runs, the kernel grants the specified capability to the new process.

---

# Process vs File Capabilities

|Process Capabilities|File Capabilities|
|---|---|
|Belong to running process|Stored on executable file|
|Used during execution|Granted when program starts|
|Temporary|Persistent until removed|

---

# Common Linux Capabilities

## CAP_NET_BIND_SERVICE

Allows binding to privileged ports (<1024).

Examples:

- HTTP (80)
    
- HTTPS (443)
    
- DNS (53)
    

---

## CAP_NET_RAW

Allows creation of raw sockets.

Used by:

- ping
    
- tcpdump
    
- Wireshark
    
- Scapy
    

---

## CAP_NET_ADMIN

Allows:

- Configure interfaces
    
- Routing
    
- Firewall rules
    
- Network namespaces
    
- Traffic control
    

---

## CAP_CHOWN

Allows changing file ownership.

Example:

```bash
chown user file.txt
```

---

## CAP_SETUID

Allows changing user IDs.

Used by:

- login
    
- sudo
    
- su
    

---

## CAP_SETGID

Allows changing group IDs.

---

## CAP_SYS_TIME

Allows changing the system clock.

---

## CAP_SYS_BOOT

Allows rebooting the machine.

---

## CAP_SYS_ADMIN

The most powerful capability.

Allows many administrative operations like:

- Mounting filesystems
    
- Namespace operations
    
- Various kernel administrative tasks
    

> Often called the **"catch-all capability"** because many privileged operations fall back to it.

---

# Capability Sets

Each process maintains multiple capability sets.

## Effective (E)

Capabilities currently active.

The kernel checks these during permission verification.

---

## Permitted (P)

Maximum capabilities the process may enable.

---

## Inheritable (I)

Capabilities that child processes may inherit across `exec()`.

---

## Bounding (B)

Upper limit of capabilities a process can ever obtain.

---

## Ambient (A)

Capabilities preserved across `exec()` for eligible executables.

---

# File Permission vs Capabilities

Traditional Linux Permissions:

```text
rwxr-xr-x
```

Control:

- Read
    
- Write
    
- Execute
    

Capabilities control **privileged kernel operations**.

They are independent mechanisms.

---

# Example: ping

Old Linux:

```text
ping

↓

Runs as Root
```

Modern Linux:

```text
ping

↓

Needs Raw Socket

↓

CAP_NET_RAW
```

Much safer than giving full root access.

---

# Linux Capabilities in Containers

Containers usually **drop unnecessary capabilities**.

Example:

Container may remove:

- CAP_SYS_ADMIN
    
- CAP_SYS_BOOT
    
- CAP_SYS_MODULE
    
- CAP_SYS_TIME
    

Result:

Even if a container is compromised:

- Cannot reboot host
    
- Cannot load kernel modules
    
- Cannot mount arbitrary filesystems
    
- Cannot modify kernel state
    

---

# Commands

## Current User

```bash
whoami
```

---

## View Process Capabilities

```bash
capsh --print
```

---

## View ping Capability

```bash
getcap /usr/bin/ping
```

---

## View File Permissions

```bash
ls -l /usr/bin/ping
```

---

## Find Executables with Capabilities

```bash
getcap -r /usr/bin 2>/dev/null
```

---

# Linux Security Layers

Linux security is built using multiple mechanisms.

```text
Namespaces
│
├── What can the process see?

cgroups
│
├── How much resources can it use?

Capabilities
│
├── What privileged operations can it perform?

Seccomp
│
└── Which system calls may it invoke?
```

Together these form the foundation of Docker, Kubernetes, Podman, and modern container runtimes.

---

# Interview Questions

### Why were Linux Capabilities introduced?

To divide root privileges into fine-grained permissions and implement the Principle of Least Privilege.

---

### Are Capabilities the same as System Calls?

No.

- System Calls → Requests made by a process to the kernel.
    
- Capabilities → Permissions checked by the kernel before allowing certain privileged system calls.
    

---

### Why is `CAP_SYS_ADMIN` dangerous?

It grants a broad range of administrative privileges and is often considered the "catch-all" capability.

---

### Which capability allows binding to port 80?

`CAP_NET_BIND_SERVICE`

---

### Which capability is commonly used by `ping`?

`CAP_NET_RAW`

---

# Quick Revision

- Linux originally had only Root and Normal User.
    
- Capabilities split root privileges into smaller permissions.
    
- They implement the Principle of Least Privilege.
    
- Every process has its own capability sets.
    
- File capabilities are attached to executables.
    
- The kernel checks capabilities during privileged system calls.
    
- Containers remove unnecessary capabilities to improve security.
    
- `CAP_SYS_ADMIN` should be granted sparingly.
    

---

# 🟨 GATE CSE Corner (OS Connection)

Although **Linux Capabilities are Linux-specific** and **not directly asked in GATE**, they are an excellent practical example of core Operating System concepts.

### 1. Protection & Security ✅ (Very Important)

GATE frequently asks about:

- Protection vs Security
    
- Access Control
    
- Principle of Least Privilege
    
- Privileged Instructions
    
- User Mode vs Kernel Mode
    

Linux Capabilities are a real-world implementation of these ideas.

---

### 2. User Mode & Kernel Mode

When a process makes a privileged system call:

```text
User Process
      │
      ▼
System Call
      │
      ▼
Kernel Mode
      │
      ▼
Permission Check (Capabilities)
```

This reinforces how **kernel mode protects critical resources**.

---

### 3. System Calls

Remember the distinction:

- **System Call** = Mechanism to request a service from the kernel.
    
- **Capability** = Permission the kernel checks before allowing certain privileged services.
    

This distinction is useful for OS interviews and conceptual GATE questions.

---

### 4. Process Management

Every process has a **Process Control Block (PCB)** containing information like:

- PID
    
- Process State
    
- Registers
    
- Scheduling Information
    
- Memory Information
    
- Credentials (UID/GID)
    
- **Security attributes (including capabilities on Linux)**
    

Capabilities are part of the process's security context.

---

### 5. Modern Operating Systems

Linux capabilities show how modern operating systems move beyond the simple "root vs user" model toward **fine-grained privilege management**, a common design principle in secure OS architectures.

> **GATE Tip:** Focus on the underlying concepts—**system calls, privilege levels, protection, security, user/kernel mode, and PCB**. Linux Capabilities themselves are implementation details, but they provide excellent intuition for these fundamental Operating Systems topics.