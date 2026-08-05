# 🐧 Linux Internals — Day 29: `strace`

> 🎯 **Core idea:** `strace` lets you see the **system calls a process makes to the Linux kernel**.

---

## 1. Why Does `strace` Exist?

Your application runs in **user space**.

It cannot directly do privileged operations like accessing devices or asking the kernel to perform filesystem/network operations.

Instead:

```text
Application
     │
     │ System Calls
     ▼
Linux Kernel
     │
     ├── Filesystem
     ├── Memory
     ├── Network
     └── Processes
```

For example:

```c
printf("Hello");
```

Eventually causes output through something like:

```text
Your Program
     ↓
printf()
     ↓
write()          ← system call
     ↓
Kernel
     ↓
Terminal
```

`strace` lets you **observe those system calls**.

---

# 2. Mental Model

Without `strace`:

```text
Program
   │
   │ ???
   ▼
Kernel
```

With `strace`:

```text
Program
   │
   │ openat()
   │ read()
   │ write()
   │ mmap()
   │ ...
   ▼
Kernel

👀 strace records these interactions
```

So when a program behaves strangely, `strace` helps answer:

> **"What is this program actually asking Linux to do?"**

---

# 3. Simple Example — `cat`

Run:

```bash
strace cat hello.txt
```

You might eventually see calls conceptually like:

```text
openat(..., "hello.txt", ...) = 3

read(3, ...)                  = 12

write(1, ...)                 = 12

close(3)                      = 0
```

Don't get scared by all the other output.

Focus on the story:

```text
cat
 │
 ├── openat() → Open hello.txt
 │
 ├── read()   → Read its contents
 │
 ├── write()  → Write contents to terminal
 │
 └── close()  → Close file
```

That's `cat` exposed at the kernel boundary.

---

# 4. Understanding File Descriptors

Remember:

```text
0 → stdin
1 → stdout
2 → stderr
```

When you see:

```text
openat(...) = 3
```

Linux is basically saying:

> "File successfully opened. Here's FD **3**."

Then:

```text
read(3, ...)
```

means:

> Read from that opened file.

And:

```text
write(1, ...)
```

means:

> Write to **stdout**.

Finally:

```text
close(3)
```

releases that FD.

So you can literally follow:

```text
openat() = 3
     ↓
   FD 3
     ↓
read(3)
     ↓
write(1)
     ↓
close(3)
```

This is one of the most useful ways to read `strace`.

---

# 5. Why Does `strace ls` Show SO MUCH Stuff?

Run:

```bash
strace ls
```

You might expect:

```text
read directory
print files
```

Instead you get a wall of text. 😭

That's because before `ls` can even do its main job, Linux/runtime setup involves things like:

```text
execve()
   ↓
Load executable
   ↓
openat()
   ↓
Load libraries/config/resources
   ↓
mmap()
   ↓
Map memory
   ↓
...
   ↓
Read directory
   ↓
write()
   ↓
Exit
```

A seemingly simple program still interacts with the OS a lot.

---

# 6. Important System Calls

You don't need to memorize hundreds.

Know these:

|System Call|Think|
|---|---|
|`openat()`|Open file/path|
|`read()`|Read data|
|`write()`|Write data|
|`close()`|Close FD|
|`execve()`|Execute program|
|`mmap()`|Create memory mapping|
|`brk()`|Adjust heap boundary|
|`clone()`|Create task/process/thread depending on flags|
|`socket()`|Create network socket|
|`connect()`|Connect socket|
|`sendto()`|Send network data|
|`recvmsg()`|Receive message/data|
|`exit_group()`|Terminate process/thread group|

Recognizing these already lets you understand a lot of traces.

---

# 7. `execve()` — Starting the Program

When you run:

```bash
ls
```

you'll often see:

```text
execve("/usr/bin/ls", ...)
```

Conceptually:

```text
Shell
  │
  │ start ls
  ▼
execve()
  │
  ▼
Kernel
  │
  ▼
Load/execute ls
```

So:

> **`execve()` replaces the current process image with a new program.**

---

# 8. `mmap()` — Memory Mapping

You learned this yesterday.

You may see:

```text
mmap(...)
```

It's used to create virtual-memory mappings, including mappings for things such as shared libraries.

Connect Day 28 → Day 29:

```text
strace
  ↓
mmap()
  ↓
Virtual Memory Mapping
  ↓
Pages
  ↓
RAM as pages become resident
```

Now you're actually seeing memory-related system calls happen.

---

# 9. Network Programs

Suppose an application connects somewhere.

Conceptually:

```text
Application
    │
    ├── socket()
    │
    ├── connect()
    │
    ├── send/recv operations
    │
    └── close()
    ▼
Kernel Network Stack
```

Try:

```bash
strace ping -c 1 8.8.8.8
```

You may encounter calls such as:

```text
socket()
sendto()
recvmsg()
close()
```

Now connect it with networking:

```text
ping
 │
 ▼
socket()
 │
 ▼
Kernel networking
 │
 ▼
Network interface
 │
 ▼
Network
```

`strace` lets you observe the **application ↔ kernel** part.

---

# 10. Debugging With `strace` ⭐

This is where `strace` becomes genuinely useful.

Imagine:

```bash
./myapp
```

prints:

```text
Failed to start
```

Useless error message.

Run:

```bash
strace ./myapp
```

You might discover:

```text
openat(..., "/etc/myapp/config", ...) = -1 ENOENT
```

Important parts:

```text
-1      → operation failed

ENOENT  → No such file or directory
```

Now you know:

```text
Application
     ↓
Tried opening config
     ↓
Kernel says file doesn't exist
     ↓
Application fails
```

That's why `strace` is such a powerful debugging tool.

---

# 11. Another Example — Permission Problem

Suppose you see:

```text
openat(..., "/secret/data", ...) = -1 EACCES
```

`EACCES` means permission denied.

So:

```text
Program says:
"Something failed"

strace says:
"I tried opening /secret/data
and Linux returned EACCES."
```

Now you know where to investigate.

---

# 12. Filtering `strace`

Real traces can contain thousands of lines.

Instead of:

```bash
strace cat hello.txt
```

you can filter specific calls:

```bash
strace -e trace=openat,read,write,close cat hello.txt
```

Now you're basically saying:

> Show me only these system calls.

This makes traces much easier to understand.

---

# 13. Save Output

```bash
strace -o trace.txt ls
```

`-o` → **output file**

Then:

```bash
less trace.txt
```

Useful when the trace is huge and you want to inspect/search it later.

---

# 14. Count System Calls

Instead of printing everything:

```bash
strace -c ls
```

`-c` → summary/count statistics.

You'll get something like:

```text
% time   calls   errors   syscall

30%       20       0      mmap
20%       15       0      openat
10%        5       0      read
...
```

Useful for quickly understanding:

> **What kinds of kernel operations is this program performing?**

---

# 15. Trace an Existing Process

You don't always need to start the program with `strace`.

If something is already running:

```bash
strace -p PID
```

Example:

```bash
sudo strace -p 1234
```

`-p` → **process ID**

Conceptually:

```text
Existing Server
      │
      │ syscalls
      ▼
    Kernel
      ▲
      │
   👀 strace
```

This can be extremely useful when investigating a process that appears stuck.

---

# 16. Follow Child Processes

Programs sometimes create other processes.

Use:

```bash
strace -f command
```

`-f` → **follow children**

Conceptually:

```text
Main Process
     │
     ├── Child A
     │
     └── Child B

strace -f
   👀 watches them too
```

---

# 17. `strace` vs eBPF

You learned eBPF on Day 26.

Now the difference should be clearer.

### `strace`

Think:

> **"What system calls is this process making?"**

```text
Application
     │
     │ syscalls
     │ ← 👀 strace
     ▼
Kernel
```

### eBPF

Think:

> **"I want programmable observation/action around particular system/kernel events."**

```text
          Kernel
             │
    ┌────────┼────────┐
    ▼        ▼        ▼
 syscall   network  scheduler
    │
   eBPF
```

So:

```text
strace → process syscall debugging

eBPF → broader programmable tracing/
       networking/security/observability
```

---

# 🔗 Connect Everything

This is a fantastic revision topic because `strace` exposes many things you've already studied:

```text
                    strace
                       │
        ┌──────────────┼──────────────┐
        ▼              ▼              ▼
     Filesystem      Memory        Network
        │              │              │
     openat()         mmap()         socket()
     read()           brk()          connect()
     write()                         sendto()
        │              │              │
        └──────────────┼──────────────┘
                       ▼
                 Linux Kernel
```

You're no longer just learning:

> "`openat()` opens files."

You can literally run:

```bash
strace cat file.txt
```

and **watch the application ask Linux to do it.**

---

# ⚡ 2-Minute Revision

**`strace`** → traces system calls and signals associated with a process.

**System call** → user-space application's interface for requesting kernel services.

```text
openat()     → open
read()       → read
write()      → write
close()      → close FD

execve()     → execute program

mmap()       → map memory
brk()        → adjust heap

socket()     → create socket
connect()    → connect socket

clone()      → create task
exit_group() → terminate
```

Important options:

```bash
strace command
```

Trace program.

```bash
strace -e trace=openat,read,write command
```

Filter calls.

```bash
strace -o trace.txt command
```

Save output.

```bash
strace -c command
```

Show summary.

```bash
strace -p PID
```

Attach to running process.

```bash
strace -f command
```

Follow child processes.

## ⭐ One Mental Model

```text
              USER SPACE

             Application
                  │
                  │
            System Calls
                  │
            👀 strace sees
                  │
══════════════════▼════════════════
              KERNEL SPACE

             Linux Kernel
                  │
        ┌─────────┼─────────┐
        ▼         ▼         ▼
      Files     Memory    Network
```

> **`strace` = "Show me what this program is asking the Linux kernel to do."**

That's really the entire Day 29.