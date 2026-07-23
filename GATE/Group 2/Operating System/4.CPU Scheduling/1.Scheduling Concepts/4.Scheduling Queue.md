# Scheduling Queues

## Definition

A **Scheduling Queue** is a data structure maintained by the Operating System that stores processes waiting for a specific resource or event.

> **Keywords:** Process Management • Waiting • Scheduling

---

# Why Scheduling Queues?

A process cannot execute all the time. Depending on its state, it waits in different queues until it becomes eligible to run.

Queues help the OS efficiently manage process execution.

---

# Types of Scheduling Queues

1. Job Queue (New Queue)
2. Ready Queue
3. Device Queue (Waiting Queue)

---

# 1. Job Queue (New Queue)

## Definition

The **Job Queue** contains all processes that have been submitted to the system.

These processes may or may not be loaded into main memory.

### Managed By

- **Long-Term Scheduler (Job Scheduler)**

### Purpose

- Controls the degree of multiprogramming.
- Selects which jobs enter main memory.

---

# 2. Ready Queue ⭐

## Definition

The **Ready Queue** contains processes that:

- Are loaded into main memory.
- Are ready to execute.
- Are waiting only for CPU allocation.

> **Only processes in the Ready Queue can be scheduled for execution.**

### Managed By

- **Short-Term Scheduler (CPU Scheduler)**

---

# 3. Device Queue (Waiting Queue)

## Definition

A **Device Queue** contains processes waiting for a specific I/O device or event.

Examples:

- Disk Queue
- Printer Queue
- Keyboard Queue
- Network Queue

### Managed By

- Device Scheduler / Operating System

---

# Process Flow

```text
New
 │
 ▼
Job Queue
 │
 ▼
Ready Queue
 │
 ▼
Running
 │
 ├── I/O Request
 ▼
Device Queue
 │
 ├── I/O Complete
 ▼
Ready Queue
 │
 ▼
Running
 │
 ▼
Terminated
```

---

# Queue Responsibilities

| Queue | Waiting For | Process State |
|---------|-------------|---------------|
| Job Queue | Admission to Memory | New |
| Ready Queue | CPU | Ready |
| Device Queue | I/O Completion | Waiting (Blocked) |

---

# Ready Queue vs Device Queue

| Ready Queue | Device Queue |
|--------------|--------------|
| Waiting for CPU | Waiting for I/O Device |
| Ready State | Waiting (Blocked) State |
| Can Be Scheduled | Cannot Be Scheduled |
| Managed by CPU Scheduler | Managed by Device Scheduler |

---

# Important Facts

- Every process enters the **Ready Queue** before execution.
- A running process leaves the Ready Queue.
- After an I/O request, the process moves to a **Device Queue**.
- After I/O completion, it returns to the **Ready Queue**.
- A system has **one Ready Queue** but may have **multiple Device Queues**.

---

# Scheduler Association

| Queue | Scheduler |
|---------|-----------|
| Job Queue | Long-Term Scheduler |
| Ready Queue | Short-Term Scheduler |
| Device Queue | Device Scheduler / OS |

---

# Process Lifecycle with Queues

```text
Job Queue
      │
      ▼
Ready Queue
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

---

# Common Mistakes

❌ Ready Queue contains running processes.

✔ The running process has already left the Ready Queue and is executing on the CPU.

---

❌ Blocked processes remain in the Ready Queue.

✔ They move to the appropriate Device Queue.

---

❌ There is only one Device Queue.

✔ Each I/O device can have its own Device Queue.

---

# Quick Revision

```text
Job Queue
    │
    ▼
Ready Queue
    │
    ▼
Running
 │      │
 │      ▼
 │  Device Queue
 │      │
 └──────┘
    │
    ▼
Terminated
```

## One-Liner

> **Scheduling Queues organize processes based on what they are waiting for—admission, CPU allocation, or I/O completion.**