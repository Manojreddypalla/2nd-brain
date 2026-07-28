# 🧠 Linux Internals Checkpoint: Namespaces, cgroups, Capabilities & Seccomp

---

# The Big Picture

Whenever a process wants to perform an operation, it makes a **system call** to the Linux kernel.

Before executing that request, the kernel performs several checks.

```text
Application
      │
      ▼
 System Call
      │
      ▼
 Linux Kernel
```

The kernel asks **four important questions**.

---

# 1️⃣ Seccomp → Can you make this request?

## Purpose

Restricts which **system calls** a process is allowed to invoke.

### Controls

- Allowed system calls
    

### Question

> **Can this process even request this system call?**

### Example

```text
mount()

↓

Seccomp

↓

❌ Blocked
```

The system call never executes.

### Memory Keyword

**System Call Filter**

---

# 2️⃣ Capabilities → Do you have permission?

## Purpose

Splits root privileges into smaller permissions.

### Controls

- Privileged operations
    

### Question

> **If the system call is allowed, is this process privileged enough?**

### Example

```text
bind(Port 80)

↓

CAP_NET_BIND_SERVICE ?

↓

✔ Yes → Allowed

❌ No → Permission Denied
```

### Memory Keyword

**Privileges**

---

# 3️⃣ Namespaces → Which resources are you using?

## Purpose

Provides isolation between processes.

Each process sees its own view of system resources.

### Controls

- Processes
    
- Filesystems
    
- Networks
    
- Hostname
    
- Users
    
- IPC
    

### Question

> **Which resources should this process see?**

### Example

```text
open("/etc/passwd")

↓

Mount Namespace

↓

Container's /etc/passwd
```

### Memory Keyword

**Isolation**

---

# 4️⃣ cgroups → How much can you use?

## Purpose

Limits resource usage.

### Controls

- CPU
    
- Memory
    
- Disk I/O
    
- Network (indirectly via controllers)
    
- Number of processes (PID controller)
    

### Question

> **Does this process have enough resource quota?**

### Example

```text
Allocate 2 GB RAM

↓

cgroup Limit = 1 GB

↓

Allocation Fails
```

### Memory Keyword

**Resource Limits**

---

# Complete Flow

```text
                Application
                      │
                      ▼
               System Call
                      │
                      ▼
      ┌────────────────────────────┐
      │ Seccomp                    │
      │ Can I make this request?   │
      └────────────────────────────┘
                      │
                      ▼
      ┌────────────────────────────┐
      │ Capabilities               │
      │ Do I have permission?      │
      └────────────────────────────┘
                      │
                      ▼
      ┌────────────────────────────┐
      │ Namespaces                 │
      │ Which resources do I see?  │
      └────────────────────────────┘
                      │
                      ▼
      ┌────────────────────────────┐
      │ cgroups                    │
      │ How much can I use?        │
      └────────────────────────────┘
                      │
                      ▼
              Linux Kernel
                      │
                      ▼
                 Hardware
```

---

# Comparison Table

|Feature|Controls|Kernel's Question|Memory Word|
|---|---|---|---|
|**Seccomp**|System Calls|Can I make this request?|**Filter**|
|**Capabilities**|Privileges|Do I have permission?|**Privileges**|
|**Namespaces**|Resource Visibility|Which resources am I using?|**Isolation**|
|**cgroups**|Resource Usage|How much can I use?|**Limits**|

---

# Easy Memory Trick

Think of entering a secure company building.

|Security Layer|Linux Feature|
|---|---|
|Which doors are open?|**Seccomp**|
|Do you have the access card?|**Capabilities**|
|Which office are you assigned to?|**Namespaces**|
|What's your budget/quota?|**cgroups**|

---

# One-Line Revision

- **Seccomp** → **Can I make this request?**
    
- **Capabilities** → **Do I have permission?**
    
- **Namespaces** → **Which resources can I see?**
    
- **cgroups** → **How much can I use?**
    

---

# GATE CSE Connection

|GATE Topic|Linux Implementation|
|---|---|
|System Calls|Seccomp filters system calls|
|Protection|Capabilities enforce privileges|
|Process Isolation|Namespaces isolate resources|
|Resource Management|cgroups limit CPU, memory, I/O|
|User Mode → Kernel Mode|All four checks occur around the system call boundary|

> **Key Takeaway:** These are **not four separate concepts**. They are **four different checks the Linux kernel applies around a process's interaction with the kernel**, especially during **system call execution**. Understanding this relationship gives you both strong Linux internals intuition and a practical bridge to core Operating Systems concepts in GATE.