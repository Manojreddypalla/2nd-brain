# Linux Internals — Day 8 Notes

# PID Namespace (Master Notes)

---

# Intuition First — Why does this even exist?

Imagine you're running **100 different applications** on one Linux machine.

- Gmail backend
    
- Netflix backend
    
- Redis
    
- PostgreSQL
    
- Your own web app
    
- Thousands of other services
    

Every program creates processes.

Without isolation, every application can see **every other application's processes**.

```
Host

PID 1      systemd
PID 230    sshd
PID 480    nginx
PID 900    postgres
PID 1300   redis
PID 2100   python
PID 3500   node
PID 6000   chrome
...
```

Suppose you run your application inside Docker.

If your application could see all host processes,

```
ps
```

would show

```
systemd
NetworkManager
sshd
docker
postgres
chrome
...
```

That completely breaks isolation.

Containers should feel like **their own tiny Linux machine.**

So Linux says:

> "Instead of showing every process, I'll create a new process universe."

That universe is called a **PID Namespace**.

---

# One-Line Definition

> **A PID Namespace isolates the process IDs (process tree) visible to a process.**

or

> Every PID namespace has its own independent process hierarchy.

---

# What is a PID?

Every running process gets a unique integer.

Example

```
PID 1
PID 2
PID 10
PID 457
PID 1932
```

Linux kernel uses PID to identify processes.

Example

```
kill 1932
```

means

```
Kill process whose PID is 1932
```

---

# Problem without PID Namespace

Suppose Docker didn't exist.

```
Host

PID 1 systemd
PID 2 kthreadd
PID 200 sshd
PID 400 nginx
PID 600 postgres
PID 900 redis
PID 1500 python
```

Now inside your container

```
ps
```

shows

```
systemd
kthreadd
sshd
postgres
redis
python
...
```

Your application now knows the entire host.

This is

- bad security
    
- bad isolation
    
- not portable
    

---

# Solution

Create another PID namespace.

Now host

```
Host Namespace

PID 1 systemd
PID 2 kthreadd
PID 100 sshd
PID 400 docker
PID 800 bash
```

Container

```
Container Namespace

PID 1 bash
PID 2 python
PID 3 nginx
PID 4 redis
```

Notice

```
Host PID 800

↓

Inside container

PID 1
```

Same process.

Different PID.

This is the magic.

---

# Mental Model

Think of apartment numbers.

Building A

```
Room 1
Room 2
Room 3
```

Building B

```
Room 1
Room 2
Room 3
```

There are two Room 1s.

No conflict.

Because they belong to different buildings.

Namespaces are like buildings.

PIDs are apartment numbers.

---

# Process Trees

Linux organizes processes as trees.

Host

```
systemd (PID 1)

├── sshd
├── docker
│      ├── containerd
│      ├── container
│      │      ├── bash
│      │      ├── nginx
│      │      └── python
│
└── chrome
```

Container only sees

```
bash (PID 1)

├── nginx
└── python
```

The rest simply doesn't exist from its perspective.

---

# Why is PID 1 Special?

This is one of the most important Linux interview questions.

PID 1 is the **root of the process tree**.

Everything starts from it.

On a normal Linux system

```
PID 1

↓

systemd
```

Older Linux

```
PID 1

↓

init
```

Every other process is ultimately a child (directly or indirectly) of PID 1.

```
systemd

├── sshd
├── nginx
├── docker
└── postgres
```

---

# Responsibilities of PID 1

## 1. Starts system services

Example

```
NetworkManager

sshd

cron

docker

display manager
```

---

## 2. Adopts orphan processes

Suppose

```
Parent
   |
 Child
```

Parent dies.

Child still exists.

Linux does NOT delete the child.

Instead

```
Child

↓

adopted by PID 1
```

This prevents orphaned processes from being left unmanaged.

---

## 3. Reaps Zombie Processes

When a child exits,

Linux keeps a tiny process table entry until the parent calls `wait()` to collect the exit status.

This leftover entry is a **zombie**.

If the parent never calls `wait()`, zombies accumulate.

PID 1 is responsible for eventually cleaning up orphaned zombies.

---

# PID 1 Inside a Container

Inside Docker

```
PID 1

↓

bash
```

or

```
PID 1

↓

python
```

or

```
PID 1

↓

nginx
```

That application is now responsible for behaving like PID 1 (handling signals, reaping orphaned children). This is why many containers use a tiny init process (like `tini`) as PID 1.

---

# Same Process, Different PIDs

This confuses almost everyone initially.

Imagine

```
Host

PID 8432
```

Inside namespace

```
PID 1
```

It is **the same process**.

The kernel stores multiple PID mappings—one for each namespace.

Think of it like a person having:

- Employee ID at work
    
- Student ID at university
    
- Passport number
    

Same person.

Different identifiers depending on the context.

---

# Visual Representation

```
Host Namespace

PID 1 systemd

PID 200 docker

PID 3200 bash
```

Inside Namespace

```
PID 1 bash
```

Kernel internally knows

```
Host PID 3200

↓

Namespace PID 1
```

---

# Creating a PID Namespace

```
sudo unshare --fork --pid bash
```

Let's understand every part.

---

## unshare

Creates a new namespace.

---

## --pid

Create a new PID namespace.

---

## --fork

Why is this necessary?

The process that _creates_ a PID namespace cannot suddenly become PID 1 itself. Instead, it forks (creates a child), and that child becomes the first process in the new namespace—PID 1.

Without `--fork`, your current shell stays in the old process hierarchy, so the new PID namespace isn't useful for running a process.

---

## bash

Launch a shell inside the namespace.

---

# Why `echo $$` shows 1

```
echo $$
```

prints the PID of the current shell.

Host

```
Host PID

4210
```

Inside namespace

```
1
```

Same shell.

Different namespace.

---

# Why `ps` Looks Different

Host

```
PID CMD

1 systemd

2 kthreadd

300 sshd

900 docker

...
```

Container

```
PID CMD

1 bash

2 ps
```

Only processes inside the namespace are visible.

---

# What the Host Sees

The host still sees everything.

```
Host

PID 4210 bash

PID 4211 ps
```

Inside

```
PID 1 bash

PID 2 ps
```

Host sees all namespaces because it lives in the parent namespace.

Child namespaces cannot see unrelated parent processes.

---

# Mini Experiment

Inside the namespace

```
sleep 300 &
```

Now

```
ps
```

shows

```
PID CMD

1 bash

2 sleep

3 ps
```

Host

```
ps -ef | grep sleep
```

shows

```
4215 sleep 300
```

Host PID

```
4215
```

Namespace PID

```
2
```

Same process.

Different identities.

---

# How Docker Uses PID Namespaces

When you run

```bash
docker run ubuntu
```

Docker (through the Linux kernel and runtime like `runc`) creates a new PID namespace.

Inside the container

```
PID 1

↓

bash
```

Another container

```
PID 1

↓

node
```

Both containers have

```
PID 1
```

No conflict.

Because each has its own PID namespace.

---

# Important Properties

|Property|Explanation|
|---|---|
|Isolation|Processes only see processes in their namespace|
|Independent PID numbering|Each namespace starts numbering from PID 1|
|Hierarchical|Parent namespaces can see child namespace processes|
|Security|Containers cannot inspect unrelated host processes|
|Used by Docker|Every container gets its own process tree by default|

---

# GATE Corner ⭐

### Q1. Why is PID 1 special?

**Answer:** It is the root of the process tree, adopts orphan processes, and is responsible for reaping orphaned zombie processes.

---

### Q2. Can two processes have PID 1?

**Answer:** Yes, if they belong to different PID namespaces.

---

### Q3. Can a container see host processes?

**Answer:** No, not unrelated host processes. It only sees processes within its own PID namespace.

---

### Q4. Can the host see container processes?

**Answer:** Yes. The parent namespace can see processes in child PID namespaces.

---

### Q5. Does PID uniquely identify a process across the whole system?

**Answer:** Only within a given PID namespace. The same process may have different PID values in different namespaces.

---

# Common Interview Questions

- Why is PID 1 different from other processes?
    
- Why do Docker containers start with PID 1?
    
- Why does `ps` inside Docker show only a few processes?
    
- Why is `tini` or another init process often used as PID 1 in containers?
    
- How can the same process have two different PIDs?
    
- Why is `--fork` required with `unshare --pid`?
    

---

# Commands Learned

```bash
pstree
```

Shows the hierarchical process tree.

```bash
sudo unshare --fork --pid bash
```

Creates a new PID namespace and starts a shell as PID 1.

```bash
echo $$
```

Displays the current shell's PID in the current namespace.

```bash
ps
```

Lists processes visible in the current PID namespace.

```bash
ps -ef
```

Shows detailed process information (within the current namespace unless run from the host).

```bash
sleep 300 &
```

Starts a background process for experimentation.

```bash
exit
```

Leaves the namespace shell.

---

# Day 8 Summary

- A **PID namespace** gives a process its own isolated view of the process tree.
    
- Every PID namespace has its own **PID 1**, which acts as the root of that namespace's process hierarchy.
    
- The **same process can have different PID values** in different namespaces because the kernel maintains namespace-specific PID mappings.
    
- The **host can see processes in child PID namespaces**, but a container cannot see unrelated host processes.
    
- Docker relies on **PID namespaces** so each container behaves like an independent Linux system with its own process hierarchy.
    
- Inside a container, the process running as **PID 1** has special responsibilities, such as properly handling child processes and signals.