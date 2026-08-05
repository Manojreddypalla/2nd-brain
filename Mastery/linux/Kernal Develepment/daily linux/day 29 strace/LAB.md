# 🧪 Linux Internals — Day 29: `strace` Lab

> 🎯 **Goal:** Don't just run `strace`. Learn to **read the story** hidden inside the system calls.

## Lab 1 — Your First Trace

Run:

```bash
strace ls
```

You'll get a wall of output. That's normal. 😭

Near the beginning, find:

```text
execve(...)
```

This means:

```text
Shell
  ↓
execve()
  ↓
Kernel starts `ls`
```

Near the end you'll see:

```text
write(...)
exit_group(...)
```

Think:

```text
Start program → do work → print output → exit
```

---

## Lab 2 — Trace a File ⭐

This one is the most important experiment.

First:

```bash
echo "Hello Linux" > test.txt
```

Now trace `cat`, but filter out the noise:

```bash
strace -e trace=openat,read,write,close cat test.txt
```

Look for something resembling:

```text
openat(..., "test.txt", ...) = 3

read(3, "Hello Linux\n", ...) = 12

write(1, "Hello Linux\n", 12) = 12

close(3) = 0
```

Now **read it like a story**.

### Step 1

```text
openat(... "test.txt" ...) = 3
```

Linux opens:

```text
test.txt
```

and returns:

```text
FD 3
```

So the process now has roughly:

```text
FD 0 → keyboard/stdin
FD 1 → terminal/stdout
FD 2 → terminal/stderr
FD 3 → test.txt
```

### Step 2

```text
read(3, ...)
```

Means:

> Read from **test.txt**, because test.txt is FD 3.

### Step 3

```text
write(1, ...)
```

Means:

> Write the data to **stdout**.

And stdout is your terminal.

### Step 4

```text
close(3)
```

Done with the file.

So you've literally observed:

```text
cat test.txt

     ↓

openat()
     ↓
FD 3
     ↓
read(3)
     ↓
"Hello Linux"
     ↓
write(1)
     ↓
Terminal
     ↓
close(3)
```

🔥 This single experiment connects **system calls + files + file descriptors + processes**.

---

# Lab 3 — Watch a Failure

Now intentionally request a file that doesn't exist:

```bash
strace -e trace=openat cat doesnotexist.txt
```

Look for:

```text
openat(..., "doesnotexist.txt", ...) = -1 ENOENT
```

Break it down:

```text
openat()
   ↓
doesnotexist.txt
   ↓
Kernel looks for it
   ↓
Not found
   ↓
-1 ENOENT
```

`ENOENT` = **No such file or directory**

This demonstrates why `strace` is useful for debugging.

Your application might only say:

```text
Failed to open file
```

But `strace` can reveal the actual kernel error.

---

# Lab 4 — Trace Only File Operations

Instead of manually listing syscalls, try:

```bash
strace -e trace=%file cat test.txt
```

This filters for file-related system calls.

You'll see things involving files such as:

```text
openat()
newfstatat()
access()
...
```

This is useful when debugging:

> **"Which files is this application trying to access?"**

---

# Lab 5 — Save a Trace

Run:

```bash
strace -o trace.txt ls
```

`-o` = **output**

Now:

```bash
less trace.txt
```

Inside `less`, search for something:

```text
/openat
```

Press:

```text
n
```

to jump to the next match.

Exit with:

```text
q
```

This is much easier than trying to read hundreds of lines directly in the terminal.

---

# Lab 6 — System Call Summary

Run:

```bash
strace -c ls
```

Instead of every syscall, you'll get a summary like:

```text
% time   calls   errors   syscall
------   -----   ------   -------
 ...      ...      ...    openat
 ...      ...      ...    mmap
 ...      ...      ...    read
 ...      ...      ...    close
```

Focus on:

```text
calls  → how many times

errors → failed calls

syscall → which kernel operation
```

This gives you a quick profile of how the program interacts with Linux.

---

# Lab 7 — Connect Day 28: Virtual Memory

Remember yesterday:

```text
Virtual Memory
mmap()
Pages
Demand Paging
```

Let's actually see part of that.

Run:

```bash
strace -e trace=mmap,munmap,brk ls
```

You'll see calls such as:

```text
brk(...)
mmap(...)
mmap(...)
mmap(...)
munmap(...)
```

Now the connection becomes real:

```text
ls starts
   ↓
needs memory mappings
   ↓
mmap()
   ↓
Kernel creates mappings
   ↓
Process virtual address space
```

Yesterday:

> "Programs use `mmap()`."

Today:

> **You're watching them call it.**

---

# Lab 8 — Network System Calls 🌐

Try:

```bash
strace -e trace=network ping -c 1 8.8.8.8
```

Depending on your system/`ping` implementation, you may see calls such as:

```text
socket(...)
sendto(...)
recvmsg(...)
```

Think:

```text
ping
 │
 ├── socket()
 │       ↓
 │   create socket
 │
 ├── sendto()
 │       ↓
 │   send packet
 │
 ├── recvmsg()
 │       ↓
 │   receive response
 │
 └── close()
```

This connects your **Computer Networks knowledge directly to Linux internals**.

---

# 🔥 Lab 9 — The Real Debugging Experiment

Let's create an actual failure.

Run:

```bash
mkdir testdir
```

Create a file:

```bash
echo "secret" > testdir/data.txt
```

Then inspect normally:

```bash
cat testdir/data.txt
```

Now trace it:

```bash
strace -e trace=%file cat testdir/data.txt
```

Look for:

```text
openat(... "testdir/data.txt" ...)
```

The key debugging mindset is:

```text
Program doesn't work
        ↓
Don't guess immediately
        ↓
strace program
        ↓
Find failed syscall
        ↓
Look at errno
        ↓
Understand what kernel rejected
```

For example:

```text
ENOENT → file/path doesn't exist

EACCES → permission denied

ECONNREFUSED → network connection refused
```

That's how `strace` becomes a **debugging tool**, rather than just a cool command that vomits 400 lines onto your terminal.

---

# 🧠 Final Experiment — Read the Story Yourself

Run:

```bash
strace -e trace=openat,read,write,close cat /etc/hostname
```

Don't just look at the output.

Try to identify:

```text
1. Which syscall opened /etc/hostname?

2. What FD did Linux return?

3. Which syscall read from that FD?

4. Which FD did write() use?

5. Why was FD 1 used?

6. Which syscall closed the file?
```

If you can answer those six from the trace, **Day 29 is done.** ✅

Your mental picture should now be:

```text
            cat /etc/hostname

                   │
                   ▼
              openat()
                   │
              returns FD
                   │
                   ▼
                read()
                   │
                   ▼
                data
                   │
                   ▼
              write(1)
                   │
                   ▼
               Terminal
                   │
                   ▼
               close()
```

> **The big skill today isn't memorizing `strace` commands. It's learning to look at a trace and reconstruct what the program asked Linux to do.**