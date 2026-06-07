 # 🚀 **PROJECT PLAN — “System Resource Monitor (Mini htop)”**

> 📚 For B.Tech — Operating Systems Lab (Units IV–VI Integration Project)

---

## 🎯 **Project Objective**

Develop a **CLI-based system resource monitoring daemon** that:

- Reads **live CPU, memory, and process usage** using `/proc`.
- Uses **process creation (fork)** for concurrency.
- Uses **shared memory & semaphores** for synchronization.
- Uses **signals** for safe cleanup and termination.

---

## 🧩 **Modules Overview**

|**Phase**|**Module Name**|**Goal**|**Concepts / Units Covered**|
|---|---|---|---|
|**Phase 1**|**System Data Collection**|Read CPU, Memory, and Uptime from `/proc`|`/proc/stat`, `/proc/meminfo`, `/proc/uptime` (Kernel interface)|
|**Phase 2**|**Live CLI Stream (Mini htop)**|Display live resource stats updating every second|File I/O, loops, ANSI colors, sleep()|
|**Phase 3**|**Process Forking**|Split monitor into **Parent (Display)** and **Child (Collector)**|`fork()`, `getpid()`, `wait()` — _Unit IV_|
|**Phase 4**|**Interprocess Communication (IPC)**|Exchange collected data between parent & child|**Shared Memory, Message Queues, Pipes** — _Unit V_|
|**Phase 5**|**Synchronization**|Avoid data conflicts in shared memory|**Semaphores** — _Unit VI_|
|**Phase 6**|**Signal Handling & Cleanup**|Graceful exit and cleanup|`signal()`, `kill()`, `SIGINT` — _Unit IV_|
|**Phase 7**|**Final Daemon Integration**|Merge all modules into one background service|Daemonization + Continuous monitoring|

---

## 🧠 **Detailed Workflow**

```
┌───────────────────────────────────────────────┐
│               START MONITOR                   │
└───────────────────────────────────────────────┘
                     │
                     ▼
     ┌────────────────────────────────┐
     │ Read system data from /proc    │
     │ → /proc/stat (CPU usage)       │
     │ → /proc/meminfo (RAM usage)    │
     │ → /proc/loadavg (process info) │
     └────────────────────────────────┘
                     │
                     ▼
     ┌────────────────────────────────┐
     │ fork() a child process         │
     │ → Child collects data          │
     │ → Parent displays dashboard    │
     └────────────────────────────────┘
                     │
                     ▼
     ┌────────────────────────────────┐
     │ Use shared memory (shmget, shmat) │
     │ to pass data between parent-child │
     └────────────────────────────────┘
                     │
                     ▼
     ┌────────────────────────────────┐
     │ Use semaphores (semget, semop)  │
     │ to lock data access             │
     └────────────────────────────────┘
                     │
                     ▼
     ┌────────────────────────────────┐
     │ Handle Ctrl+C signal           │
     │ → Remove shared memory segment │
     │ → Clean up semaphores safely   │
     └────────────────────────────────┘
                     │
                     ▼
             ┌─────────────────┐
             │ Continuous Loop │
             │ until SIGINT    │
             └─────────────────┘

```

---

## 🧱 **Phase-by-Phase Breakdown**

### **Phase 1 — Data Collection**

**Goal:** Learn to read `/proc`

**Code Files:** `read_cpu.c`, `read_mem.c`, `read_uptime.c`

**Functions:**

- `fopen()`, `fscanf()`, `fclose()`
- Use `%llu` for big counters
- Calculate usage with time deltas

✅ _Output:_ CPU %, Memory %, Uptime printed once.

---

### **Phase 2 — Live CLI Stream (Done ✅)**

**Goal:** Display continuously refreshing stats

**Code File:** `live_monitor.c`

**New Additions:**

- `system("clear")` for live refresh
- ANSI color codes for styling
- `while(1)` + `sleep(1)` loop

✅ _Output:_

```
CPU Usage: 22.5%
Memory Usage: 48.7%
Uptime: 02:43:12

```

(updated every second)

---

### **Phase 3 — Fork & Dual Process Mode**

**Goal:** Separate tasks using `fork()`

**File:** `monitor_fork.c`

**Behavior:**

- Parent = UI updater
- Child = Data collector

**Functions Used:**

`fork()`, `getpid()`, `getppid()`, `wait()`, `_exit()`

✅ _Concept Link:_ Unit IV — Process creation, parent-child communication.

---

### **Phase 4 — IPC: Shared Memory Communication**

**Goal:** Share data collected by child with parent

**File:** `monitor_ipc.c`

**Functions Used:**

- `shmget()`, `shmat()`, `shmdt()`, `shmctl()`

**Shared Data Structure Example:**

```c
struct sysinfo {
    float cpu;
    float mem;
    int procs;
};

```

Child writes → Parent reads from shared memory.

✅ _Concept Link:_ Unit V — Shared memory & message queues.

---

### **Phase 5 — Synchronization with Semaphores**

**Goal:** Ensure safe concurrent access to shared memory

**Functions:**

`semget()`, `semop()`, `semctl()`

**Operations:**

- `P()` → Wait (lock)
- `V()` → Signal (unlock)

Used before read/write:

```c
sem_wait(semid);
    // read/write shared memory
sem_signal(semid);

```

✅ _Concept Link:_ Unit VI — Semaphores and mutual exclusion.

---

### **Phase 6 — Signal Handling & Cleanup**

**Goal:** Handle termination gracefully

**Functions:**

`signal(SIGINT, handler)`

`shmctl(IPC_RMID)`, `semctl(IPC_RMID)`

✅ _Concept Link:_ Unit IV — Signals & process control.

✅ _Behavior:_

When Ctrl + C pressed:

```
→ Stop child
→ Remove shared memory
→ Remove semaphore
→ Exit cleanly

```

---

### **Phase 7 — Final Daemonization**

**Goal:** Run as background system monitor service

**Steps:**

- Use `fork()` again to detach from terminal
- Change working directory `/`
- Redirect standard I/O to `/dev/null`

✅ _Output:_


Continuous logging or periodic printing to file.





# ⚙️ **1️⃣ What the First App Can Do**

Your **Phase 1–2** (single-process monitor) can:

- Read `/proc/stat`, `/proc/meminfo`, `/proc/uptime`
- Calculate CPU, memory, uptime
- Print them to screen
- Refresh every 1 second

✅ It _works perfectly fine_ as a **demo** or personal tool.

But inside the system, it’s very _linear_ —

it does:

```
collect → print → sleep → collect → print → sleep → ...

```

That means during **collection**, it can’t display,

and during **display**, it can’t collect.

It’s doing one thing at a time.

---

# ⚙️ **2️⃣ What the Forked App Adds**

When you introduce `fork()` (Phase 3):

- The **child process** runs the “collector loop” — reading `/proc`, computing CPU/memory continuously.
- The **parent process** just reads data from the child (or shared memory) and displays it.

Now both work **in parallel**.

So the system looks like this:

```
┌──────────────┐
│ Parent (UI)  │
│  • Prints live output   │
│  • Handles signals      │
└───────┬────────┘
        │
        ▼
┌──────────────┐
│ Child (Data) │
│  • Reads /proc/stat    │
│  • Calculates CPU, MEM │
└──────────────┘

```

---

# ⚡ **3️⃣ Why That’s Better (Even if 1st Worked)**

### ✅ a) **Non-blocking Design**

The parent and child can run simultaneously.

→ Parent doesn’t wait for the child to finish collecting.

→ Child continuously updates shared memory in the background.

→ Parent displays instantly — **smoother updates**.

This is how tools like `htop`, `atop`, `vmstat` work.

---

### ✅ b) **Foundation for Interprocess Communication (Unit V)**

Without `fork()`, there’s only one process — so IPC (shared memory, semaphores, pipes) **can’t happen**.

Forking creates **two independent processes**, which is the minimum requirement for:

- Shared Memory (shmget, shmat)
- Message Queues (msgget, msgsnd)
- Semaphores (semget, semop)
- Pipes or FIFOs

So even if your monitor “works,” it doesn’t demonstrate OS-level process communication — which is essential for your syllabus and project scope.

---

### ✅ c) **Scalable / Modular Design**

You can later extend this into:

- A **daemon** (child keeps collecting data even after terminal closes)
- A **client-server** model (client asks for system info, server replies)
- A **multi-client system** (several terminals watching the same data)

Without forking, all of that is impossible — it’s just one process doing everything.

---

### ✅ d) **True OS Concept Demonstration**

Your Unit IV–VI topics revolve around:

- _Process Creation_ → `fork()`
- _IPC_ → `shmget`, `msgget`
- _Synchronization_ → `semget`
- _Signals_ → `kill`, `raise`, `alarm`

All of these **need multiple processes**.

So Phase 3 isn’t for performance alone —

it’s to **bridge your working monitor to all OS-level concepts** you must demonstrate.

---

### ✅ e) **Real-World Relevance**

In actual systems:

- One process (service) collects metrics → stores them in shared memory.
- Another process (UI) reads them → displays or logs.

Example:

|Real Tool|Parent|Child|
|---|---|---|
|**htop**|UI thread|Stats collector|
|**systemd**|Manager|Worker daemons|
|**collectd**|Daemon|Plugin readers|
|**Chrome**|Main process|Renderer processes|

So, `fork()` models the **architecture** used in real production systems.

---

# 🧩 **4️⃣ In Short**

|Aspect|Single Process|With fork()|
|---|---|---|
|Simplicity|Easy to write|More structure|
|Flexibility|Limited|Can grow with IPC, semaphores|
|Performance|OK|Better responsiveness|
|Concept Coverage|Basic Linux file I/O|Complete OS concept demo|
|Use Case|Small utility|Real background monitor|

---

# 🎯 **When to Use Each**

|Situation|Recommended|
|---|---|
|Learning `/proc` basics|Single-process|
|Building simple demo|Single-process|
|Implementing OS project / daemon|Multi-process (with fork)|
|Demonstrating IPC, semaphores|Needs fork|
|Handling multiple jobs concurrently|Needs fork|

---

# 🧠 **Conclusion**

> Your first app proved the concept — how to read kernel data.
> 
> Your second app (with `fork()`) **proves the system** — how to manage, communicate, and synchronize multiple processes like a real OS service.****
