# 🐧 Linux Internals — Day 31: `lsof`

> 🎯 **Core idea:** `lsof` answers: **“What resources does this process currently have open?”**

---

## 1. First — What Does “Everything is a File” Mean?

You've probably heard:

> **In Linux, everything is a file.**

It's a useful simplification, but more precisely Linux gives many different I/O resources a **file-descriptor-based interface**.

A process can interact with:

```text
Regular file
Directory
Terminal
Network socket
Pipe
Device
```

through file descriptors.

This gives programs a common model:

```text
open/get resource
      ↓
File Descriptor
      ↓
read() / write() / other operations
      ↓
close()
```

---

# 2. What is a File Descriptor?

A **File Descriptor (FD)** is a small integer used by a process to refer to an open resource.

Every process has its own **file descriptor table**.

```text
Process
  │
  ▼
FD Table

0 ───→ stdin
1 ───→ stdout
2 ───→ stderr
3 ───→ file.txt
4 ───→ TCP Socket
5 ───→ Pipe
6 ───→ Device
```

So when code does:

```c
read(3, buffer, 100);
```

it's essentially saying:

> Kernel, read from whatever **FD 3** refers to in this process.

---

# 3. The First Three FDs

Processes conventionally start with:

```text
FD 0 → stdin
FD 1 → stdout
FD 2 → stderr
```

For a shell running in a terminal:

```text
            Shell

0 ─────→ Terminal input
1 ─────→ Terminal output
2 ─────→ Terminal errors
```

That's why:

```bash
echo "hello"
```

eventually writes to:

```text
FD 1 → stdout → terminal
```

You actually saw this yesterday with `strace`:

```text
write(1, "hello\n", ...)
      ↑
    FD 1
```

---

# 4. Opening a File

Suppose a program opens:

```text
notes.txt
```

Internally:

```text
Program
   │
   │ openat("notes.txt")
   ▼
Kernel
   │
   ▼
Returns FD 3
```

Now:

```text
Process FD Table

0 → stdin
1 → stdout
2 → stderr
3 → notes.txt
```

The program can:

```text
read(3)
write(3)
close(3)
```

Once:

```text
close(3)
```

happens, that FD is free to be reused later.

---

# 5. What is `lsof`?

**`lsof` = List Open Files**

It asks Linux for information about resources processes currently have open and presents it in a useful format.

Mental model:

```text
             Process
                │
         File Descriptor Table
                │
     ┌──────────┼──────────┐
     ▼          ▼          ▼
   Files      Sockets     Pipes
     │          │          │
     └──────────┼──────────┘
                │
             👀 lsof
```

Think:

> **`lsof` = “Show me what this process currently has open.”**

---

# 6. Why Are Network Sockets in `lsof`?

Suppose you create a network socket:

```text
Application
     │
     │ socket()
     ▼
Kernel
     │
     ▼
Returns FD 4
```

Now:

```text
FD 4 → TCP socket
```

The application can use that descriptor for network I/O.

Conceptually:

```text
Process
  │
FD 4
  │
Socket
  │
TCP/IP
  │
Network
```

That's why `lsof` can show network connections too.

---

# 7. Why Are Pipes FDs?

Remember IPC:

```bash
ls | grep txt
```

The shell creates a pipe.

Conceptually:

```text
ls
 │
FD → Pipe → FD
             │
            grep
```

Again, processes interact with the pipe through **file descriptors**.

So `lsof` can expose pipe-related resources too.

---

# 8. Basic `lsof`

Run:

```bash
lsof
```

This can produce a **huge** amount of output because you're asking:

> Show open files/resources across processes I can inspect.

You'll see columns like:

```text
COMMAND   PID   USER   FD   TYPE   DEVICE   SIZE/OFF   NODE   NAME
```

The most useful ones initially:

|Column|Meaning|
|---|---|
|`COMMAND`|Process/program|
|`PID`|Process ID|
|`USER`|Owner|
|`FD`|File descriptor/use|
|`TYPE`|Resource type|
|`NAME`|Resource/path/socket|

---

# 9. Inspect One Process ⭐

Much more useful:

```bash
lsof -p PID
```

`-p` → **process**

For your current shell:

```bash
lsof -p $$
```

Remember:

```text
$$
```

means the **PID of the current shell** in common shells such as Bash.

Conceptually:

```text
Current Shell
     │
     ▼
FD Table / open resources
     │
     ▼
   lsof
```

You may see its terminal, working directory, libraries, executables, and open descriptors.

---

# 10. Some `FD` Values Aren't Numbers

You might expect only:

```text
0
1
2
3
4
```

But `lsof` may show values like:

```text
cwd
rtd
txt
mem
```

These aren't ordinary numbered FDs.

Useful meanings:

```text
cwd → Current Working Directory

rtd → Root Directory

txt → Program executable/text

mem → Memory-mapped file
```

So:

```bash
lsof -p $$
```

gives a broader view than simply listing `/proc/<PID>/fd`.

---

# 11. Find Who Is Using a Port ⭐

This is one of the most useful `lsof` commands.

Imagine you start your backend:

```text
Error:

Port 8000 already in use
```

Instead of randomly killing processes:

```bash
lsof -i :8000
```

`-i` → network-related files/sockets.

You might see:

```text
COMMAND   PID    USER   FD   TYPE   NAME

python3   8421   user   3u   IPv4   *:8000
```

Now you know:

```text
Port 8000
    ↓
Socket
    ↓
FD 3
    ↓
Python
    ↓
PID 8421
```

🔥 This is an extremely practical debugging pattern.

---

# 12. Real Example — Web Server

Start:

```bash
python3 -m http.server 8000
```

Internally, roughly:

```text
Python
  │
  ▼
socket()
  │
  ▼
FD
  │
  ▼
bind(:8000)
  │
  ▼
listen()
```

Another terminal:

```bash
lsof -i :8000
```

Now `lsof` discovers:

```text
Python Process
      │
      ▼
Socket FD
      │
      ▼
Port 8000
```

This connects directly to your networking lessons.

---

# 13. Show Network Resources

Run:

```bash
lsof -i
```

Think:

> **Show network-related open files/sockets.**

You might see:

```text
Browser
SSH
Web server
DNS-related processes
other network programs
```

Depending on your permissions, you may not see everything.

---

# 14. Find Who Is Using a File

Suppose you have:

```text
database.db
```

and want to know:

> Which process has this open?

Use:

```bash
lsof database.db
```

Conceptually:

```text
database.db
     ▲
     │ FD 7
     │
Process 4200
```

`lsof` gives you the connection between:

```text
Resource ↔ Process
```

---

# 15. “Device or Resource Busy”

Suppose:

```bash
umount /mnt/usb
```

returns:

```text
target is busy
```

Why?

Some process may still be using something inside the mounted filesystem.

Conceptually:

```text
USB filesystem
      ▲
      │
  file open
      │
    Process
```

Tools such as `lsof` can help identify what's holding resources open.

For example:

```bash
lsof +D /mnt/usb
```

can recursively inspect open files under a directory, though it can be slow on large directory trees.

Then you can determine what process needs to be closed/stopped before unmounting.

---

# 16. Deleted File But Disk Space Isn't Freed 😵

This is a classic Linux issue.

Imagine a server writes:

```text
huge.log → 10 GB
```

A process has it open:

```text
Server
  │
FD 5
  │
huge.log
```

You delete:

```bash
rm huge.log
```

The filename disappears.

But the process may still hold an open reference:

```text
Server
  │
FD 5
  │
deleted file data still referenced
```

Linux can keep the underlying file data around until the last open reference is closed.

`lsof` can reveal entries marked:

```text
(deleted)
```

A common investigation:

```bash
sudo lsof +L1
```

This is extremely useful when:

```text
df says disk is full

but

du doesn't explain all the usage
```

One possible cause is **deleted-but-still-open files**.

---

# 17. `lsof` vs `/proc/<PID>/fd`

Linux already exposes a process's numbered FDs through:

```bash
ls -l /proc/<PID>/fd
```

Example:

```text
0 → /dev/pts/0
1 → /dev/pts/0
2 → /dev/pts/0
3 → /home/user/file.txt
4 → socket:[12345]
```

So conceptually:

```text
/proc/PID/fd
      │
      ▼
Raw kernel-exposed FD view

lsof
      │
      ▼
More human-friendly correlated information
```

This is another reason `/proc` is so useful for Linux internals.

---

# 18. Connect `lsof` + `strace`

This is the connection I really want you to understand.

Yesterday:

```bash
strace cat test.txt
```

You saw:

```text
openat("test.txt") = 3
      ↓
read(3)
      ↓
write(1)
      ↓
close(3)
```

That's showing the **actions happening over time**.

Today `lsof` shows more of the **current state**:

```text
Process
   │
FD 3
   │
test.txt
```

So:

```text
strace
  ↓
"What syscalls is the process MAKING?"


lsof
  ↓
"What resources does the process HAVE OPEN?"
```

That's a very useful distinction.

---

# 19. Connect With `epoll`

Day 27:

```text
10,000 clients
      ↓
10,000 sockets
      ↓
10,000 FDs
      ↓
epoll watches them
```

Today:

```text
Process
   │
   ├── FD 4 → Socket
   ├── FD 5 → Socket
   ├── FD 6 → Socket
   ├── FD 7 → Socket
   └── ...
```

`lsof` can help you inspect those open sockets/resources.

So the topics connect:

```text
Socket
   ↓
File Descriptor
   ↓
epoll → watches readiness

lsof → inspects open resources
```

---

# 🧠 Big Picture

This is the diagram worth remembering:

```text
                  PROCESS

                     │
                     ▼
           File Descriptor Table
                     │
       ┌─────────────┼──────────────┐
       ▼             ▼              ▼
     FD 3          FD 4           FD 5
       │             │              │
       ▼             ▼              ▼
     File          Socket          Pipe
                     │
                     ▼
                   Network


                  👀 lsof
                     │
                     ▼

        "What does this process
             currently use?"
```

---

# ⚡ 2-Minute Revision

**File Descriptor (FD)**

> Small integer used by a process to refer to an open resource.

```text
0 → stdin
1 → stdout
2 → stderr
3+ → files/sockets/pipes/etc.
```

**`lsof`**

> **List Open Files** — inspect open resources associated with processes.

Important commands:

```bash
lsof -p PID
```

→ resources opened/used by a process.

```bash
lsof -p $$
```

→ inspect current shell.

```bash
lsof -i
```

→ network-related open files/sockets.

```bash
lsof -i :8000
```

→ find process using port 8000.

```bash
lsof /path/to/file
```

→ find processes using a particular file.

```bash
sudo lsof +L1
```

→ find open files whose link count is below 1, commonly useful for deleted-but-still-open files.

### ⭐ Remember

```text
strace → What is the process DOING?

lsof   → What does the process HAVE OPEN?

perf   → Where is the program spending resources/time?
```

Those three together give you a pretty solid little Linux debugging toolkit.