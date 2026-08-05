Yep. Do this as a **hands-on Day 22 lab**. Don’t just copy commands—after each one, predict what you think Linux will show.

## 🧪 Lab 1 — Find your shell

Run:

```bash
echo $$
```

`$$` = PID of your current shell.

Then:

```bash
ps -p $$
```

You should see something like:

```text
PID    TTY       TIME     CMD
4821   pts/0     00:00    bash
```

Mental model:

```text
bash process
     │
     ├── PID = 4821
     │
     ▼
/proc/4821/
```

Now verify:

```bash
ls /proc/$$
```

Look specifically for:

```text
status
cmdline
maps
fd
environ
limits
```

---

## 🧪 Lab 2 — Inspect process state

Run:

```bash
cat /proc/$$/status
```

Don't worry about everything. Find these:

```bash
grep -E 'Name|State|Pid|PPid|Threads|VmSize|VmRSS' /proc/$$/status
```

Example:

```text
Name:       bash
State:      S (sleeping)
Pid:        4821
PPid:       4700
VmSize:     10000 kB
VmRSS:       5000 kB
Threads:    1
```

Think about:

```text
Pid     → Who am I?
PPid    → Who created me?
State   → What am I currently doing?
Threads → How many execution threads?
VmSize  → How much virtual memory?
VmRSS   → How much is currently resident in RAM?
```

---

## 🧪 Lab 3 — Follow the parent

Get the parent:

```bash
grep PPid /proc/$$/status
```

Suppose:

```text
PPid: 3120
```

Inspect it:

```bash
cat /proc/3120/status | head
```

You're now literally following:

```text
Parent Process
      │
      │ fork/exec etc.
      ▼
    Shell
```

---

## 🧪 Lab 4 — File descriptors

Run:

```bash
ls -l /proc/$$/fd
```

Look for:

```text
0 -> /dev/pts/0
1 -> /dev/pts/0
2 -> /dev/pts/0
```

Remember:

```text
0 → stdin
1 → stdout
2 → stderr
```

Now create another FD indirectly:

```bash
exec 3>test.txt
```

Check again:

```bash
ls -l /proc/$$/fd
```

You should now find something like:

```text
3 -> /path/to/test.txt
```

🔥 This is important.

You just watched the kernel's file-descriptor state change through `/proc`.

Write through FD 3:

```bash
echo "hello from fd 3" >&3
```

Check:

```bash
cat test.txt
```

Close it:

```bash
exec 3>&-
```

Check again:

```bash
ls -l /proc/$$/fd
```

FD `3` should disappear.

Mental model:

```text
bash
 │
 ├── FD 0 ──► terminal
 ├── FD 1 ──► terminal
 ├── FD 2 ──► terminal
 └── FD 3 ──► test.txt
```

This experiment is worth remembering.

---

## 🧪 Lab 5 — Inspect virtual memory

Run:

```bash
head /proc/$$/maps
```

Then:

```bash
grep '\[stack\]' /proc/$$/maps
```

and:

```bash
grep '\[heap\]' /proc/$$/maps
```

You might see:

```text
55ab...-55cd... rw-p ... [heap]
7ffd...-7fff... rw-p ... [stack]
```

The addresses are **virtual addresses**.

Conceptually:

```text
Process Virtual Address Space

High addresses
┌─────────────┐
│    Stack    │ ← [stack]
├─────────────┤
│ Libraries   │
├─────────────┤
│    Heap     │ ← [heap]
├─────────────┤
│ Data / BSS  │
├─────────────┤
│    Code     │
└─────────────┘
Low addresses
```

Also try:

```bash
grep '\.so' /proc/$$/maps | head
```

Those are shared libraries mapped into the process.

---

## 🧪 Lab 6 — Environment variables

First:

```bash
cat /proc/$$/environ
```

It'll probably look ugly because entries are separated by **null bytes**, not newlines.

Make it readable:

```bash
tr '\0' '\n' < /proc/$$/environ | head
```

Look for things such as:

```text
HOME=
PATH=
USER=
SHELL=
```

---

## 🧪 Lab 7 — Process limits

Run:

```bash
cat /proc/$$/limits
```

Find:

```text
Max open files
Max processes
Max stack size
```

Compare open-file limits with:

```bash
ulimit -n
```

The numbers should connect.

---

## 🧪 Lab 8 — Compare two processes

Start:

```bash
sleep 120 &
```

You'll get something like:

```text
[1] 8392
```

Store its PID:

```bash
PID=$!
```

`$!` means the PID of the most recently started background process.

Verify:

```bash
echo $PID
```

Now compare.

Shell:

```bash
grep -E 'Name|State|Pid|PPid|VmSize|VmRSS|Threads' /proc/$$/status
```

Sleep:

```bash
grep -E 'Name|State|Pid|PPid|VmSize|VmRSS|Threads' /proc/$PID/status
```

Compare FDs:

```bash
ls -l /proc/$$/fd
```

```bash
ls -l /proc/$PID/fd
```

Compare memory mappings:

```bash
wc -l /proc/$$/maps
wc -l /proc/$PID/maps
```

Now you're asking Linux:

> How are these two processes internally different?

---

## 🧪 Final Experiment — Watch `/proc` disappear

Check:

```bash
ls /proc/$PID
```

Now kill `sleep`:

```bash
kill $PID
```

Try again:

```bash
ls /proc/$PID
```

You should get:

```text
No such file or directory
```

💡 **This proves the core idea of today's lesson.**

```text
Process created
     ↓
PID assigned
     ↓
/proc/<PID> exists
     ↓
Process terminates
     ↓
/proc/<PID> disappears
```

`/proc/<PID>` isn't a permanent folder containing saved process information. It's a **live kernel representation of a currently existing process**.

### Your task

Do **Labs 1–4 first** and send me the output of:

```bash
echo $$
grep -E 'Name|State|Pid|PPid|Threads|VmSize|VmRSS' /proc/$$/status
ls -l /proc/$$/fd
```

We'll read your **actual process internals** together before moving into the memory map.