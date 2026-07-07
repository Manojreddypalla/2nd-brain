# Linux Processes — Day 2 (10–15 min)

This is one of the most important concepts in Linux because **everything you run becomes a process**. Once you understand processes, topics like scheduling, memory, signals, threads, Docker, Kubernetes, and even malware analysis become much easier.

---

# 1. What is a Program?

A **program** is just a file stored on disk.

Think of it as a recipe.

Examples:

- `/bin/ls`
    
- `/usr/bin/python3`
    
- `firefox`
    
- `code`
    

A program **does nothing by itself**.

It is simply instructions waiting to be executed.

```
Disk

python
firefox
vim
gcc
```

No CPU.  
No RAM.

Just files.

---

# 2. What is a Process?

A **process** is a program that is currently executing.

When Linux starts a program, it creates a process.

The OS allocates:

- Memory
    
- CPU time
    
- Stack
    
- Heap
    
- Registers
    
- File descriptors
    
- Process ID (PID)
    

Now the program becomes **alive**. ([Wikipedia](https://en.wikipedia.org/wiki/Process_management_%28computing%29?utm_source=chatgpt.com "Process management (computing)"))

```
Disk
------
python

↓

Execution

CPU
RAM
Stack
Heap
Registers

↓

Process
```

### Mental Model

Imagine a game.

The `.exe` file is the game installed on disk.

When you double-click it,

Windows/Linux loads it into RAM.

Now it has

- current level
    
- player position
    
- score
    
- inventory
    

That running game is the **process**.

---

# Program vs Process

|Program|Process|
|---|---|
|Passive|Active|
|Stored on disk|Running in memory|
|No CPU usage|Uses CPU|
|No memory|Has allocated memory|
|Static|Dynamic|

Example:

```
/usr/bin/python
```

is just a program.

Running

```
python app.py
```

creates

```
PID 3521
```

which is a process.

---

# 3. Process ID (PID)

Every process gets a unique number called a **Process ID (PID)**.

Example

```
PID

1
245
801
1234
5421
```

Linux uses this number to identify and manage processes. ([Wikipedia](https://en.wikipedia.org/wiki/Process_identifier?utm_source=chatgpt.com "Process identifier"))

Commands:

```
echo $$
```

Current shell PID

```
ps
```

Show running processes

```
ps -ef
```

Detailed list

```
top
```

Live processes

Example

```
PID TTY TIME CMD

1254 bash
1320 vim
1411 python
```

---

# 4. Parent and Child Processes

Linux creates processes in a **family tree**.

One process starts another.

```
Parent

↓

Child

↓

Grandchild
```

Example

You type

```
python app.py
```

What actually happens?

```
Terminal

↓

bash

↓

python

↓

app.py
```

The shell (`bash`) creates the Python process.

Therefore

```
bash
```

is the **parent**

```
python
```

is the **child**.

On Unix-like systems, a parent commonly creates a child using the `fork()` system call. After `fork()`, the child receives a new PID, while the parent receives the child's PID as the return value. ([GeeksforGeeks](https://www.geeksforgeeks.org/dsa/difference-between-process-parent-process-and-child-process/?utm_source=chatgpt.com "Difference Between Process, Parent Process, and Child ..."))

---

### Visual

```
PID 100

bash
   │
   ├── python
   │      │
   │      └── gcc
   │
   └── vim
```

Everything begins from one ancestor process started during boot.

---

# Parent Process

A parent process

- starts another process
    
- may wait for it
    
- may terminate it
    
- may create many children ([GeeksforGeeks](https://www.geeksforgeeks.org/dsa/difference-between-process-parent-process-and-child-process/?utm_source=chatgpt.com "Difference Between Process, Parent Process, and Child ..."))
    

Example

```
bash

↓

python

↓

node

↓

chrome
```

---

# Child Process

A child process

- is created by another process
    
- has its own PID
    
- inherits some resources (such as environment variables and open file descriptors) from its parent, but runs independently in its own address space. ([natalieagus.github.io](https://natalieagus.github.io/50005/os/processes?utm_source=chatgpt.com "Processes | 50.005 CSE"))
    

---

# 5. Process Lifecycle

A process changes states during its lifetime.

```
Created

↓

Ready

↓

Running

↓

Sleeping

↓

Running

↓

Stopped

↓

Running

↓

Zombie

↓

Removed
```

Let's understand each one.

---

## Created (New)

Linux creates the process.

Memory is allocated.

PID assigned.

Process Control Block (PCB) is created.

Not executing yet.

---

## Running

CPU is executing instructions.

Example

```
python app.py
```

CPU is currently interpreting Python code.

---

## Sleeping (Waiting / Blocked)

The process is waiting for something.

Examples

Waiting for

- keyboard input
    
- network
    
- disk
    
- timer
    

The CPU is **not** executing it during this time.

Example

```
sleep 10
```

The process exists but simply waits.

---

## Stopped

Execution is paused.

Example

Press

```
Ctrl + Z
```

The process stops.

It can later continue.

---

## Zombie

One of the most misunderstood Linux concepts.

A zombie is a process that has **already finished execution**, but its parent has not yet collected (reaped) its exit status using `wait()`. It no longer runs or uses CPU, but a small entry remains in the process table until the parent acknowledges it. ([natalieagus.github.io](https://natalieagus.github.io/50005/os/processes?utm_source=chatgpt.com "Processes | 50.005 CSE"))

Think of it like:

```
Student finished exam.

Still waiting for teacher
to collect answer sheet.
```

Zombie

```
Dead

Not running

Still listed
```

---

# Complete Lifecycle

```
Program

↓

Process Created

↓

Ready

↓

Running

↓

Waiting

↓

Running

↓

Stopped

↓

Running

↓

Exit

↓

Zombie

↓

Removed
```

---

# Quick Revision

**Program**

- Stored on disk
    
- Passive
    

**Process**

- Running program
    
- Uses RAM and CPU
    

**PID**

- Unique identifier for every process
    

**Parent**

- Creates child processes
    

**Child**

- Created by parent
    
- Gets its own PID
    

**Running**

- Executing on CPU
    

**Sleeping**

- Waiting for an event (I/O, timer, input)
    

**Stopped**

- Paused
    

**Zombie**

- Finished execution, waiting for the parent to collect its exit status
    

---

## 5-minute practical

Run these commands in your terminal:

```bash
echo $$          # Show your shell's PID
ps               # List processes for this terminal
ps -ef | head    # View the first few system processes
pstree           # Display the process tree (install if needed)
sleep 30 &       # Start a background process
jobs             # Show background jobs
```

By the end of this session, you should be able to answer these questions without looking up the notes:

1. Why is a program different from a process?
    
2. Why does every process need a PID?
    
3. How are parent and child processes related?
    
4. Why does a sleeping process use almost no CPU?
    
5. Why can a zombie process still appear in `ps` even though it has already finished?