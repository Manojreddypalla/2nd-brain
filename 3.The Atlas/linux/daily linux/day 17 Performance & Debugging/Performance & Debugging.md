# 📘 Linux Internals – Day 17: Performance & Debugging

---

# Introduction

Performance debugging is the process of identifying **why a Linux system is slow, unresponsive, or behaving unexpectedly**.

Linux provides a rich set of tools to monitor CPU, memory, storage, networking, processes, and system calls, allowing administrators and developers to locate bottlenecks and optimize performance.

> **Definition:** Linux Performance & Debugging is the practice of monitoring system resources, analyzing process behavior, and troubleshooting issues using Linux performance analysis tools.

---

# Why Performance Monitoring Matters

Every application ultimately uses four major system resources:

- CPU
    
- Memory
    
- Disk
    
- Network
    

When an application becomes slow, one (or more) of these resources is usually the bottleneck.

Instead of guessing, Linux allows us to **measure first, then troubleshoot**.

---

# The Four Main Resources

```text
                 Linux System
                      │
        ┌─────────────┼─────────────┐
        │             │             │
       CPU         Memory        Disk I/O
                      │
                   Network
```

|Resource|Common Issues|
|---|---|
|CPU|High utilization, slow processing|
|Memory|Low RAM, swapping|
|Disk|Slow reads/writes|
|Network|High latency, dropped packets|

---

# Mental Model

Think of a computer as a factory.

```text
Workers        → CPU

Warehouse      → Memory

Storage Room   → Disk

Roads          → Network
```

If production slows:

- Too few workers → CPU bottleneck
    
- Warehouse full → Memory pressure
    
- Slow storage room → Disk bottleneck
    
- Traffic jam → Network bottleneck
    

---

# Performance Debugging Workflow

Never start by killing processes.

Always identify the bottleneck first.

```text
System Slow
      │
      ▼
CPU?
      │
      ▼
Memory?
      │
      ▼
Disk?
      │
      ▼
Network?
      │
      ▼
Application?
```

Measure → Analyze → Fix

---

# Essential Performance Tools

|Tool|Purpose|
|---|---|
|top|Live CPU & Memory Monitoring|
|htop|Interactive Process Viewer|
|free|Memory Usage|
|vmstat|System Statistics|
|iostat|Disk Performance|
|iotop|Per-process Disk Usage|
|lsof|Open Files|
|ss|Network Connections|
|strace|Trace System Calls|
|perf|CPU Performance Analysis|

---

# top

## Purpose

Displays a real-time view of system activity.

Shows:

- Running Processes
    
- CPU Usage
    
- Memory Usage
    
- Process IDs
    
- CPU Time
    
- Load Average
    

Command

```bash
top
```

Exit

```text
q
```

---

## Example Output

```text
PID USER CPU MEM COMMAND

2100 root 45.3 3.2 nginx

1223 user 28.5 5.1 chrome

1021 root  3.0 1.0 sshd
```

---

## Useful Information

Look for:

- Highest CPU-consuming process
    
- Memory-hungry applications
    
- Zombie processes
    
- System load
    

---

# Load Average

Shown at the top of `top`.

Example:

```text
Load Average

0.25

1.10

2.30
```

These represent:

- Last 1 minute
    
- Last 5 minutes
    
- Last 15 minutes
    

Interpretation:

If a system has **4 CPU cores**:

- Load ≈ 4 → CPUs fully utilized
    
- Load > 4 → Processes waiting for CPU
    
- Load < 4 → CPU has spare capacity
    

---

# htop

Improved version of top.

Features:

- Interactive interface
    
- Mouse support
    
- Process tree
    
- Search
    
- Easier sorting
    

Command

```bash
htop
```

Install

```bash
sudo apt install htop
```

---

# free

## Purpose

Displays physical and virtual memory usage.

Command

```bash
free -h
```

Sample Output

```text
Total

Used

Free

Shared

Buff/Cache

Available

Swap
```

---

## Important Columns

|Column|Meaning|
|---|---|
|total|Total RAM|
|used|RAM currently used|
|free|Completely unused RAM|
|available|Memory available for applications|
|swap|Virtual memory on disk|

---

## Why "Free" Memory Can Be Low

Linux uses unused RAM as **cache**.

Cached memory is automatically released when applications need more RAM.

Therefore:

> **Available memory is more important than Free memory.**

---

# vmstat

## Purpose

Displays overall system statistics.

Command

```bash
vmstat 1 5
```

Meaning:

- Refresh every 1 second
    
- Display 5 times
    

---

## Important Columns

|Column|Meaning|
|---|---|
|r|Runnable processes|
|b|Blocked processes|
|swpd|Swap used|
|free|Free RAM|
|buff|Buffer memory|
|cache|Cached memory|
|si|Swap In|
|so|Swap Out|
|bi|Disk blocks read|
|bo|Disk blocks written|
|us|User CPU|
|sy|Kernel CPU|
|id|Idle CPU|
|wa|Waiting for Disk|

---

## High Swap Activity

If:

```text
si > 0

or

so > 0
```

The system is swapping.

Swapping usually indicates insufficient RAM.

---

# iostat

## Purpose

Monitors disk performance.

Command

```bash
iostat
```

Install

```bash
sudo apt install sysstat
```

Shows:

- Read operations
    
- Write operations
    
- Disk utilization
    
- Average wait time
    

Useful for:

- Slow SSD
    
- Slow HDD
    
- Database troubleshooting
    

---

# iotop

## Purpose

Shows which process is using disk I/O.

Command

```bash
sudo iotop
```

Useful when:

- Disk LED constantly active
    
- Slow file operations
    
- Heavy database writes
    

---

# lsof

## Purpose

Lists Open Files.

Linux treats many resources as files.

Including:

- Regular files
    
- Directories
    
- Devices
    
- Pipes
    
- Network sockets
    

Command

```bash
lsof
```

Example

```bash
lsof | head
```

---

## Useful Examples

### Which process opened a file?

```bash
lsof filename.txt
```

---

### Which process uses Port 8080?

```bash
lsof -i :8080
```

---

### Which files does nginx use?

```bash
lsof -c nginx
```

---

# ss

## Purpose

Shows network sockets.

Modern replacement for `netstat`.

Command

```bash
ss -tulnp
```

---

## Flags

|Flag|Meaning|
|---|---|
|t|TCP|
|u|UDP|
|l|Listening|
|n|Numeric Output|
|p|Process Information|

---

## Example

```text
TCP

0.0.0.0:22

sshd

TCP

0.0.0.0:80

nginx

TCP

0.0.0.0:5432

postgres
```

Useful for:

- Open Ports
    
- Listening Services
    
- Active Connections
    

---

# strace

## Purpose

Traces every system call made by a process.

Command

```bash
strace ls
```

Output

```text
execve()

open()

read()

write()

close()

mmap()

exit_group()
```

---

## Why is strace Useful?

Suppose a program crashes.

Instead of reading source code, observe:

```text
Application

↓

System Calls

↓

Kernel
```

If:

```text
open("file.txt")

↓

ENOENT
```

The file does not exist.

Problem solved.

---

## Trace a Running Process

```bash
strace -p <PID>
```

---

# perf

## Purpose

Advanced CPU profiling.

Command

```bash
perf stat ls
```

Measures:

- CPU Cycles
    
- Instructions
    
- Cache Misses
    
- Branch Misses
    
- Execution Time
    

Used by:

- Kernel Developers
    
- Performance Engineers
    

---

# Common Performance Problems

---

## High CPU

Symptoms

- Fan running constantly
    
- High temperature
    
- Slow response
    

Tools

```bash
top

htop
```

---

## Memory Pressure

Symptoms

- Swapping
    
- Programs freezing
    
- OOM Killer
    

Tools

```bash
free -h

vmstat
```

---

## Disk Bottleneck

Symptoms

- Slow file copy
    
- Slow boot
    
- Database delays
    

Tools

```bash
iostat

iotop
```

---

## Network Issues

Symptoms

- Slow websites
    
- Connection refused
    
- Timeout
    

Tools

```bash
ss

ping

traceroute
```

---

## Application Errors

Symptoms

- Crash
    
- Hang
    
- Missing files
    

Tool

```bash
strace
```

---

# Practical Troubleshooting Workflow

Suppose:

"My Linux server is slow."

Step 1

Check CPU

```bash
top
```

↓

High CPU?

↓

Find process.

---

Step 2

Check Memory

```bash
free -h

vmstat
```

↓

Swapping?

↓

Need more RAM or optimize applications.

---

Step 3

Check Disk

```bash
iostat

iotop
```

↓

Disk busy?

↓

Find process causing heavy I/O.

---

Step 4

Check Network

```bash
ss -tulnp
```

↓

Wrong port?

↓

Wrong service?

---

Step 5

Check Application

```bash
strace
```

↓

Observe failed system calls.

---

# Common Commands

## CPU

```bash
top
```

---

## Interactive CPU

```bash
htop
```

---

## Memory

```bash
free -h
```

---

## Overall Statistics

```bash
vmstat 1 5
```

---

## Disk

```bash
iostat
```

---

## Disk by Process

```bash
sudo iotop
```

---

## Network

```bash
ss -tulnp
```

---

## Open Files

```bash
lsof
```

---

## Trace System Calls

```bash
strace ls
```

---

## CPU Profiling

```bash
perf stat ls
```

---

# How This Connects to Previous Days

```text
Application
      │
      ▼
System Calls
      │
      ▼
Seccomp
      │
      ▼
Capabilities
      │
      ▼
Namespaces
      │
      ▼
cgroups
      │
      ▼
Linux Kernel
      │
      ▼
Hardware
```

Performance tools help observe different layers:

|Tool|Layer|
|---|---|
|top / htop|Processes & CPU|
|free / vmstat|Memory|
|iostat / iotop|Disk I/O|
|ss|Networking|
|lsof|File Descriptors|
|strace|System Calls|
|perf|CPU & Kernel Performance|

---

# Linux Internals Roadmap Summary

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
      ├── Process Management
      ├── Memory Management
      ├── Filesystem
      ├── Networking
      ├── System Calls
      │
      ▼
systemd (PID 1)
      │
      ▼
Docker
      │
      ▼
Namespaces
      │
      ▼
cgroups
      │
      ▼
Capabilities
      │
      ▼
Seccomp
      │
      ▼
Container
      │
      ▼
Performance Monitoring
```

---

# Memory Tricks

## Four Resources

```text
CPU

Memory

Disk

Network
```

---

## Which Tool Should I Use?

|Problem|Tool|
|---|---|
|High CPU|`top`, `htop`|
|Memory usage|`free -h`, `vmstat`|
|Disk bottleneck|`iostat`, `iotop`|
|Network ports|`ss`|
|Open files|`lsof`|
|Debug application|`strace`|
|CPU profiling|`perf`|

---

# Quick Revision

- Linux performance issues usually involve **CPU, Memory, Disk, or Network**.
    
- `top` and `htop` monitor processes in real time.
    
- `free -h` reports RAM and swap usage.
    
- `vmstat` provides a system-wide health overview.
    
- `iostat` and `iotop` help diagnose disk bottlenecks.
    
- `ss` shows network sockets and listening services.
    
- `lsof` lists files, sockets, and devices opened by processes.
    
- `strace` traces system calls to understand application behavior.
    
- `perf` analyzes CPU performance using hardware counters.
    

---

# Interview Questions

### Which command shows live CPU usage?

```bash
top
```

or

```bash
htop
```

---

### Which command checks memory?

```bash
free -h
```

---

### Which command shows overall system statistics?

```bash
vmstat
```

---

### Which command lists listening ports?

```bash
ss -tulnp
```

---

### Which command lists open files?

```bash
lsof
```

---

### Which command traces system calls?

```bash
strace
```

---

### Which command profiles CPU performance?

```bash
perf
```

---

# GATE Corner

## Related Operating Systems Concepts

- Process Scheduling
    
- CPU Utilization
    
- Memory Management
    
- Virtual Memory
    
- Paging & Swapping
    
- Disk Scheduling
    
- File Descriptors
    
- Networking
    
- System Calls
    
- Performance Monitoring
    

### Frequently Tested Facts

- Always identify the bottleneck first: **CPU → Memory → Disk → Network**.
    
- `top` shows process-level CPU and memory usage.
    
- `free -h` displays physical and swap memory.
    
- `vmstat` provides CPU, memory, swap, and I/O statistics.
    
- `ss` is the preferred modern tool for inspecting sockets.
    
- `lsof` is based on the Unix philosophy that many resources are represented as files.
    
- `strace` traces interactions between user-space programs and the kernel through **system calls**.
    
- `perf` uses hardware performance counters to analyze CPU behavior.
    

---

# Final Summary

> **Performance debugging in Linux is the systematic process of identifying bottlenecks in CPU, memory, disk, or network resources using specialized tools. Utilities such as `top`, `free`, `vmstat`, `iostat`, `iotop`, `ss`, `lsof`, `strace`, and `perf` allow engineers to observe system behavior, diagnose issues, and optimize performance. Together with the previous Linux Internals topics, these tools complete the foundation needed to understand, administer, debug, and troubleshoot modern Linux systems.**