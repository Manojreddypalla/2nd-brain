| Day   | Topic                   | Core Concepts                                                                       | Hands-on Commands                                                                                                     | Goal Achieved                                                                   |
| ----- | ----------------------- | ----------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------- |
| **1** | Linux Architecture      | Kernel, User Space, Kernel Space, Shell, System Calls, `/proc`, `/sys`, `/dev`, FHS | `uname -a`, `cat /etc/os-release`, `ls /`, `ls /proc`, `cat /proc/cpuinfo`, `cat /proc/meminfo`, `ls /dev`, `ls /sys` | Understand how Linux is structured and how applications interact with hardware. |
| **2** | Processes               | Program vs Process, PID, Parent & Child Processes, Process States, Process Tree     | `echo $$`, `ps`, `ps -ef`, `pstree`, `top`                                                                            | Understand what a process is and how Linux manages running programs.            |
| **3** | Memory Management       | Physical Memory, Virtual Memory, Stack, Heap, RAM, Swap                             | `free -h`, `cat /proc/meminfo`, `vmstat 1 5`                                                                          | Understand how Linux manages memory for processes.                              |
| **4** | Filesystem              | Filesystem, Inode, Metadata, Hard Link, Soft Link, Mount                            | `pwd`, `ls -l`, `ls -li`, `df -h`, `du -sh`, `touch`, `ln`, `ln -s`                                                   | Learn how Linux stores files and the role of inodes and links.                  |
| **5** | Users & Permissions     | Users, Groups, UID, GID, `rwx`, Ownership, `sudo`, Permission Model                 | `whoami`, `id`, `groups`, `ls -l`, `touch`, `chmod`                                                                   | Understand how Linux controls access to files and resources.                    |
| **6** | Linux Networking Basics | Network Interface, IP Address, MAC Address, Routing Table, Socket, DNS              | `ip addr`, `ip route`, `hostname -I`, `ss -tuln`, `ping`                                                              | Learn the fundamentals of how Linux communicates over networks.                 |
Yes. This is the right moment to stop treating the daily Linux sessions as an endless chain of interesting topics and put a **destination + checkpoints** around them.

From **August 2 → December 31, 2026**, you have about **151 days / ~21.5 weeks**. That's enough for a serious first pass, but not enough to become a mature kernel maintainer—and that's fine. The December target should be **kernel-development ready**, with at least one real open-source contribution attempt.

The roadmap stays **adaptive**: phases are fixed, exact day counts can move ±1 week based on how you're progressing.

# 🐧 Linux → Kernel Engineering: Aug–Dec 2026

|Phase|Approx. dates|Linux Track|C Track|End milestone|
|---|---|---|---|---|
|**0. Foundation**|Already done|Linux Internals Days 1–32|—|✅ Strong Linux conceptual base|
|**1. Go Deeper**|Aug 3–23|Deeper Linux internals|Systems C foundations|Understand C memory + deeper Linux|
|**2. Merge**|Aug 24–Sep 20|Linux APIs/internals|Systems programming|Build Linux utilities in C|
|**3. OS → Kernel**|Sep 21–Oct 18|Kernel architecture|Kernel-oriented C|Read Linux kernel source|
|**4. Kernel Development**|Oct 19–Nov 15|Modules + kernel APIs|Kernel C|Build/boot/modify kernel|
|**5. Drivers + Hardware**|Nov 16–Dec 6|Device model/drivers|MMIO/bit-level C|Write simple driver/module|
|**6. Contribution**|Dec 7–31|Real kernel subsystem|Patch workflow|Submit genuine OSS patch|

The **dates are guidance, not prison bars**.

---

# Phase 1 — Deeper Linux + Systems C

### August 3 → August 23

Two parallel tracks.

### 🐧 Linux

Continue from **Day 33**.

Go deeper into:

- VFS concepts
    
- storage/block I/O
    
- page cache
    
- `/proc`
    
- `/sys`
    
- `sysfs`
    
- `debugfs`
    
- process internals
    
- scheduling
    
- advanced virtual memory
    
- page tables
    
- networking internals
    
- sockets
    
- TCP lifecycle
    
- routing
    
- netfilter/nftables concepts
    
- kernel boot parameters
    
- kernel modules deeper
    
- tracing/observability
    

### ⚙️ C — Day 1 starts here

Learn C specifically for systems work:

```text
C syntax
 ↓
Memory representation
 ↓
Pointers
 ↓
Pointer arithmetic
 ↓
Arrays
 ↓
Strings
 ↓
Structs
 ↓
Unions
 ↓
Enums
 ↓
Bitwise operations
 ↓
Stack vs Heap
 ↓
malloc / calloc / realloc / free
 ↓
Function pointers
 ↓
const / static / volatile
 ↓
Preprocessor
 ↓
Headers
```

But every concept gets connected to memory.

Example:

```c
struct process {
    int pid;
    char name[32];
};
```

We don't stop at "this is a struct."

We ask:

```text
Where is it stored?

How many bytes?

What is padding?

What does a pointer to it contain?

What does the CPU actually access?
```

### 🎯 Phase 1 Exit Test

Before moving forward, you should be able to explain:

```text
pointer
stack
heap
struct
memory address
virtual memory
process
file descriptor
system call
```

and write small C programs without blindly copying code.

If not → **extend Phase 1**.

---

# Phase 2 — Linux Systems Programming

### August 24 → September 20

This is **Merge #1**.

Linux and C stop being separate subjects.

```text
C
+
Linux
========
Systems Programming
```

Study the Linux API from C.

### Files

```c
open()
read()
write()
close()
lseek()
stat()
```

Build:

**mini `cp`**

then:

**mini `cat`**

then:

**mini `ls`**

---

### Processes

```c
fork()
exec()
wait()
_exit()
```

Build:

**tiny shell**

You'll finally understand:

```text
bash
 ↓
fork
 ↓
child
 ↓
execve
 ↓
program
```

---

### Signals

```c
signal()
sigaction()
kill()
```

---

### IPC

```text
pipes
FIFO
shared memory
semaphores
UNIX sockets
```

---

### Memory

```c
mmap()
munmap()
```

Investigate:

```text
/proc/<PID>/maps
```

---

### Networking

```c
socket()
bind()
listen()
accept()
connect()
send()
recv()
```

Eventually:

```text
TCP server
   ↓
multiple clients
   ↓
epoll
```

### 🎯 Phase 2 Projects

By the end:

```text
mini-cat
mini-cp
mini-ls
mini-ps
tiny-shell
TCP server
```

These should go into Git.

### Phase 2 Exit Test

You should be capable of seeing:

```c
read(fd, buffer, 1024);
```

and mentally understanding:

```text
userspace
   ↓
system call
   ↓
kernel
   ↓
VFS
   ↓
filesystem
   ↓
storage
```

That's a major milestone.

---

# Phase 3 — Crossing Into the Kernel

### September 21 → October 18

Now we stop treating Linux as a black box.

Get the Linux source.

Learn the source tree:

```text
arch/
block/
drivers/
fs/
include/
kernel/
mm/
net/
security/
```

Don't try to "read Linux."

Learn to **navigate Linux**.

---

### Processes

Meet:

```c
struct task_struct
```

Connect:

```text
PID
PPID
scheduler
process state
signals
credentials
```

to the kernel implementation.

---

### Memory

Study:

```text
mm_struct
vm_area_struct
pages
page tables
buddy allocator
SLAB/SLUB
```

---

### Filesystem

Study:

```text
VFS

inode
dentry
file
superblock
file_operations
```

Now:

```c
open("/tmp/a")
```

becomes:

```text
open()
 ↓
syscall
 ↓
VFS
 ↓
path lookup
 ↓
dentry
 ↓
inode
 ↓
filesystem
```

---

### Networking

Start understanding:

```text
socket layer
sk_buff
network devices
TCP/IP stack
```

Not every implementation detail yet.

### 🎯 Phase 3 Exit Test

I should be able to give you:

```text
"Find where Linux represents a process."
```

and you can navigate toward `task_struct`.

Or:

```text
"Find the abstraction used for an open file."
```

and kernel source doesn't look like alien hieroglyphics anymore.

---

# Phase 4 — Kernel Development

### October 19 → November 15

Now things get serious.

Set up:

```text
Linux source
+
QEMU
+
GDB
+
Git
```

Your laptop's normal kernel stays safe.

Experiment inside QEMU.

Learn:

```text
Kernel configuration
Kconfig
Kernel Makefiles
Compilation
Boot process
initramfs
Kernel command line
```

Then modules:

```c
module_init()
module_exit()
printk()
```

Continue into:

```text
Kernel memory allocation

kmalloc
kfree
vmalloc
```

Synchronization:

```text
mutex
spinlock
atomic operations
RCU concepts
```

Execution mechanisms:

```text
interrupts
softirqs
workqueues
timers
```

Debugging:

```text
dmesg
GDB
ftrace
perf
debugfs
```

### 🎯 Phase 4 Project

Build a small kernel module that:

- loads
    
- creates observable kernel behavior
    
- exposes useful information
    
- unloads cleanly
    

Not merely:

```text
Hello kernel
```

### Phase 4 Exit Test

You should be comfortable doing:

```text
modify kernel/module
      ↓
compile
      ↓
boot/test
      ↓
observe
      ↓
debug
```

without being terrified that you've summoned Linus Torvalds into your terminal.

---

# Phase 5 — Drivers + Hardware

### November 16 → December 6

Now Linux meets electronics.

Learn the Linux device model:

```text
Hardware
   ↓
Bus
   ↓
Driver
   ↓
Kernel
   ↓
Device file
   ↓
Userspace
```

Start with **character devices**.

Learn:

```text
major/minor numbers
/dev
sysfs
udev
file_operations
ioctl
```

Then hardware concepts:

```text
MMIO
interrupts
DMA
```

Protocols:

```text
GPIO
UART
I²C
SPI
USB
PCI/PCIe
```

This is also where the embedded/Flipper-type interest begins connecting naturally.

### 🎯 Phase 5 Project

Something like:

```text
Userspace program
       ↓
/dev/mydevice
       ↓
Your driver
       ↓
Kernel
```

Initially this can be virtualized; physical hardware isn't required.

### Phase 5 Exit Test

Given:

```c
read(fd, buf, size);
```

you should understand how a driver can eventually receive that operation through its kernel callbacks.

---

# Phase 6 — Open Source

### December 7 → December 31

Now **stop studying broadly**.

Pick one subsystem.

Possible directions:

```text
drivers/
fs/
net/
mm/
kernel/
security/
tools/
documentation/
```

Your first contribution doesn't need to rewrite the scheduler.

In fact, please don't rewrite the scheduler on Christmas Eve. 😭

Start with something legitimate and bounded:

- warning fix
    
- cleanup
    
- documentation correction
    
- error handling
    
- small bug
    
- test improvement
    
- tooling improvement
    

Learn the real workflow:

```text
git clone
 ↓
branch
 ↓
modify
 ↓
build
 ↓
test
 ↓
checkpatch.pl
 ↓
git diff
 ↓
git format-patch
 ↓
MAINTAINERS
 ↓
review process
```

And importantly:

**learn to read existing patches and review discussions.**

---

# 🎯 December 31 Target

I'm deliberately **not** setting:

> "Become a Linux kernel expert."

Five months isn't realistic for that.

Instead, December 31 means you can:

### Linux

Understand deeply:

```text
processes
virtual memory
filesystem/VFS
networking
IPC
scheduling
namespaces
cgroups
security primitives
system calls
boot
modules
devices
```

### C

Comfortably use:

```text
pointers
structs
memory
bit manipulation
function pointers
manual allocation
Linux APIs
```

### Systems

Build:

```text
CLI utilities
process tools
shell
IPC programs
network servers
```

### Kernel

Be capable of:

```text
navigating source
building kernel
booting QEMU
writing modules
reading kernel structures
debugging basic kernel code
understanding driver architecture
```

### Open source

And ideally:

> **Have at least one real patch submitted to an upstream/open-source Linux-related project.**

Acceptance isn't under your control.

**Submitting a technically sound patch and handling review is the milestone.**

---

# 🧭 How we'll keep the plan flexible

This is important.

We don't advance because:

> "It's September 21."

We advance because you pass the **phase exit condition**.

Every ~7 days we'll effectively evaluate:

```text
UNDERSTAND?
    │
    ├── No → reinforce
    │
    └── Yes
         │
CAN BUILD IT?
    │
    ├── No → labs
    │
    └── Yes
         │
CAN DEBUG IT?
    │
    ├── No → experiments
    │
    └── Yes
         ↓
      ADVANCE
```

If C pointers need an extra week → take it.

If Linux networking is already strong → compress it.

If kernel memory becomes fascinating → spend longer there.

**The phase objectives stay fixed. The route inside them can change.**

---

## Your daily format

Don't turn this into another 6-hour course.

For the existing Linux reminder:

```text
🐧 Linux
30–60 min

10–15m theory
20–30m experiment
5–10m notes
```

Add:

```text
⚙️ Systems C
30–45 min

10m concept
20–30m coding
5m memory/debugging inspection
```

After **Merge #1**, those become mostly **one 60–90 minute systems session**, because you'll be learning Linux _by writing C_.

And later:

```text
Linux + C
    ↓
Systems Programming
    ↓
Kernel
```

so we don't maintain artificial separate tracks forever.

This gives us structure through **December 31**, while leaving enough freedom to change the individual daily topics as we discover where you're strong, where you're struggling, and which kernel subsystem pulls you in most.