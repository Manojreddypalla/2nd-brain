# 📘 Linux Internals – Day 15: systemd – The Init System & Service Manager

---

# What is systemd?

**systemd** is the **first userspace process** started by the Linux kernel after the system boots.

It is the **init system** and **service manager** responsible for starting, stopping, and managing the entire userspace environment.

> **Definition:** systemd is the init system that initializes userspace, manages system services, and coordinates the Linux operating system after the kernel finishes booting.

---

# Why Do We Need systemd?

The Linux kernel initializes:

- CPU
    
- Memory
    
- Device Drivers
    
- Filesystem
    
- Scheduler
    
- Networking
    

However, the kernel **does not**:

- Start SSH
    
- Start Docker
    
- Start NetworkManager
    
- Start the Login Manager
    
- Start the Desktop Environment
    

Someone must organize and start all these services.

That job belongs to **systemd**.

---

# Linux Boot Process

```text
Power On
      │
      ▼
BIOS / UEFI
      │
      ▼
Bootloader (GRUB)
      │
      ▼
Linux Kernel
      │
Initialize Hardware
Initialize Memory
Initialize Drivers
Initialize Scheduler
Mount Root Filesystem
      │
      ▼
Starts systemd (PID 1)
      │
      ▼
Starts All System Services
      │
      ▼
User Login / Desktop / Terminal
```

---

# Why is systemd PID 1?

After booting, the Linux kernel launches **exactly one userspace process**.

That process is **systemd**.

Since it is created first, it receives:

```text
PID = 1
```

Every other userspace process is directly or indirectly created by **systemd**.

---

# Why is PID 1 Special?

PID 1 has unique responsibilities:

- Starts system services
    
- Manages service dependencies
    
- Starts user sessions
    
- Handles shutdown and reboot
    
- Adopts orphan processes
    
- Keeps the system running
    

Without PID 1, Linux cannot provide a usable operating system.

---

# Process Hierarchy

```text
systemd (PID 1)
│
├── NetworkManager
├── sshd
├── Docker
├── cron
├── Display Manager
│      └── User Session
│              └── Terminal
│                      └── bash
│                              └── vim
```

Every userspace process ultimately traces back to **PID 1**.

---

# What Does systemd Manage?

systemd manages:

- System Boot
    
- Background Services (Daemons)
    
- Service Dependencies
    
- Mount Points
    
- Socket Activation
    
- Timers
    
- Logging
    
- Shutdown
    
- Reboot
    

---

# What is an Init System?

An **Init System** is the first userspace program started by the kernel.

Responsibilities:

- Initialize userspace
    
- Start services
    
- Monitor services
    
- Shutdown cleanly
    

Historically Linux used:

- SysV Init
    
- Upstart
    

Modern Linux primarily uses:

- **systemd**
    

---

# What is a Daemon?

A **Daemon** is a background process that runs continuously and provides services.

Examples:

- SSH Server
    
- Docker
    
- Apache
    
- Nginx
    
- MySQL
    
- NetworkManager
    

systemd starts and monitors these daemons.

---

# systemd Units

Everything managed by systemd is represented as a **Unit**.

A Unit is simply an object managed by systemd.

---

# Types of Units

## 1. Service Unit (.service)

Represents a background service.

Examples:

```text
ssh.service
docker.service
nginx.service
```

Purpose:

- Start
    
- Stop
    
- Restart
    
- Monitor services
    

---

## 2. Target Unit (.target)

Represents a group of related units.

Similar to traditional runlevels.

Examples:

```text
multi-user.target
graphical.target
network.target
```

Example:

```text
graphical.target
│
├── display-manager.service
├── NetworkManager.service
└── sound.service
```

---

## 3. Mount Unit (.mount)

Represents mounted filesystems.

Example:

```text
home.mount
```

---

## 4. Socket Unit (.socket)

Implements **Socket Activation**.

Instead of running a service continuously:

```text
Incoming Connection

↓

systemd detects socket activity

↓

Starts service

↓

Service handles request
```

This improves resource efficiency.

---

## 5. Timer Unit (.timer)

Alternative to cron jobs.

Used for scheduled tasks.

Example:

```text
backup.timer
```

---

## 6. Path Unit (.path)

Monitors filesystem paths.

Example:

```text
Watch /var/log

↓

File Changes

↓

Start Service
```

---

# Service Dependencies

Many services depend on others.

Example:

Docker requires networking.

```text
NetworkManager

↓

Docker

↓

Containers
```

systemd automatically starts services in the correct order.

---

# Service Lifecycle

```text
Inactive

↓

Starting

↓

Running

↓

Stopping

↓

Stopped
```

systemd monitors services and can restart them automatically if configured.

---

# systemctl

`systemctl` is the command-line tool used to communicate with **systemd**.

Think of it as:

```text
You

↓

systemctl

↓

systemd

↓

Service
```

---

# Common systemctl Commands

## View overall system status

```bash
systemctl status
```

---

## List running services

```bash
systemctl list-units --type=service --state=running
```

---

## Inspect a service

```bash
systemctl status ssh
```

or

```bash
systemctl status NetworkManager
```

---

## Start a service

```bash
sudo systemctl start nginx
```

---

## Stop a service

```bash
sudo systemctl stop nginx
```

---

## Restart a service

```bash
sudo systemctl restart nginx
```

---

## Enable service at boot

```bash
sudo systemctl enable nginx
```

---

## Disable service at boot

```bash
sudo systemctl disable nginx
```

---

# journald

systemd includes its own logging system called **journald**.

Instead of scattered log files, logs are collected into the **system journal**.

---

# journalctl

View logs managed by systemd.

---

## Current boot logs

```bash
journalctl -b
```

---

## Latest 20 log entries

```bash
journalctl -n 20
```

---

## Follow logs live

```bash
journalctl -f
```

---

# Complete Boot Flow

```text
Power Button
      │
      ▼
BIOS / UEFI
      │
      ▼
Bootloader (GRUB)
      │
      ▼
Linux Kernel
      │
Initialize:
 • CPU
 • Memory
 • Drivers
 • Scheduler
 • Filesystem
 • Networking
      │
      ▼
Starts systemd (PID 1)
      │
      ▼
systemd starts:
 • SSH
 • Docker
 • NetworkManager
 • Display Manager
 • Logging
 • User Login
      │
      ▼
Linux Ready
```

---

# How systemd Connects to Previous Topics

```text
Power On
      │
      ▼
Kernel
      │
      ├── Memory Management
      ├── Scheduler
      ├── Filesystem
      ├── Networking
      └── System Calls
      │
      ▼
systemd (PID 1)
      │
      ├── Starts Docker
      │      ├── Namespaces
      │      ├── cgroups
      │      ├── Capabilities
      │      └── Seccomp
      │
      ├── Starts SSH
      ├── Starts NetworkManager
      ├── Starts Display Manager
      └── Starts Login Services
```

systemd **does not replace the kernel**.

The kernel provides core OS functionality.

systemd organizes and manages the services that use those kernel features.

---

# Memory Tricks

## Boot Sequence

```text
Power On
↓

BIOS / UEFI
↓

GRUB
↓

Kernel
↓

systemd (PID 1)
↓

Services
↓

User Login
```

---

## systemd Responsibilities

**systemd = Starts + Stops + Monitors + Logs + Boots**

---

## Remember the Unit Types

|Unit|Purpose|
|---|---|
|`.service`|Background services|
|`.target`|Group of units|
|`.mount`|Mounted filesystems|
|`.socket`|Socket activation|
|`.timer`|Scheduled tasks|
|`.path`|Filesystem monitoring|

---

# Quick Revision

- systemd is the **first userspace process**.
    
- systemd always runs as **PID 1**.
    
- It initializes and manages the Linux userspace.
    
- systemctl communicates with systemd.
    
- journald stores system logs.
    
- journalctl reads system logs.
    
- Everything managed by systemd is called a **Unit**.
    
- Services are represented by `.service` units.
    
- systemd manages service dependencies and boot order.
    

---

# Interview Questions

### What is systemd?

The modern Linux init system and service manager.

---

### Why is systemd PID 1?

Because the kernel starts it as the first userspace process after boot.

---

### What is the role of PID 1?

- Initialize userspace
    
- Start services
    
- Manage services
    
- Handle shutdown/reboot
    
- Adopt orphan processes
    

---

### What is a Unit?

An object managed by systemd (e.g., service, target, mount, timer).

---

### Difference between `.service` and `.target`

- `.service` → Represents a single background service.
    
- `.target` → Represents a collection of units used to reach a desired system state.
    

---

### What is `systemctl`?

The command-line utility used to control and inspect systemd.

---

### What is `journalctl`?

The command-line utility used to view logs collected by `journald`.

---

# GATE Corner

## Operating Systems Concepts

- Boot Process
    
- Process Creation
    
- PID
    
- Parent-Child Process Relationship
    
- Init Process
    
- Daemons
    
- Service Management
    
- Boot Sequence
    
- User Space vs Kernel Space
    

### Frequently Tested Facts

- The **kernel** is **not** PID 1.
    
- **systemd** (or another init system) is typically **PID 1**.
    
- PID 1 is the first **userspace** process.
    
- Every userspace process is a descendant of PID 1.
    
- `systemctl` manages services through systemd.
    
- `journalctl` reads logs maintained by `journald`.
    

---

# One-Line Summary

> **systemd is Linux's init system and service manager. Started by the kernel as PID 1, it initializes userspace, manages services, resolves dependencies, controls the boot process, and keeps the operating system running after the kernel has finished its work.**