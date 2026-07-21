# 🎯 GATE Corner – Scheduling Queues

## Definition

A **Scheduling Queue** is a queue maintained by the Operating System to organize processes based on what they are waiting for.

---

# ⭐ Most Important GATE Points

### 1. Job Queue

- Contains **all submitted jobs/processes**.
- Some processes may not yet be loaded into main memory.
- Managed by the **Long-Term Scheduler**.

---

### 2. Ready Queue ⭐

- Contains processes that are:
  - In main memory.
  - Ready to execute.
  - Waiting only for the CPU.

> **Only processes in the Ready Queue can be selected by the CPU Scheduler.**

---

### 3. Device Queue

- Contains processes waiting for a specific I/O device.
- Processes are in the **Waiting (Blocked)** state.
- Cannot be scheduled until I/O completes.

---

### 4. Process Movement

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
 ├── Requests I/O
 ▼
Device Queue
 │
 ├── I/O Completed
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

### 5. Scheduler Association

| Queue | Scheduler |
|---------|-----------|
| Job Queue | Long-Term Scheduler |
| Ready Queue | Short-Term Scheduler (CPU Scheduler) |
| Device Queue | Device Scheduler / OS |

---

### 6. Queue vs Process State

| Queue | State |
|---------|-------|
| Job Queue | New |
| Ready Queue | Ready |
| Device Queue | Waiting (Blocked) |

---

# Previous GATE Questions

### Q1. Which queue is used by the CPU Scheduler?

✅ Ready Queue

---

### Q2. Can a process in the Device Queue be scheduled?

❌ No

---

### Q3. What happens after an I/O operation completes?

✅ Device Queue → Ready Queue

---

### Q4. Which scheduler selects processes from the Job Queue?

✅ Long-Term Scheduler

---

### Q5. Which scheduler selects processes from the Ready Queue?

✅ Short-Term Scheduler (CPU Scheduler)

---

### Q6. Can a Running process be inside the Ready Queue?

❌ No

A running process has already been removed from the Ready Queue.

---

### Q7. How many Ready Queues are generally maintained?

✅ One

---

### Q8. How many Device Queues can exist?

✅ Multiple (one for each I/O device)

---

# Common GATE Traps

❌ Ready Queue stores Running processes.

✔ Running processes have already left the Ready Queue.

---

❌ Waiting processes stay in the Ready Queue.

✔ They move to the appropriate Device Queue.

---

❌ Every queue contains Ready processes.

✔ Only the Ready Queue contains Ready processes.

---

❌ Device Queue is managed by the CPU Scheduler.

✔ Device Queues are managed by the Device Scheduler / Operating System.

---

# Memory Tricks

🧠 **Job Queue → Waiting to Enter Memory**

🧠 **Ready Queue → Waiting for CPU**

🧠 **Device Queue → Waiting for I/O Device**

🧠 **Ready Queue = Only Schedulable Queue**