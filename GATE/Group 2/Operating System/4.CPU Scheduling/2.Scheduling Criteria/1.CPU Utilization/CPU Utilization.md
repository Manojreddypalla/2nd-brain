# CPU Utilization

## Definition

**CPU Utilization** is the **percentage of time the CPU is busy executing processes instead of remaining idle.**

> It measures how efficiently the CPU is being used.

---

# Formula

\[
\boxed{\text{CPU Utilization}=\frac{\text{CPU Busy Time}}{\text{Total Time}}\times100\%}
\]

Where:

- **CPU Busy Time** = Time spent executing processes.
- **Total Time** = Busy Time + Idle Time.

---

# Why is CPU Utilization Important?

The CPU is one of the most expensive resources in a computer system.

The goal of the Operating System is to keep the CPU busy as much as possible.

A busy CPU means:

- Better resource utilization.
- Improved system efficiency.
- Less idle time.

---

# How CPU Utilization Increases

Suppose Process P1 requests an I/O operation.

```text
Running P1
      │
      ▼
Waiting (I/O)
```

Instead of leaving the CPU idle, the Operating System schedules another ready process.

```text
P1 → Waiting

CPU → P2
```

Thus, the CPU remains busy.

---

# Multiprogramming

Without Multiprogramming:

```text
P1 Running
      │
      ▼
P1 Waiting
      │
      ▼
CPU Idle
```

With Multiprogramming:

```text
P1 Waiting
      │
      ▼
CPU Runs P2
      │
      ▼
CPU Runs P3
```

This significantly improves CPU Utilization.

---

# Factors that Increase CPU Utilization

- Multiprogramming
- Efficient CPU Scheduling
- More Ready Processes
- Faster I/O Devices
- Reduced CPU Idle Time

---

# Factors that Decrease CPU Utilization

- Frequent I/O Waiting
- Poor Scheduling Algorithms
- Few Processes in the Ready Queue
- Long Idle Periods

---

# Example

Suppose:

- CPU Busy Time = 8 seconds
- CPU Idle Time = 2 seconds

Total Time

```text
8 + 2 = 10 seconds
```

CPU Utilization

```text
(8 / 10) × 100 = 80%
```

---

# Relationship with CPU Scheduling

A good scheduling algorithm attempts to:

- Keep the CPU busy.
- Reduce idle time.
- Schedule another ready process whenever the current process blocks for I/O.

---

# Important Facts

- CPU Utilization is a **performance metric** used to evaluate scheduling algorithms.
- Higher CPU Utilization generally indicates better resource usage.
- CPU Utilization alone does not determine overall system performance.

---

# CPU Utilization vs Throughput

| CPU Utilization | Throughput |
|-----------------|------------|
| Measures how busy the CPU is | Measures the number of completed processes per unit time |
| Expressed as a percentage | Expressed as processes per unit time |

---

# CPU Utilization vs Response Time

High CPU Utilization does **not** always imply better Response Time.

A CPU may be fully occupied by a single long-running process, causing interactive processes to wait.

Therefore, scheduling algorithms aim to balance:

- CPU Utilization
- Throughput
- Response Time
- Turnaround Time
- Waiting Time

---

# Common Mistakes

❌ CPU Utilization is CPU speed.

✔ CPU Utilization measures **how busy** the CPU is.

---

❌ Higher CPU Utilization always means better performance.

✔ High Utilization may still result in poor responsiveness if scheduling is inefficient.

---

❌ CPU Utilization and Throughput are the same.

✔ They measure different aspects of system performance.

---

# Quick Revision

```text
CPU Busy Time
        │
        ▼
Total Time
        │
        ▼
(Busy / Total) × 100
        │
        ▼
CPU Utilization
```

## One-Liner

> **CPU Utilization is the percentage of total time during which the CPU is actively executing processes rather than remaining idle.**