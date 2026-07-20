# Linux Internals – Day 11

# Control Groups (cgroups)

> **Category:** Linux Internals / Docker / Kubernetes / DevOps _(Development Topic)_

---

# What are cgroups?

**cgroups (Control Groups)** are a Linux kernel feature that **limits, monitors, and manages** the resources used by a group of processes.

**One-Line Definition:**

> A cgroup controls how much CPU, memory, and other resources a group of processes can use.

---

# Why Were cgroups Introduced?

Imagine a server running multiple applications.

```text
Host
├── Database
├── Web Server
├── Redis
└── Python Script
```

If one application starts consuming **100% CPU** or **all available RAM**, the others slow down or crash.

cgroups solve this by setting resource limits.

---

# Mental Model

Think of three roommates sharing a house.

Without cgroups:

```text
Roommate A → Uses all electricity ⚡
Roommate B → No power
Roommate C → No power
```

With cgroups:

```text
Roommate A → 30%
Roommate B → 40%
Roommate C → 30%
```

Everyone gets a fair share.

---

# What Can cgroups Control?

- 🖥️ CPU Usage
    
- 🧠 Memory (RAM)
    
- 💾 Disk I/O
    
- 🔢 Number of Processes (PIDs)
    
- 📊 Resource Accounting (usage statistics)
    

> **Note:** Network bandwidth isn't directly controlled by cgroups alone. Linux typically combines cgroups with traffic control (`tc`), eBPF, or other networking features.

---

# Namespaces vs cgroups

|Namespace|cgroup|
|---|---|
|**What can I see?**|**How much can I use?**|
|Isolates resources|Limits resources|
|PID, Mount, Network|CPU, RAM, Disk I/O, PIDs|

**Remember:**

```text
Namespaces → Isolation

cgroups → Resource Control
```

Docker needs **both**.

---

# How Docker Uses cgroups

Suppose you run:

```bash
docker run --memory=1g --cpus=2 ubuntu
```

Docker creates a cgroup like:

```text
Container
├── CPU    → 2 cores
├── Memory → 1 GB
└── Disk I/O → Limited (optional)
```

Even if the host has:

- 16 CPU cores
    
- 32 GB RAM
    

the container **cannot exceed** its assigned limits.

---

# Where Are cgroups Stored?

Linux exposes cgroups through a virtual filesystem:

```text
/sys/fs/cgroup
```

These are **not normal files**.

Reading them asks the kernel for information.

Writing to them changes resource limits.

---

# Useful Files (cgroup v2)

```text
cpu.max
```

Maximum CPU quota.

```text
memory.max
```

Maximum RAM allowed.

```text
memory.current
```

Current RAM usage.

```text
cpu.stat
```

CPU usage statistics.

```text
pids.max
```

Maximum number of processes allowed.

---

# Important Commands

### Check cgroup Version

```bash
mount | grep cgroup
```

Most modern Linux distributions use **cgroup v2**.

---

### View cgroup Files

```bash
ls /sys/fs/cgroup
```

Lists available resource controllers and statistics.

---

### Find Your Current cgroup

```bash
cat /proc/self/cgroup
```

Shows which cgroup the current process belongs to.

---

### Check Memory Usage

```bash
cat /sys/fs/cgroup/memory.current
```

Displays current memory usage (in bytes) for your cgroup.

---

### Monitor Processes

```bash
top
```

Shows CPU and memory usage in real time.

---

### Generate CPU Load

```bash
yes > /dev/null
```

Continuously prints `"y"` and discards the output, keeping the CPU busy.

Stop it with:

```text
Ctrl + C
```

---

# Why is `yes > /dev/null` Used?

```text
yes
```

Outputs:

```text
y
y
y
y
...
```

continuously.

```text
> /dev/null
```

throws the output away.

Result:

- No terminal spam
    
- CPU stays busy
    

Great for testing CPU monitoring tools.

---

# Interview Corner ⭐

### What problem do cgroups solve?

They prevent one process or container from consuming all system resources.

---

### What is the difference between Namespaces and cgroups?

- **Namespaces** isolate resources.
    
- **cgroups** limit and account for resource usage.
    

---

### Which resources can cgroups manage?

- CPU
    
- Memory
    
- Disk I/O
    
- Number of Processes (PIDs)
    

---

### Why are cgroups important for Docker?

Without cgroups, one container could use all the CPU or RAM, affecting every other container on the host.

---

# Commands Learned

```bash
mount | grep cgroup
```

```bash
ls /sys/fs/cgroup
```

```bash
cat /proc/self/cgroup
```

```bash
cat /sys/fs/cgroup/memory.current
```

```bash
top
```

```bash
yes > /dev/null
```

---

# Day 11 Summary

- **cgroups (Control Groups)** limit and monitor the resources used by a group of processes.
    
- They can manage **CPU, memory, disk I/O, and the number of processes**.
    
- **Namespaces isolate** what a process can see, while **cgroups control** how much it can use.
    
- Docker and Kubernetes combine **namespaces + cgroups** to provide isolated, resource-controlled containers.
    
- The cgroup filesystem (`/sys/fs/cgroup`) is a kernel interface used to inspect and configure resource limits.
    

## Connection So Far

```text
Linux Containers
│
├── PID Namespace      → Own process tree
├── Mount Namespace    → Own filesystem
├── Network Namespace  → Own network stack
└── cgroups            → Own resource limits
```

These are four of the core building blocks behind Docker containers. Tomorrow you'll add another security layer with **Linux Capabilities**, which split the powerful `root` privilege into smaller, fine-grained permissions.