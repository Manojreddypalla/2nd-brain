# 🐧 Linux Internals – Day 22

## Linux Process Memory – `/proc` Deep Dive

> [!goal] Goal  
> Understand how Linux exposes a running process's **state, memory, file descriptors, limits, environment, and execution information** through `/proc`.

---

# 1. What is `/proc`?

`/proc` is a **virtual filesystem** created and maintained by the Linux kernel.

- It does **not** contain normal files stored on disk.
    
- Information is generated dynamically by the kernel.
    
- It exposes information about:
    
    - Processes
        
    - Memory
        
    - CPU
        
    - Kernel
        
    - Devices
        
    - System configuration
        

### Mental Model

```text
Kernel
  │
  │ exposes information
  ▼
/proc
  │
  ├── 1/
  ├── 245/
  ├── 1024/
  └── 5678/
       │
       ├── status
       ├── maps
       ├── fd/
       ├── cmdline
       ├── environ
       └── limits
```

Each numeric directory represents a running process:

```text
/proc/<PID>
```

Example:

```bash
/proc/1234
```

---

# 2. `$$` — Current Shell PID

```bash
echo $$
```

`$$` expands to the **PID of the current shell**.

Example:

```text
$ echo $$
12345
```

Therefore:

```bash
/proc/$$
```

means:

> `/proc/<current-shell-PID>`

Explore it:

```bash
ls /proc/$$
```

---

# 3. Process Status

```bash
cat /proc/$$/status
```

Contains human-readable process information.

Important fields:

```text
Name:       bash
State:      S (sleeping)
Pid:        12345
PPid:       12000
Threads:    1
VmSize:     ...
VmRSS:      ...
```

### Important Memory Fields

**VmSize**

Total **virtual address space** used by the process.

**VmRSS**

Resident Set Size — memory pages currently present in **physical RAM**.

Mental model:

```text
VmSize
└── Everything mapped into virtual memory

VmRSS
└── Part currently resident in RAM
```

So:

```text
VmRSS ≤ VmSize
```

---

# 4. Process Command Line

```bash
cat /proc/$$/cmdline
```

Contains the arguments used to start the process.

Internally, arguments are separated using **null bytes (`\0`)**, not spaces.

For easier viewing:

```bash
tr '\0' ' ' < /proc/$$/cmdline
```

---

# 5. File Descriptors

```bash
ls -l /proc/$$/fd
```

`fd` contains symbolic links representing the process's **open file descriptors**.

Standard descriptors:

```text
0 → stdin
1 → stdout
2 → stderr
```

Mental model:

```text
Process
   │
   ├── FD 0 ──► Input
   ├── FD 1 ──► Output
   ├── FD 2 ──► Error Output
   ├── FD 3 ──► File
   └── FD 4 ──► Socket
```

A file descriptor is essentially a **small integer used by a process to refer to an open kernel-managed resource**.

Resources can include:

- Files
    
- Pipes
    
- Sockets
    
- Terminals
    
- Devices
    

---

# 6. Process Memory Map

```bash
cat /proc/$$/maps
```

For shorter output:

```bash
cat /proc/$$/maps | head
```

`maps` shows the process's **virtual memory regions**.

Typical layout:

```text
High Address
┌──────────────────┐
│      Stack       │
├──────────────────┤
│ Shared Libraries │
├──────────────────┤
│      Heap        │
├──────────────────┤
│       BSS        │
├──────────────────┤
│       Data       │
├──────────────────┤
│       Code       │
└──────────────────┘
Low Address
```

You may see:

```text
[heap]
[stack]
libc.so
bash
```

Each line describes a **virtual memory mapping**.

Example:

```text
55a0...-55a1... r-xp ... /usr/bin/bash
```

Important permissions:

```text
r = Read
w = Write
x = Execute
p = Private
s = Shared
```

---

# 7. Environment Variables

```bash
cat /proc/$$/environ
```

Contains the process's environment variables.

Like `cmdline`, entries are separated using null bytes.

Better:

```bash
tr '\0' '\n' < /proc/$$/environ
```

Example:

```text
HOME=/home/user
PATH=/usr/bin:/bin
SHELL=/bin/bash
```

---

# 8. Process Limits

```bash
cat /proc/$$/limits
```

Shows resource limits applied to the process.

Examples:

- Maximum open files
    
- Maximum processes
    
- Stack size
    
- Locked memory
    
- CPU time
    

These are related to:

```bash
ulimit
```

---

# 9. Mini Experiment

Start a background process:

```bash
sleep 60 &
```

Find its PID:

```bash
jobs -l
```

Suppose:

```text
[1] 15000 Running sleep 60 &
```

Inspect it:

```bash
cat /proc/15000/status
```

```bash
ls -l /proc/15000/fd
```

```bash
cat /proc/15000/maps | head
```

Compare it with:

```bash
/proc/$$
```

### What to Notice

Your shell is a much more complex process than `sleep`.

Therefore their:

- Memory mappings
    
- Open files
    
- Memory usage
    
- Process state
    

can differ significantly.

---

# 🧠 Big Picture

```text
Program
   │
   ▼
Running Process
   │
   ├── PID
   ├── Virtual Memory
   ├── File Descriptors
   ├── Environment
   ├── Resource Limits
   └── Execution State
           │
           ▼
       Linux Kernel
           │
           ▼
      /proc/<PID>
```

`/proc/<PID>` acts like a **live window into the kernel's view of a process**.

---

# 🔗 Connection to Previous Topics

### Processes

Every running process has:

```text
PID → /proc/<PID>
```

### Memory

```text
/proc/<PID>/maps
```

shows the process's virtual memory mappings.

### File Descriptors

```text
/proc/<PID>/fd
```

shows resources currently opened by the process.

### System Calls

Processes interact with kernel resources through system calls such as:

```text
open()
read()
write()
mmap()
close()
```

The resulting process state can often be inspected through `/proc`.

### Debugging

`/proc` helps answer questions like:

```text
What is this process doing?
What files does it have open?
How much memory is it using?
What libraries are mapped?
What limits does it have?
```

---

# ⚡ Important `/proc/<PID>` Files

|Path|Purpose|
|---|---|
|`/proc/<PID>/status`|Process metadata|
|`/proc/<PID>/cmdline`|Startup command + arguments|
|`/proc/<PID>/fd/`|Open file descriptors|
|`/proc/<PID>/maps`|Virtual memory mappings|
|`/proc/<PID>/environ`|Environment variables|
|`/proc/<PID>/limits`|Resource limits|

---

# 🎯 Interview / Revision Questions

**Why is `/proc` called a virtual filesystem?**

Because its contents are generated dynamically by the kernel rather than being normal files stored on disk.

**What is `/proc/<PID>`?**

A directory exposing kernel information about a specific running process.

**What are file descriptors 0, 1 and 2?**

```text
0 → stdin
1 → stdout
2 → stderr
```

**What does `/proc/<PID>/maps` show?**

The process's **virtual memory mappings**, including executable regions, libraries, heap and stack.

**What is the difference between VmSize and VmRSS?**

```text
VmSize → Total virtual address space
VmRSS  → Portion currently resident in RAM
```

---

# 💻 Commands Cheat Sheet

```bash
# Current shell PID
echo $$

# Explore process
ls /proc/$$

# Process metadata
cat /proc/$$/status

# Command line
cat /proc/$$/cmdline

# Open file descriptors
ls -l /proc/$$/fd

# Memory mappings
cat /proc/$$/maps | head

# Environment
tr '\0' '\n' < /proc/$$/environ

# Resource limits
cat /proc/$$/limits

# Background process experiment
sleep 60 &
jobs -l
```

---

# 🔥 One-Line Memory

> `/proc/<PID>` is a **live interface into the Linux kernel's view of a running process**, exposing its state, memory mappings, open resources, environment, and limits.