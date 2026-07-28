# What is a Container?

A **container** is an isolated Linux process that shares the **host operating system kernel** while having its own isolated execution environment.

> **Definition:** A container is a lightweight runtime environment created using Linux kernel features such as namespaces, cgroups, capabilities, and seccomp.

Unlike a Virtual Machine, a container **does not have its own kernel**.

---

# Container vs Virtual Machine

## Virtual Machine

A virtual machine emulates an entire computer.

```text
Hardware
      │
Hypervisor
      │
├── Guest OS (Kernel + User Space)
│      └── Applications
│
├── Guest OS (Kernel + User Space)
│      └── Applications
│
└── Guest OS (Kernel + User Space)
       └── Applications
```

### Characteristics

- Own Kernel
    
- Own Drivers
    
- Own Memory Management
    
- Higher RAM usage
    
- Slower boot (seconds/minutes)
    

---

## Container

Containers share the host kernel.

```text
Hardware
      │
Host Linux Kernel
      │
Container Runtime
(Docker → containerd → runc)
      │
├── Container A
├── Container B
└── Container C
```

### Characteristics

- Shared Kernel
    
- Lightweight
    
- Starts in milliseconds
    
- Lower memory usage
    
- Fast deployment
    

---

# Biggest Misconception

A container is **NOT**:

- A tiny operating system
    
- A lightweight virtual machine
    
- A miniature Linux installation
    

A container is simply:

> **A Linux process running with isolation, resource limits, and reduced privileges.**

---

# Why Are Containers Lightweight?

Containers share:

- Linux Kernel
    
- Device Drivers
    
- Scheduler
    
- Memory Manager
    
- Filesystem Driver
    
- Networking Stack
    

Only applications and their required libraries are isolated.

---

# Docker Architecture

```text
User
      │
docker CLI
      │
Docker Engine
      │
containerd
      │
runc
      │
Linux Kernel
      │
Hardware
```

---

# What Happens During `docker run`?

Example:

```bash
docker run nginx
```

Internally, Docker performs several steps.

---

# Step 1 – Pull the Image

Docker checks:

"Does this image exist locally?"

If not:

```text
Docker Hub
      │
Download Image
      │
Local Image Store
```

---

# Step 2 – Create Writable Layer

Docker images are **read-only**.

A container adds a writable layer.

```text
Read-only Image
        │
Writable Layer
        │
Container
```

Changes remain inside the writable layer.

The original image never changes.

---

# Step 3 – Create Namespaces

Namespaces isolate different resources.

---

## PID Namespace

Creates a separate process tree.

Container:

```text
PID 1 → nginx
PID 2 → worker
```

Host:

```text
PID 29481
PID 29482
```

The same process has different PIDs.

Purpose:

- Process isolation
    

---

## Mount Namespace

Provides an isolated filesystem.

Container sees:

```text
/
```

Host sees:

```text
/
```

Although both appear as `/`, they refer to different filesystem views.

Purpose:

- Filesystem isolation
    

---

## Network Namespace

Provides independent networking.

Each container gets:

- Network Interface
    
- IP Address
    
- Routing Table
    
- Firewall Rules
    

Purpose:

- Network isolation
    

---

## UTS Namespace

Provides separate hostname.

Example:

```bash
hostname
```

Host:

```text
linux-server
```

Container:

```text
nginx-container
```

Purpose:

- Hostname isolation
    

---

## IPC Namespace

Provides isolated:

- Shared Memory
    
- Semaphores
    
- Message Queues
    

Purpose:

- IPC isolation
    

---

## User Namespace (Optional)

Maps container users to different host users.

Container root:

```text
UID 0
```

Host:

```text
UID 100000
```

Purpose:

- Improved security
    

---

# Step 4 – Apply cgroups

Docker configures resource limits.

Example:

```bash
docker run --memory=512m nginx
```

cgroups limit:

- CPU
    
- Memory
    
- Disk I/O
    
- Number of Processes
    

Example:

```text
Memory Limit = 512 MB

↓

Application requests 2 GB

↓

Request Denied / OOM Kill
```

Purpose:

- Resource Management
    

---

# Step 5 – Drop Linux Capabilities

Root inside a container should not have unrestricted privileges.

Docker removes unnecessary capabilities.

Examples removed:

- Load Kernel Modules
    
- Mount Filesystems
    
- Modify System Clock
    
- Reboot Host
    

Purpose:

- Principle of Least Privilege
    

---

# Step 6 – Apply Seccomp

Docker installs a Seccomp profile.

Dangerous system calls are blocked.

Examples:

- `mount()`
    
- `swapon()`
    
- `ptrace()`
    
- `kexec_load()`
    

Purpose:

- Reduce attack surface
    

---

# Step 7 – Mount Overlay Filesystem

Docker combines image layers.

```text
Image Layer 1
       │
Image Layer 2
       │
Image Layer 3
       │
Writable Layer
       │
Merged Filesystem
```

Applications see a single filesystem.

Purpose:

- Efficient storage
    
- Image sharing
    
- Fast container creation
    

---

# Step 8 – Start PID 1

Finally Docker starts the application.

Example:

```text
nginx
```

Inside container:

```text
PID = 1
```

Host:

```text
PID = 18273
```

Different PID namespaces.

---

# Complete Container Creation Flow

```text
docker run nginx
        │
        ▼
Pull Image (if required)
        │
        ▼
Create Writable Layer
        │
        ▼
Create Namespaces
        │
        ▼
Apply cgroups
        │
        ▼
Drop Capabilities
        │
        ▼
Apply Seccomp
        │
        ▼
Mount Overlay Filesystem
        │
        ▼
Start PID 1
        │
        ▼
Container Running
```

---

# Linux Features Used by Containers

|Linux Feature|Purpose|
|---|---|
|Namespaces|Isolation|
|cgroups|Resource Limits|
|Capabilities|Privilege Reduction|
|Seccomp|System Call Filtering|
|Overlay Filesystem|Layered Storage|
|runc|Creates and starts containers|
|OCI Image|Standard image format|

---

# Overlay Filesystem

Docker images consist of multiple layers.

Example:

```text
Ubuntu Base
      │
Python Layer
      │
Application Layer
      │
Writable Layer
```

Advantages:

- Faster downloads
    
- Layer reuse
    
- Smaller images
    
- Efficient storage
    

---

# OCI (Open Container Initiative)

OCI defines open standards for:

- Container Images
    
- Container Runtimes
    

Examples supporting OCI:

- Docker
    
- Podman
    
- containerd
    
- CRI-O
    

---

# What is runc?

`runc` is the low-level container runtime.

Responsibilities:

- Create namespaces
    
- Configure cgroups
    
- Apply capabilities
    
- Apply seccomp
    
- Start container process
    

Docker delegates container creation to **runc**.

---

# Docker Runtime Stack

```text
User
      │
docker CLI
      │
Docker Engine
      │
containerd
      │
runc
      │
Linux Kernel
```

---

# Container Mental Model

```text
Application
      │
Namespaces
      │
cgroups
      │
Capabilities
      │
Seccomp
      │
Overlay Filesystem
      │
Linux Kernel
      │
Hardware
```

Applications believe they have their own machine.

In reality, they share the same kernel.

---

# How This Connects to Previous Days

```text
Power On
      │
      ▼
BIOS / UEFI
      │
      ▼
GRUB
      │
      ▼
Linux Kernel
      │
      ▼
systemd (Host PID 1)
      │
      ▼
Docker Service
      │
      ▼
docker run nginx
      │
      ├── Create Namespaces
      ├── Configure cgroups
      ├── Drop Capabilities
      ├── Apply Seccomp
      ├── Mount Overlay Filesystem
      └── Start Container PID 1
```

---

# Host vs Container PID 1

Host:

```text
PID 1 → systemd
```

Container:

```text
PID 1 → nginx
```

Both are PID 1 **inside their own PID namespaces**.

---

# Useful Commands

## List images

```bash
docker images
```

---

## Running containers

```bash
docker ps
```

---

## Inspect container

```bash
docker inspect <container_id>
```

---

## Resource usage

```bash
docker stats
```

---

## Enter container

```bash
docker exec -it <container_id> bash
```

---

## View namespaces

```bash
lsns
```

---

## View cgroups

```bash
cat /proc/self/cgroup
```

---

## Namespace links

```bash
ls -l /proc/$$/ns
```

---

# Memory Tricks

## What Makes Containers Possible?

|Feature|Remember As|
|---|---|
|Namespaces|What can I see?|
|cgroups|How much can I use?|
|Capabilities|What am I allowed to do?|
|Seccomp|Which system calls can I make?|
|Overlay FS|What files do I see?|
|runc|Who creates the container?|

---

## Container Creation Formula

```text
Image
   │
Namespaces
   │
cgroups
   │
Capabilities
   │
Seccomp
   │
Overlay Filesystem
   │
PID 1
   │
Running Container
```

---

# Quick Revision

- Containers share the host Linux kernel.
    
- Containers are isolated Linux processes.
    
- Docker combines Linux kernel features to build containers.
    
- Namespaces provide isolation.
    
- cgroups enforce resource limits.
    
- Capabilities reduce privileges.
    
- Seccomp filters dangerous system calls.
    
- Overlay Filesystem provides layered images.
    
- runc creates and starts containers.
    
- Every container has its own PID 1 within its PID namespace.
    

---

# Interview Questions

### What is a container?

An isolated Linux process sharing the host kernel.

---

### Why are containers lightweight?

Because they share the host kernel instead of running separate kernels.

---

### Difference between VM and Container?

VM:

- Own kernel
    
- Hypervisor
    
- Heavy
    

Container:

- Shared kernel
    
- Lightweight
    
- Fast startup
    

---

### What happens during `docker run`?

1. Pull image
    
2. Create writable layer
    
3. Create namespaces
    
4. Configure cgroups
    
5. Drop capabilities
    
6. Apply seccomp
    
7. Mount overlay filesystem
    
8. Start container PID 1
    

---

### What is runc?

A low-level OCI runtime responsible for creating and starting containers.

---

### What is OCI?

An open standard defining container image and runtime specifications.

---

### Why does a container have PID 1?

Because within its own **PID namespace**, the first process started by the runtime becomes PID 1.

---

# GATE Corner

## Operating Systems Concepts

- Process Isolation
    
- Process Hierarchy
    
- PID Namespace
    
- Resource Management
    
- Protection & Security
    
- Virtualization
    
- Filesystem Layering
    
- User Space vs Kernel Space
    
- System Calls
    
- Process Creation
    

### Frequently Tested Facts

- Containers **share the host kernel**.
    
- A container is **not** a virtual machine.
    
- Docker relies on **Linux kernel features**, not a separate kernel.
    
- **Namespaces** provide isolation.
    
- **cgroups** provide resource limits.
    
- **Capabilities** reduce privileges.
    
- **Seccomp** filters system calls.
    
- **OverlayFS** enables layered images.
    
- **PID 1 inside a container is different from PID 1 on the host** because they belong to different PID namespaces.
    

---

# Final Summary

> **A Docker container is an isolated Linux process created by the container runtime using namespaces for isolation, cgroups for resource limits, capabilities for privilege reduction, seccomp for system call filtering, and OverlayFS for layered storage. Unlike virtual machines, containers share the host Linux kernel, making them lightweight, fast, and efficient.**