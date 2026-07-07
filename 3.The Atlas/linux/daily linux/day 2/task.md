## 🐧 Linux Internals – Day 2 (30–60 Minutes)

**Today's topic:** **Processes**

Since you completed **Linux Architecture**, today you'll learn how Linux actually runs programs.

### 📖 Theory (10–15 min)

Focus on these ideas:

- What is a **program**?
- What is a **process**?
- Process ID (PID)
- Parent and Child processes
- Process lifecycle (Created → Running → Sleeping → Stopped → Zombie)

### 💻 Hands-on Lab (15–20 min)

Run these commands and observe the output:

```
echo $$
```

Shows the PID of your current shell.

```
ps
```

Lists processes attached to your current terminal.

```
ps -ef
```

Shows all running processes.

```
pstree
```

Visualizes the parent-child process hierarchy.

```
top
```

Watch processes running in real time. Press `q` to quit.

---

### 🧠 Mental Model

Think of Linux like this:

```
Program (stored on disk)          │      Executed          │          ▼Process (running in RAM)
```

A **program** is just a file on disk.

A **process** is that program **currently executing**, with its own memory, CPU state, open files, and PID.

---

### 📝 Quick Revision (5–10 min)

Write these three points in your notes:

- A **program** is passive (stored on disk); a **process** is an active, running instance.
- Every running process has a unique **PID**.
- Linux organizes processes in a **parent-child tree**.

---

### 🎯 Goal for Today

By the end of today's session, you should be able to answer:

- What is the difference between a **program** and a **process**?
- What is a **PID**?
- Why does Linux organize processes as a tree?

Tomorrow you'll build on this by learning **how Linux creates processes** using `fork()` and `exec()`, which are two of the most important system calls in the operating system.