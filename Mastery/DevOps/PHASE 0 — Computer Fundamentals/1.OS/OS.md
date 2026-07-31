For **DevOps / Linux interviews**, you don't need GATE-level theory. You need to know **what it is, why it exists, where it's used, and the Linux commands/tools related to it.**

Here's a compact DevOps version of each topic.

---

# DevOps OS Notes

---

# 1. Process

## What?

A **process** is a running instance of a program.

Example

```
chrome
nginx
docker
python app.py
```

Each process has

- PID
    
- Memory
    
- CPU state
    
- Open files
    
- Threads
    

---

## Linux Commands

```
ps
ps aux
top
htop
pidof nginx
pgrep nginx
kill PID
kill -9 PID
```

---

## Why DevOps cares

- Find high CPU usage
    
- Kill stuck services
    
- Restart applications
    
- Monitor resource usage
    

---

# 2. Thread

## What?

A thread is the **smallest unit executed by CPU.**

One process

```
Chrome
 ├── Thread
 ├── Thread
 ├── Thread
```

Threads share

- Memory
    
- Files
    
- Variables
    

---

## Commands

```
ps -T -p PID

top -H
```

---

## DevOps

Many servers are multithreaded.

Example

```
Java
Nginx
Apache
MySQL
```

---

# 3. Scheduling

## What?

CPU decides

```
Who runs next?
```

Scheduler allocates CPU time.

---

Common Algorithms

- FCFS
    
- SJF
    
- Round Robin
    
- Priority
    

Linux uses

```
Completely Fair Scheduler (CFS)
```

---

Useful Commands

```
nice

renice

chrt
```

---

DevOps

Adjust process priority.

---

# 4. Context Switch

## What?

CPU changes from

```
Process A

↓

Process B
```

CPU saves

- Registers
    
- Program Counter
    
- Stack Pointer
    

then restores another process.

---

High Context Switch

↓

Lower Performance

---

Commands

```
vmstat 1

pidstat
```

---

# 5. Memory

Every process gets memory.

Contains

```
Code

Heap

Stack

Libraries

Data
```

---

Commands

```
free -h

vmstat

cat /proc/meminfo

pmap PID
```

---

DevOps

Check

- RAM usage
    
- Memory leak
    
- OOM
    

---

# 6. Virtual Memory

Each process thinks

```
I own all RAM.
```

Reality

Kernel maps

```
Virtual

↓

Physical RAM
```

---

Benefits

- Isolation
    
- Security
    
- Larger address space
    

---

Commands

```
free -h

vmstat

swapon --show
```

---

# 7. Paging

RAM divided into

```
Pages
```

Usually

```
4 KB
```

Memory mapped using page tables.

---

Benefits

- Efficient allocation
    
- Virtual memory
    
- No external fragmentation
    

---

Commands

```
getconf PAGE_SIZE

cat /proc/PID/maps
```

---

# 8. Segmentation

Memory divided logically.

Example

```
Code

Data

Stack
```

Unlike paging

```
Variable size
```

Mostly historical.

Modern Linux mainly uses paging.

---

# 9. Deadlock

Four conditions

- Mutual Exclusion
    
- Hold and Wait
    
- No Preemption
    
- Circular Wait
    

---

Example

```
Thread A

waiting for Lock B

↓

Thread B

waiting for Lock A
```

---

DevOps

Occurs in

- Databases
    
- Kubernetes controllers
    
- Java apps
    

---

# 10. IPC (Inter Process Communication)

Processes communicate using

- Pipes
    
- Named Pipes
    
- Shared Memory
    
- Message Queue
    
- Socket
    
- Signals
    

---

Commands

```
ipcs

ipcrm

kill -SIGTERM PID
```

---

Real Examples

Docker

↓

Unix Socket

```
/var/run/docker.sock
```

Nginx

↓

Socket

↓

Backend

---

# 11. Synchronization

Multiple threads access same data.

Need synchronization.

Methods

- Mutex
    
- Semaphore
    
- Spinlock
    
- Read Write Lock
    

---

Without synchronization

↓

Race Condition

---

Example

```
Thread A

Counter++

Thread B

Counter++
```

Result may become

```
101

instead of

102
```

---

# 12. File System

Linux stores everything as files.

Common File Systems

- ext4
    
- XFS
    
- Btrfs
    
- ZFS
    

---

Commands

```
df -h

du -sh *

lsblk

mount

umount

findmnt
```

---

Useful Directories

```
/

├── /etc
├── /home
├── /var
├── /usr
├── /tmp
├── /dev
├── /proc
├── /sys
```

---

## DevOps Focus

Know how to

- Mount disks
    
- Check storage
    
- Expand volumes
    
- Find disk usage
    
- Manage permissions
    

---

# Quick Interview Revision

|Topic|Remember|
|---|---|
|Process|Running program with its own memory and PID|
|Thread|Lightweight execution unit sharing process resources|
|Scheduling|CPU decides which process/thread runs next (Linux: CFS)|
|Context Switch|CPU saves one task's state and loads another's|
|Memory|RAM used by processes (stack, heap, code, data)|
|Virtual Memory|Each process gets its own virtual address space mapped to physical RAM|
|Paging|Fixed-size pages map virtual memory to physical memory|
|Segmentation|Variable-size logical memory sections (mostly historical in modern Linux)|
|Deadlock|Processes wait forever for each other's resources|
|IPC|Processes communicate using pipes, sockets, shared memory, signals, etc.|
|Synchronization|Prevent race conditions using mutexes, semaphores, locks|
|File System|Organizes and stores files (ext4, XFS, Btrfs, ZFS)|

---

## Most Important Linux Commands to Memorize

```bash
ps aux
top
htop
kill
kill -9
free -h
vmstat
df -h
du -sh
lsblk
mount
umount
findmnt
ipcs
swapon --show
cat /proc/meminfo
cat /proc/<PID>/maps
getconf PAGE_SIZE
nice
renice
chrt
```

If you're preparing specifically for **DevOps/System Admin/SRE interviews**, these 12 topics form the OS foundation. The next high-value topics to study are **systemd**, **signals**, **cgroups**, **namespaces**, **networking**, **permissions/ACLs**, **system logging (journald/rsyslog)**, and **containers (Docker internals)**, as they come up very frequently.