# 🎯 GATE Corner – I/O Burst

## Definition

> **An I/O Burst is the continuous period during which a process waits for an I/O operation instead of executing on the CPU.**

---

# ⭐ Most Important GATE Points

### 1. Process State

During an **I/O Burst**, a process is in the:

✅ **Waiting (Blocked) State**

---

### 2. CPU Usage

During an I/O Burst:

❌ Process does **not** use the CPU.

✅ CPU is allocated to another Ready process.

---

### 3. Scheduling

A process in the **Waiting State cannot be scheduled.**

Only processes in the **Ready Queue** are eligible for CPU scheduling.

---

### 4. State Transition

```text
Running
    │
Requests I/O
    ▼
Waiting (Blocked)
    │
I/O Complete
    ▼
Ready
    ▼
Running
```

---

### 5. Why CPU Scheduling Exists?

When one process enters an I/O Burst,

➡️ another Ready process gets the CPU.

This improves:

- CPU Utilization
- Throughput
- System Performance

---

### 6. CPU Burst vs I/O Burst

| CPU Burst | I/O Burst |
|------------|-----------|
| Computation | Waiting |
| Running State | Waiting State |
| Uses CPU | Doesn't use CPU |
| CPU Busy | CPU Free for Other Processes |

---

### 7. CPU-bound vs I/O-bound

| CPU-bound | I/O-bound |
|------------|------------|
| Long CPU Bursts | Long I/O Bursts |
| Short I/O Bursts | Short CPU Bursts |

---

# Previous GATE Questions

### Q1. During an I/O Burst, which state is the process in?

✅ Waiting (Blocked)

---

### Q2. Can a blocked process be scheduled?

❌ No

---

### Q3. What happens after I/O completion?

**Waiting → Ready → Running**

---

### Q4. During an I/O Burst, what does the CPU do?

✅ Executes another process from the Ready Queue.

---

### Q5. Which scheduler selects another process when the current process enters an I/O Burst?

✅ **Short-Term Scheduler (CPU Scheduler)**

---

### Q6. Does an I/O Burst consume CPU time?

❌ No

---

### Q7. Why does an I/O-bound process usually have better response time?

✅ Because it has **short CPU Bursts** and frequently releases the CPU, allowing interactive tasks to complete quickly.

---

# Common GATE Traps

❌ Ready State = Waiting State

✔ Ready → Waiting for CPU

✔ Waiting → Waiting for I/O Event

---

❌ CPU waits during an I/O Burst.

✔ The **process waits**, not the CPU.

---

❌ Waiting processes compete for CPU.

✔ Only **Ready** processes compete for CPU.

---

❌ I/O Burst uses CPU cycles.

✔ I/O Burst uses **I/O devices**, not CPU cycles.

---

# Memory Tricks

🧠 **CPU Burst = Compute**

🧠 **I/O Burst = Wait**

🧠 **Ready = Waiting for CPU**

🧠 **Waiting = Waiting for I/O**

🧠 **Blocked Process ≠ Schedulable Process**

---

# Expected GATE Questions

- Difference between **Ready** and **Waiting** states.
- What happens when a process requests I/O?
- Which process can be scheduled?
- CPU behavior during an I/O Burst.
- CPU-bound vs I/O-bound process comparison.
- Process state transition after I/O completion.