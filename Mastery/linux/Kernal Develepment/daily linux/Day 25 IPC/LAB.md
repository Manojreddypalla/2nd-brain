# 🧪 Linux Internals — Day 25 IPC Lab

Do these in order. The goal is to **see processes communicating**, not just run commands.

## Lab 1 — Anonymous Pipe

Run:

```bash
echo "Hello Linux IPC" | cat
```

Expected:

```text
Hello Linux IPC
```

### What happened internally?

```text
echo
 │ stdout
 ▼
[ Kernel Pipe Buffer ]
 │ stdin
 ▼
cat
```

The shell creates the pipe and connects the file descriptors roughly like:

```text
echo stdout (fd 1) → pipe → cat stdin (fd 0)
```

Try:

```bash
ls | wc -l
```

Here `ls` produces bytes → pipe carries them → `wc -l` consumes them.

---

## Lab 2 — Create a FIFO

First create it:

```bash
mkfifo mypipe
```

### What is `mkfifo`?

- `mk` → make
    
- `fifo` → First In, First Out
    

Check it:

```bash
ls -l mypipe
```

You'll see something like:

```text
prw-r--r-- 1 user user ... mypipe
^
```

The `p` means **named pipe**.

### Open two terminals

**Terminal 1:**

```bash
cat mypipe
```

It appears to freeze.

That's intentional: `cat` is **blocked waiting for data**.

**Terminal 2:**

```bash
echo "Hello from Process B" > mypipe
```

Terminal 1 receives:

```text
Hello from Process B
```

Mental model:

```text
Terminal 2
 echo
   │
   ▼
[mypipe FIFO]
   │
   ▼
  cat
Terminal 1
```

🔥 Important observation: the two commands are unrelated processes, but the **named FIFO gives them a common communication point**.

Clean up:

```bash
rm mypipe
```

---

## Lab 3 — Observe System V IPC

Run:

```bash
ipcs
```

You'll see sections such as:

```text
Message Queues

Shared Memory Segments

Semaphore Arrays
```

`ipcs` = **IPC status**.

For more focused views:

```bash
ipcs -m
```

`-m` → shared **memory**

```bash
ipcs -q
```

`-q` → message **queues**

```bash
ipcs -s
```

`-s` → **semaphores**

So remember:

```text
ipcs
 ├── -m → memory
 ├── -q → queues
 └── -s → semaphores
```

Don't remove anything yet.

---

## Lab 4 — Inspect IPC Cleanup

Run:

```bash
ipcrm --help
```

`ipcrm` means roughly:

```text
IPC + remove
```

It's used to remove IPC objects such as:

```text
Shared Memory
Message Queue
Semaphore
```

For today, just inspect the help rather than deleting existing objects—you may be looking at IPC resources created by other programs.

---

## Lab 5 — See the Processes Behind a Pipe

Run:

```bash
sleep 100 | cat
```

Keep that terminal open.

Open another terminal:

```bash
ps
```

or:

```bash
ps aux | grep sleep
```

You should see `sleep` running.

The important idea is that:

```text
sleep process
     │
     │ pipe
     ▼
cat process
```

The pipe isn't a process. It's a **kernel object connecting file descriptors belonging to the processes**.

Stop it with:

```text
Ctrl + C
```

---

# 🧠 Lab Check

After doing these experiments, make sure you can explain this without notes:

```text
echo "hello" | cat

        Shell
          │
       creates
          ▼
        Pipe
       /    \
      /      \
   echo      cat
 stdout     stdin
      \      /
       ▼    ▼
   Kernel pipe
      buffer
```

The most important thing to notice today is:

> **A pipe doesn't magically connect programs. The shell creates a kernel pipe and connects the programs' file descriptors to its read/write ends.**

That's the Linux-internals idea you want to carry forward.