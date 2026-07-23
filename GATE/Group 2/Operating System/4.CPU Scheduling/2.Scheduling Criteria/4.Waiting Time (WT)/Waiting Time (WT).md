# Waiting Time (WT)

## Definition

**Waiting Time (WT)** is the **total time a process spends waiting in the Ready Queue for CPU execution.**

In simple words:

> **Waiting Time is the time during which a process is ready to run but is waiting because another process is using the CPU.**

---

## Formula

If there is no I/O,

$$
\boxed{
WT = TAT - BT
}
$$

where,

- WT = Waiting Time
- TAT = Turnaround Time
- BT = Burst Time

---

## Why Do We Need Waiting Time?

A process may not get the CPU immediately after it arrives.

If another process is already executing, it has to wait in the **Ready Queue**.

The longer it waits, the poorer the scheduling algorithm performs.

Therefore, one of the main goals of CPU scheduling is to **minimize Waiting Time**.

---

## Timeline

```text
Arrival
   │
   ▼
+---------+-----------------+
| Waiting |     Running     |
+---------+-----------------+
            │
            ▼
       Completion
```

Only the time spent in the **Ready Queue** is counted as Waiting Time.

---

## Example 1

Suppose:

```text
Arrival Time = 0 ms

Burst Time = 6 ms

CPU starts at = 4 ms
```

Timeline:

```text
0          4          10
|----------|-----------|
 Arrival   CPU      Completion
```

The process waits from **0 ms to 4 ms**.

Therefore,

$$
WT = 4\text{ ms}
$$

---

## Example 2

Suppose:

```text
Arrival Time = 2 ms

Completion Time = 15 ms

Burst Time = 8 ms
```

First calculate Turnaround Time:

$$
TAT = 15 - 2 = 13\text{ ms}
$$

Now,

$$
WT = TAT - BT
$$

$$
WT = 13 - 8 = 5\text{ ms}
$$

---

## What is NOT Included?

Waiting Time **does not include**:

- CPU execution time
- Time after process completion

It includes only the time spent waiting in the **Ready Queue**.

---

## High vs Low Waiting Time

### Low Waiting Time

- Process gets CPU quickly.
- Better responsiveness.
- Better user experience.

---

### High Waiting Time

- Process waits longer before execution.
- Poor scheduling performance.
- Can lead to starvation.

---

## Waiting Time vs Turnaround Time

| Waiting Time | Turnaround Time |
|--------------|-----------------|
| Time spent waiting for the CPU | Total time from arrival to completion |
| Excludes execution time | Includes execution time |

Relationship:

$$
\boxed{
TAT = WT + BT
}
$$

or

$$
\boxed{
WT = TAT - BT
}
$$

---

## Waiting Time vs Response Time

| Waiting Time | Response Time |
|--------------|---------------|
| Total waiting in the Ready Queue | Time until the process gets the CPU for the first time |

In **non-preemptive scheduling**, Waiting Time and Response Time are often equal.

In **preemptive scheduling**, they can be different because a process may wait multiple times.

---

## Factors Affecting Waiting Time

Waiting Time increases when:

- Long CPU bursts occupy the CPU.
- Scheduling algorithm is inefficient.
- Many processes compete for the CPU.

Waiting Time decreases when:

- CPU scheduling is efficient.
- Shorter processes are executed appropriately.
- CPU remains well utilized.

---

# GATE Corner ⭐

## Must Remember

- Waiting Time = Time spent **only in the Ready Queue**.
- Goal of CPU scheduling is to **minimize Waiting Time**.
- Waiting Time does **not** include CPU execution time.

---

## Formula

$$
\boxed{
WT = TAT - BT
}
$$

or

$$
\boxed{
TAT = WT + BT
}
$$

---

## Common GATE Traps

❌ Waiting Time ≠ Turnaround Time

❌ Waiting Time ≠ Response Time

❌ Waiting Time ≠ Burst Time

---

## PYQ Focus

GATE frequently asks:

- Calculate Waiting Time using Gantt charts.
- Compute Average Waiting Time.
- Compare scheduling algorithms based on Average Waiting Time.
- Use the relation:

$$
WT = TAT - BT
$$

---

## Memory Trick 💡

Imagine you're waiting at a ticket counter.

```text
You arrive
     │
     ▼
Wait in line
     │
     ▼
Reach the counter
     │
     ▼
Ticket issued
```

The **Waiting Time** is **only the time spent standing in the queue**.

The time the clerk spends issuing your ticket is **not** Waiting Time—it is equivalent to the **Burst Time**.