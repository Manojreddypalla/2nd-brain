# Linux Cgroups (Control Groups) – Deep Short Notes

---

# 1. What is a Cgroup?

A **Control Group (cgroup)** is a Linux kernel feature that **controls, measures, and limits the resources used by a group of processes**.

**Purpose:**

> Prevent one application from consuming all system resources.

---

# 2. Why were Cgroups created?

Without cgroups:

```text
Linux Server

App A
App B
App C
```

If App A has a bug:

```cpp
while(true) {}
```

It can consume:

- 100% CPU
    
- All RAM
    
- Excessive Disk I/O
    

Result:

```text
App A  → OK
App B  → Slow
App C  → Crashes
```

Linux needed resource control.

---

# 3. Core Idea

Namespaces isolate visibility.

Cgroups isolate **resource usage**.

```text
Namespaces → "What can I see?"

Cgroups → "How much can I use?"
```

---

# 4. What resources can Cgroups control?

- CPU
    
- Memory (RAM)
    
- Disk I/O
    
- Number of Processes (PIDs)
    
- Devices
    
- Huge Pages
    
- Resource Accounting
    

---

# 5. How does it work?

Every process belongs to a cgroup.

```text
Cgroup

├── Process A
├── Process B
└── Process C
```

The kernel checks the cgroup whenever a process requests resources.

Example:

```text
Process

↓

Need 2 GB RAM

↓

Kernel checks cgroup

↓

Allowed?

Yes → Allocate

No → Reject / OOM Kill
```

---

# 6. CPU Control

Example

```text
Server

16 CPU cores

↓

Cgroup

Maximum = 2 CPUs
```

Even if the application tries to use all 16 cores,

the kernel scheduler throttles it.

---

# 7. Memory Control

Example

```text
Memory Limit

512 MB
```

Application requests

```text
700 MB
```

Kernel

```text
Limit exceeded

↓

OOM Killer terminates process
```

---

# 8. PID Control

Protects against fork bombs.

Example

```cpp
while(true)
    fork();
```

Limit

```text
Maximum

100 Processes
```

101st process creation fails.

---

# 9. Disk I/O Control

Can throttle disk speed.

Example

```text
Maximum

50 MB/s
```

Even if SSD supports 500 MB/s,

kernel limits it.

---

# 10. Resource Accounting

Cgroups also monitor usage.

Example:

- CPU time
    
- RAM used
    
- I/O usage
    
- Number of processes
    

Useful for monitoring and billing.

---

# 11. Kernel Perspective

Every process belongs to a cgroup.

```text
Process

↓

task_struct

↓

Cgroup Pointer

↓

CPU Limit
RAM Limit
PID Limit
```

Whenever the process requests resources,

the kernel checks the cgroup rules.

---

# 12. Docker Example

```bash
docker run \
--memory=512m \
--cpus=2 nginx
```

Docker simply creates a cgroup with:

```text
CPU = 2

RAM = 512 MB
```

The Linux kernel enforces these limits.

---

# 13. Namespace vs Cgroup

|Namespace|Cgroup|
|---|---|
|Isolation|Resource Control|
|What can I see?|How much can I use?|
|Processes, Network, Filesystem|CPU, RAM, Disk, PIDs|

---

# 14. Important Commands

View current cgroup:

```bash
cat /proc/self/cgroup
```

View cgroup filesystem:

```bash
ls /sys/fs/cgroup
```

---

# 15. Interview Definition

> **A cgroup (Control Group) is a Linux kernel feature that limits, prioritizes, and accounts for resource usage (CPU, memory, disk I/O, PIDs, etc.) for a group of processes, ensuring that one application cannot monopolize system resources.**

---

# 16. Mental Model (Remember Forever)

```text
Linux Kernel
│
├── Namespace
│      "What can I see?"
│
└── Cgroup
       "How much can I use?"
```

**One-line memory trick:**

- **Namespaces = Visibility (Isolation)**
    
- **Cgroups = Limits (Resources)**