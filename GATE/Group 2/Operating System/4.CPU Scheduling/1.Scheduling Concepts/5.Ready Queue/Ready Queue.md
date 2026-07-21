# ➕ Ready Queue

## Definition

The **Ready Queue** is a scheduling queue that contains all processes:

- Loaded into **main memory (RAM)**.
- In the **Ready State**.
- Waiting only for **CPU allocation**.

> **Only processes in the Ready Queue are eligible for CPU scheduling.**

---

# Purpose

The Ready Queue stores processes that are prepared to execute but are waiting because the CPU is currently executing another process.

---

# Characteristics

- Located in **Main Memory (RAM)**.
- Contains **Ready** processes only.
- Managed by the **Short-Term Scheduler (CPU Scheduler)**.
- The scheduler selects one process from this queue whenever the CPU becomes available.

---

# Process Flow

```text
New
 │
 ▼
Ready Queue
 │
 │ CPU Scheduler
 ▼
Running
```

If the running process requests I/O:

```text
Running
    │
Requests I/O
    ▼
Device Queue
    │
I/O Complete
    ▼
Ready Queue
```

If the running process is preempted:

```text
Running
    │
Time Quantum Expires
    ▼
Ready Queue
```

---

# When Does a Process Enter the Ready Queue?

A process enters the Ready Queue when:

- Created and admitted to memory.
- I/O operation completes.
- Preempted by the scheduler.
- Wakes up from a waiting state.

---

# When Does a Process Leave the Ready Queue?

A process leaves the Ready Queue when:

- The CPU Scheduler selects it for execution.

---

# Managed By

**Short-Term Scheduler (CPU Scheduler)**

Its job is to select the next process from the Ready Queue.

---

# Important Facts

- Every process must pass through the Ready Queue before execution.
- A running process is **not** part of the Ready Queue.
- The Ready Queue may contain one or many processes.
- Processes are ordered according to the scheduling algorithm (FCFS, Priority, RR, etc.).

---

# Example

Suppose:

```text
P1 → Running

Ready Queue:

P2
P3
P4
```

When P1 finishes or blocks:

```text
CPU
 ↓
P2 Runs
```

The Ready Queue becomes:

```text
P3
P4
```

---

# Common Mistakes

❌ Ready Queue contains Running processes.

✔ Running processes have already left the Ready Queue.

---

❌ Ready Queue stores Blocked processes.

✔ Blocked processes stay in the Device Queue.

---

❌ Every process in memory is in the Ready Queue.

✔ Only processes in the **Ready State** are in the Ready Queue.

---

# Quick Revision

```text
Ready Queue
      │
CPU Scheduler
      │
      ▼
Running
      │
 ┌────┴────┐
 │         │
 ▼         ▼
Exit   Device Queue
           │
           ▼
      Ready Queue
```

## One-Liner

> **The Ready Queue contains all processes in main memory that are ready to execute and waiting only for CPU allocation.**