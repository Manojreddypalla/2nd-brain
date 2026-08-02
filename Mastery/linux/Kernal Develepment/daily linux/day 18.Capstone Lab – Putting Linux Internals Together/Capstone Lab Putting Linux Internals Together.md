Yep — Day 18 is the **integration day**. The point isn’t to memorize another Linux feature; it’s to finally see that processes, `/proc`, memory, files, namespaces, cgroups, capabilities, syscalls, systemd, and networking are all different views of the **same kernel-managed object**.

Below is a full Obsidian-ready note.

# 🐧 Linux Internals — Day 18

## Capstone Lab: Putting Linux Internals Together

---

# 1. The Big Picture

When you execute:

```bash
./app
```

it looks like you're simply "running a program."

Internally, Linux has to manage much more:

```text
                       Running Process
                             │
          ┌──────────────────┼──────────────────┐
          │                  │                  │
       Identity           Resources          Access
          │                  │                  │
       PID/PPID            Memory          Permissions
          │                cgroups         Capabilities
          │                  │              Seccomp
          │                  │
          ├──────────────┬───┴───────────────┐
          │              │                   │
     Namespaces      File Descriptors     Networking
          │              │                   │
          └──────────────┼───────────────────┘
                         │
                    System Calls
                         │
                    Linux Kernel
                         │
          ┌──────────────┼──────────────┐
          │              │              │
         CPU           Memory         Devices
```

A **process is therefore not just a PID**.

It has:

- an identity
    
- a parent
    
- virtual memory
    
- open files
    
- credentials
    
- namespaces
    
- cgroup membership
    
- capabilities
    
- system-call access
    
- network resources
    
- CPU scheduling state
    

The kernel connects all of these pieces.

---

# 2. Program vs Process

This distinction is extremely important.

## Program

A **program** is passive.

Example:

```text
/usr/bin/bash
```

It's basically executable code stored on disk.

## Process

A **process** is a running instance of that program.

```text
bash executable
      │
      │ exec
      ▼
+----------------------+
| bash process         |
| PID: 4201            |
| Memory               |
| Stack                |
| Heap                 |
| Open files           |
| Credentials          |
| Namespaces           |
| cgroup               |
| File descriptors     |
+----------------------+
```

You could run Bash five times.

```text
/usr/bin/bash
     │
 ┌───┼────┬────┐
 ▼   ▼    ▼    ▼
PID PID  PID  PID
101 205  399  812
```

Same program.

Different processes.

---

# 3. The Process Mental Model

Think of a process like an employee.

|Linux concept|Mental model|
|---|---|
|PID|Employee ID|
|PPID|Manager/parent|
|Namespace|Environment/office|
|cgroup|Resource budget|
|Capability|Special keys|
|Seccomp|Allowed operations|
|Virtual memory|Private workspace|
|File descriptors|Open documents/connections|
|System calls|Requests to management|
|Kernel|Management|
|Scheduler|Decides when employee works|
|systemd|Service manager|
|`/proc`|Employee information dashboard|

This model becomes especially useful when learning **Docker/Kubernetes**, because containers mostly manipulate these exact Linux mechanisms.

---

# 4. Choose a Process

We'll investigate your current shell.

Run:

```bash
echo $$
```

### Breaking down the command

`echo`

Print something to the terminal.

`$$`

Shell variable representing the **PID of the current shell**.

Example:

```text
4217
```

So:

```text
bash
PID = 4217
```

For the rest of the notes:

```text
<PID> = 4217
```

Replace it with your actual PID.

---

# 5. Process Identity

Run:

```bash
ps -fp <PID>
```

Example:

```bash
ps -fp 4217
```

Possible output:

```text
UID        PID   PPID  C STIME TTY      TIME CMD
retr0     4217   4001  0 20:10 pts/0  00:00:00 bash
```

## Command breakdown

`ps`

**Process Status**

Displays information about running processes.

### `-f`

Full-format listing.

Shows extra information such as:

```text
UID
PID
PPID
CMD
```

### `-p`

Select a specific PID.

---

# 6. PID and PPID

Two important values:

```text
PID  = Process ID
PPID = Parent Process ID
```

Processes usually form a hierarchy.

```text
systemd
PID 1
 │
 └── terminal
       │
       └── bash
            │
            ├── ls
            ├── cat
            └── grep
```

If Bash starts:

```bash
ls
```

conceptually:

```text
bash
 │
 └── ls
```

The `ls` process has Bash as its parent.

You can visualize process relationships with:

```bash
pstree -p
```

---

# 7. `/proc` — The Process Window

Now comes one of the most important Linux concepts.

Look at:

```bash
ls /proc/<PID>
```

Example:

```bash
ls /proc/4217
```

You'll see files/directories such as:

```text
cmdline
cwd
environ
exe
fd
limits
maps
mounts
ns
status
task
```

This directory exposes information the **kernel maintains about the process**.

---

# 8. Why `/proc` Is a Virtual Filesystem

`/proc` looks like normal files:

```bash
cat /proc/4217/status
```

But most of these files aren't ordinary files stored on your SSD.

They are generated by the kernel.

Mental model:

```text
User
 │
 │ cat /proc/4217/status
 ▼
VFS
 │
 ▼
procfs
 │
 ▼
Kernel process information
 │
 ▼
Formatted as text
```

So `/proc` acts like an interface into kernel state.

That's why it's called a:

> **Virtual filesystem**

The filesystem interface exists, but the information is largely generated dynamically from kernel state rather than being ordinary persistent files on disk.

---

# 9. Process Status

Run:

```bash
cat /proc/<PID>/status
```

You'll see something similar to:

```text
Name:   bash
State:  S (sleeping)
Pid:    4217
PPid:   4001
Uid:    1000
Gid:    1000
Threads: 1
VmSize: ...
VmRSS: ...
```

Important fields:

### `Name`

Process name.

### `State`

Current process state.

Common states:

```text
R = Running
S = Sleeping
D = Uninterruptible sleep
T = Stopped
Z = Zombie
```

### `Pid`

Process ID.

### `PPid`

Parent PID.

### `Uid/Gid`

Security identity.

### `Threads`

Number of threads belonging to the process.

### `VmSize`

Total virtual address space.

### `VmRSS`

Physical memory currently resident in RAM.

---

# 10. Process Command Line

Run:

```bash
cat /proc/<PID>/cmdline
```

This exposes the arguments used to start the process.

However, you may notice ugly output because arguments are separated by **NUL bytes**, not normal spaces.

A more readable version:

```bash
tr '\0' ' ' < /proc/<PID>/cmdline
```

Example:

```text
bash
```

For another application you might see:

```text
python server.py --port 8080
```

---

# 11. Executable Behind the Process

Run:

```bash
ls -l /proc/<PID>/exe
```

Example:

```text
/proc/4217/exe -> /usr/bin/bash
```

This tells you:

```text
Process 4217
      │
      ▼
Executable
      │
      ▼
/usr/bin/bash
```

Useful when you see a mysterious process and want to know **which binary is actually executing**.

---

# 12. Current Working Directory

Run:

```bash
ls -l /proc/<PID>/cwd
```

Example:

```text
/proc/4217/cwd -> /home/retr0/projects
```

`cwd` means:

> Current Working Directory

Every process has one.

That's why:

```bash
pwd
```

has meaning.

Your shell maintains a current working directory.

---

# 13. File Descriptors

This is another major connection.

Run:

```bash
ls -l /proc/<PID>/fd
```

You may see:

```text
0 -> /dev/pts/0
1 -> /dev/pts/0
2 -> /dev/pts/0
```

These are **file descriptors**.

```text
0 = stdin
1 = stdout
2 = stderr
```

Mental model:

```text
Process
   │
   ├── FD 0 ──> keyboard/terminal input
   ├── FD 1 ──> terminal output
   ├── FD 2 ──> terminal errors
   ├── FD 3 ──> file
   └── FD 4 ──> socket
```

The process doesn't normally manipulate a file directly.

It works through a small integer:

```text
fd = 3
```

and asks the kernel:

```text
read(3, ...)
write(3, ...)
close(3)
```

This connects:

```text
File Descriptors
       ↓
System Calls
       ↓
Kernel
       ↓
Filesystem / Socket / Device
```

---

# 14. Inspect Open Files with `lsof`

Run:

```bash
lsof -p <PID>
```

`lsof` means:

> **List Open Files**

`-p`

Select a process by PID.

You may see:

```text
COMMAND PID USER FD TYPE DEVICE NAME
bash    4217 ... cwd DIR  ...   /home/retr0
bash    4217 ... txt REG  ...   /usr/bin/bash
```

Linux uses the file abstraction heavily.

A process may have descriptors referring to:

```text
regular files
directories
pipes
sockets
devices
terminals
```

Hence the famous Unix idea:

> "Everything is a file."

More precisely: **many kernel resources are exposed through file-like interfaces/file descriptors.**

---

# 15. Virtual Memory

Now inspect:

```bash
cat /proc/<PID>/maps | head
```

This displays the process's **virtual memory mappings**.

Example conceptually:

```text
555555554000-55555557d000 r--p /usr/bin/bash
55555557d000-55555563e000 r-xp /usr/bin/bash
...
7ffff7...                 libc.so
...
[heap]
...
[stack]
```

The process sees a virtual address space.

```text
High Address
┌──────────────────────┐
│ Stack                │
├──────────────────────┤
│ Memory mappings      │
│ Shared libraries     │
├──────────────────────┤
│ Heap                 │
├──────────────────────┤
│ Data / BSS           │
├──────────────────────┤
│ Program code         │
└──────────────────────┘
Low Address
```

---

# 16. Why Virtual Memory Matters

The process might think it owns:

```text
0x400000
0x500000
0x600000
...
```

But those are **virtual addresses**.

The kernel + CPU MMU translate them:

```text
Virtual Address
      │
      ▼
Page Tables
      │
      ▼
Physical Address
      │
      ▼
RAM
```

Therefore:

```text
Process A 0x5000 ──> Physical frame 20
Process B 0x5000 ──> Physical frame 91
```

Both processes can use the same virtual address without colliding.

This provides:

- isolation
    
- protection
    
- flexible memory allocation
    
- shared mappings
    
- demand paging
    

---

# 17. Namespaces

Run:

```bash
ls -l /proc/<PID>/ns
```

Example:

```text
cgroup -> cgroup:[4026531835]
ipc    -> ipc:[4026531839]
mnt    -> mnt:[4026531841]
net    -> net:[4026531992]
pid    -> pid:[4026531836]
user   -> user:[4026531837]
uts    -> uts:[4026531838]
```

Namespaces answer:

> **What resources/world can this process see?**

Important namespaces:

|Namespace|Isolates|
|---|---|
|PID|Process IDs|
|NET|Network stack|
|MNT|Mount points|
|UTS|Hostname/domain name|
|IPC|Inter-process communication|
|USER|User/group IDs|
|CGROUP|cgroup view|

Mental model:

```text
Host
│
├── Namespace A
│     ├── Process 1
│     └── Process 2
│
└── Namespace B
      ├── Process 1
      └── Process 2
```

Different processes can experience different views of the same Linux system.

This is one of the foundations of containers.

---

# 18. Namespace Comparison Trick

Suppose you have two processes:

```text
PID 4217
PID 9000
```

Check:

```bash
readlink /proc/4217/ns/net
readlink /proc/9000/ns/net
```

If both return:

```text
net:[4026531992]
```

they belong to the same network namespace.

If the IDs differ:

```text
4217 → net:[4026531992]

9000 → net:[4026533001]
```

they're in different network namespaces.

This is a very practical way of understanding namespaces:

> Processes sharing the same namespace identifier share that namespace's view.

---

# 19. cgroups

Now inspect:

```bash
cat /proc/<PID>/cgroup
```

On modern systems using cgroup v2, you might see:

```text
0::/user.slice/user-1000.slice/session-3.scope
```

Namespaces and cgroups solve different problems.

```text
Namespace
   ↓
What can the process SEE?

cgroup
   ↓
How much can the process USE?
```

cgroups can control/account for resources such as:

```text
CPU
memory
I/O
process counts
```

This distinction is critical for containers.

```text
Container
   │
   ├── Namespaces → isolation
   │
   └── cgroups → resource control/accounting
```

---

# 20. Capabilities

Traditional Unix privilege was roughly:

```text
root
vs
non-root
```

But root has enormous power.

Linux capabilities split many privileged operations into smaller units.

Examples:

```text
CAP_NET_ADMIN
CAP_NET_BIND_SERVICE
CAP_SYS_ADMIN
CAP_CHOWN
CAP_KILL
```

You can inspect capability masks through:

```bash
grep Cap /proc/<PID>/status
```

Possible output:

```text
CapInh:
CapPrm:
CapEff:
CapBnd:
CapAmb:
```

The most useful conceptual field is:

```text
CapEff
```

Effective capabilities currently available to the process.

Capabilities answer:

> **Which privileged kernel operations may this process perform?**

---

# 21. Seccomp

Capabilities control privileged powers.

Seccomp attacks a different layer.

Seccomp can restrict:

> **Which system calls a process may make.**

Conceptually:

```text
Process
   │
   │ openat()
   │ read()
   │ write()
   │ mount()
   ▼
Seccomp Filter
   │
   ├── allowed → Kernel
   │
   └── blocked → deny/kill/etc.
```

Check:

```bash
grep Seccomp /proc/<PID>/status
```

Common modes:

```text
0 = disabled
1 = strict
2 = filter
```

Containers frequently combine:

```text
Namespaces
+
cgroups
+
Capabilities
+
Seccomp
```

for isolation and restriction.

---

# 22. System Calls

A normal user-space process cannot directly control kernel resources however it wants.

Instead:

```text
Application
    │
    │ system call
    ▼
Kernel
    │
    ▼
Hardware / Files / Network
```

Examples:

```text
openat()
read()
write()
close()
mmap()
socket()
connect()
clone()
execve()
```

If Bash needs to write to your terminal:

```text
bash
 │
 │ write()
 ▼
Kernel
 │
 ▼
terminal device
```

System calls are the main controlled interface between:

```text
User Space
    ↕
Kernel Space
```

---

# 23. Trace Syscalls with `strace`

Run:

```bash
strace -p <PID>
```

`strace`

means **system-call trace**.

`-p`

attach to an existing process.

Depending on security settings, you may need:

```bash
sudo strace -p <PID>
```

You might see:

```text
read(...)
write(...)
poll(...)
ioctl(...)
```

Stop tracing with:

```text
Ctrl+C
```

---

# 24. A Better `strace` Experiment

Tracing your shell may be confusing because it could be waiting for input.

Try:

```bash
strace ls
```

You'll see operations such as:

```text
execve(...)
openat(...)
mmap(...)
read(...)
write(...)
close(...)
```

Think about what's happening:

```text
ls
 │
 ├── load executable
 │
 ├── load libraries
 │
 ├── open directory
 │
 ├── read directory entries
 │
 └── write filenames
 ▼
Kernel
```

This is why `strace` is such a powerful debugging tool.

A program saying:

```text
Permission denied
```

might reveal:

```text
openat(...) = -1 EACCES
```

Now you know **which kernel operation actually failed**.

---

# 25. Resource Limits

Run:

```bash
cat /proc/<PID>/limits
```

You may see:

```text
Max cpu time
Max file size
Max stack size
Max open files
Max processes
```

These are per-process resource limits.

For example:

```text
Max open files    1024
```

means the process cannot keep unlimited file descriptors open.

This matters heavily for servers.

Imagine nginx handling thousands of connections:

```text
Connection
    ↓
Socket
    ↓
File Descriptor
```

If its FD limit is too low:

```text
Too many open files
```

---

# 26. Networking Is Also Connected to the Process

A server process might create a socket:

```c
socket(...)
bind(...)
listen(...)
accept(...)
```

Conceptually:

```text
nginx
 │
 │ socket FD 7
 ▼
Kernel networking stack
 │
 ├── TCP
 ├── IP
 └── NIC
```

You can inspect sockets belonging to a process with tools such as:

```bash
lsof -p <PID>
```

or system-wide:

```bash
ss -tulpn
```

Command idea:

```text
ss = socket statistics
```

Common flags:

```text
-t = TCP
-u = UDP
-l = listening
-p = process
-n = numeric addresses/ports
```

---

# 27. systemd Connection

Long-running services are often managed by:

```text
systemd
```

Example:

```bash
systemctl status ssh
```

or:

```bash
systemctl status nginx
```

Conceptually:

```text
systemd
PID 1
 │
 ├── sshd
 ├── nginx
 ├── docker
 └── other services
```

systemd can:

- start processes
    
- stop processes
    
- restart failed services
    
- configure dependencies
    
- collect service state
    
- place services into cgroups
    
- manage logs through journald
    

So systemd isn't the kernel.

It's a **user-space service manager** that heavily uses kernel facilities.

---

# 28. The Complete Journey of a Process

Suppose you run:

```bash
./server
```

Your shell eventually asks the kernel to create/execute the program.

Conceptually:

```text
bash
 │
 │ process creation + exec
 ▼
Linux Kernel
 │
 ├── create/manage process state
 ├── assign PID
 ├── establish memory mappings
 ├── associate credentials
 ├── inherit/set file descriptors
 ├── establish namespace membership
 ├── establish cgroup membership
 └── schedule execution
        │
        ▼
      CPU
```

Then while the server runs:

```text
server
 │
 ├── mmap()    → memory
 ├── openat()  → filesystem
 ├── read()    → files/input
 ├── write()   → files/output
 ├── socket()  → networking
 ├── accept()  → connections
 └── close()   → release resources
        │
        ▼
      Kernel
```

This is the central Linux mental model.

---

# 29. Containers Finally Make Sense

After studying Linux internals, Docker becomes much less magical.

A container is **not a tiny virtual machine**.

Very roughly:

```text
Container Process
      │
      ├── PID namespace
      ├── Mount namespace
      ├── Network namespace
      ├── UTS namespace
      ├── IPC namespace
      │
      ├── cgroups
      │
      ├── capabilities
      │
      └── seccomp
             │
             ▼
         Host Kernel
```

Containers share the host Linux kernel.

Compare:

```text
Virtual Machine

Application
Guest OS
Guest Kernel
Hypervisor
Host Hardware
```

versus:

```text
Container

Application
Namespaces/cgroups/etc.
Host Linux Kernel
Hardware
```

This is why Linux internals knowledge transfers directly into:

```text
Docker
Kubernetes
DevOps
Cloud
SRE
Security
```

---

# 30. Linux Debugging Mental Model

When something breaks, don't randomly try commands.

Ask:

> **Which subsystem is failing?**

A useful investigation flow:

```text
                    Problem
                       │
                       ▼
                  Find Process
                       │
                       ▼
                  PID / PPID
                       │
       ┌───────────────┼──────────────┐
       ▼               ▼              ▼
    Memory           Files         Network
       │               │              │
       ▼               ▼              ▼
 /proc/PID/maps      lsof            ss
       │
       ├───────────────┬──────────────┐
       ▼               ▼              ▼
 Namespaces         cgroups       Permissions
       │               │              │
       └───────────────┼──────────────┘
                       ▼
                    strace
                       │
                       ▼
                What syscall fails?
```

This is much closer to how Linux debugging is actually approached.

---

# 31. Capstone Lab

Pick your Bash PID:

```bash
PID=$$
```

Now investigate it.

### Identity

```bash
ps -fp $PID
```

### Kernel process information

```bash
cat /proc/$PID/status
```

### Command

```bash
tr '\0' ' ' < /proc/$PID/cmdline
```

### Executable

```bash
ls -l /proc/$PID/exe
```

### Working directory

```bash
ls -l /proc/$PID/cwd
```

### File descriptors

```bash
ls -l /proc/$PID/fd
```

### Open files

```bash
lsof -p $PID
```

### Memory mappings

```bash
cat /proc/$PID/maps | head -20
```

### Namespaces

```bash
ls -l /proc/$PID/ns
```

### cgroup

```bash
cat /proc/$PID/cgroup
```

### Capabilities

```bash
grep Cap /proc/$PID/status
```

### Seccomp

```bash
grep Seccomp /proc/$PID/status
```

### Limits

```bash
cat /proc/$PID/limits
```

### Syscalls

```bash
sudo strace -p $PID
```

---

# 32. What You Should Be Able to Explain

## Where is process information stored/exposed?

Primarily through:

```text
/proc/<PID>/
```

Examples:

```text
/proc/PID/status
/proc/PID/maps
/proc/PID/fd
/proc/PID/ns
/proc/PID/cgroup
/proc/PID/limits
```

---

## Why is `/proc` virtual?

Because the information is largely generated dynamically from **kernel state**, rather than being ordinary persistent data stored on disk.

```text
/proc
 ↓
procfs
 ↓
Kernel state
```

---

## Namespace vs cgroup

Remember this forever:

```text
Namespace = what can I SEE?

cgroup = how much can I USE?
```

---

## Capabilities vs Seccomp

```text
Capabilities
      ↓
Which privileged operations am I authorized for?

Seccomp
      ↓
Which system calls am I allowed to make?
```

---

## What does `strace` do?

It exposes interactions across the boundary:

```text
Process
   ↓
System Calls
   ↓
Kernel
```

So it helps answer:

> **What is the program asking the kernel to do, and what is the kernel returning?**

---

# 33. The Four Isolation/Security Concepts

Don't mix these up.

|Mechanism|Main question|
|---|---|
|Namespace|What can you see?|
|cgroup|How much can you use?|
|Capability|What privileged powers do you have?|
|Seccomp|Which syscalls can you make?|

Together:

```text
Process
 │
 ├── Namespace → visibility/isolation
 ├── cgroup → resources
 ├── Capability → privilege
 └── Seccomp → syscall filtering
```

This is one of the most important conceptual groups from the entire roadmap.

---

# 34. One Process, Multiple Views

The same PID can be investigated from completely different perspectives.

```text
                         PID 4217
                            │
        ┌───────────────────┼───────────────────┐
        │                   │                   │
    /proc/4217           ps/lsof             strace
        │                   │                   │
        ▼                   ▼                   ▼
 Kernel state          Resources           Syscalls
        │
 ┌──────┼───────┬────────┬─────────┐
 ▼      ▼       ▼        ▼         ▼
Memory Files   NS      cgroup   Security
```

That's the entire lesson.

These topics weren't separate chapters.

They're **different dimensions of one running process**.

---

# 🧠 Final Mental Model

When you see:

```text
nginx
python
node
bash
docker
postgres
```

don't think:

> "That's a program running."

Think:

```text
                    PROCESS
                       │
          ┌────────────┼────────────┐
          │            │            │
        PID          Memory       Files
          │            │            │
       Parent       VM/maps       FDs
          │
    ┌─────┼────────┬─────────┐
    ▼     ▼        ▼         ▼
Namespace cgroup Capability Seccomp
    │
    └──────────────┬──────────┘
                   ▼
              System Calls
                   │
                   ▼
              Linux Kernel
                   │
        ┌──────────┼──────────┐
        ▼          ▼          ▼
       CPU        RAM       Devices
                              │
                           Network
```

The **kernel is the central authority**.

The process lives in user space and requests kernel services through **system calls**. The kernel schedules its CPU time, maps its memory, tracks its files, enforces permissions, controls its resources, handles its networking, and exposes much of that state through `/proc`.

---

# ⚡ 2-Minute Revision

```text
Program → executable stored on disk
Process → running instance of a program

PID → process identity
PPID → parent identity

/proc/PID → window into kernel process state

/proc/PID/maps → virtual memory
/proc/PID/fd → file descriptors
/proc/PID/ns → namespaces
/proc/PID/cgroup → cgroup membership
/proc/PID/status → process/security information
/proc/PID/limits → resource limits

Namespace → what can it see?
cgroup → how much can it use?
Capability → what privileged powers does it have?
Seccomp → which syscalls may it make?

System call → controlled request from user space to kernel

strace → observe those requests/results

systemd → manages long-running services

Container ≈ isolated/restricted Linux processes
           using namespaces + cgroups
           + capabilities + seccomp
```

## 🔥 The one sentence to remember

> **A Linux process is a kernel-managed execution context with an identity, virtual memory, open resources, credentials, namespace and cgroup membership, and it interacts with the kernel primarily through system calls.**

Once that sentence makes intuitive sense, you've connected almost everything from the Linux Internals roadmap.