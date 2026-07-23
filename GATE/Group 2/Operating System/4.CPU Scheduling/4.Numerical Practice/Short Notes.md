# CPU Scheduling Numerical Cheat Sheet

## 1. Gantt Chart

A **Gantt Chart** is a timeline showing the order in which processes execute on the CPU.

Example:

```text
0      4      7      10
|------|------|-------|
  P1      P2      P3
```

It is the first step in solving almost every scheduling problem.

---

## 2. Completion Time (CT)

**Completion Time (CT)** is the time at which a process finishes execution.

Example:

If P1 finishes at **8 ms**,

```text
CT(P1) = 8 ms
```

---

## 3. Turnaround Time (TAT)

**Definition:** Total time from process arrival until completion.

$$
TAT = CT - AT
$$

Goal:

- Minimize Turnaround Time.

---

## 4. Waiting Time (WT)

**Definition:** Total time a process spends waiting in the Ready Queue.

$$
WT = TAT - BT
$$

Goal:

- Minimize Waiting Time.

---

## 5. Response Time (RT)

**Definition:** Time from process arrival until it gets the CPU **for the first time**.

$$
RT = \text{First Start Time} - AT
$$

Goal:

- Minimize Response Time.

---

## 6. Average Waiting Time

$$
\boxed{
\text{Average WT}
=
\frac{\sum WT}{\text{Number of Processes}}
}
$$

---

## 7. Average Turnaround Time

$$
\boxed{
\text{Average TAT}
=
\frac{\sum TAT}{\text{Number of Processes}}
}
$$

---

## 8. Average Response Time

$$
\boxed{
\text{Average RT}
=
\frac{\sum RT}{\text{Number of Processes}}
}
$$

---

## 9. CPU Utilization

**Definition:** Percentage of time the CPU is busy executing processes.

$$
\boxed{
CPU\ Utilization
=
\frac{\text{CPU Busy Time}}
{\text{Total Time}}
\times100\%
}
$$

Higher CPU Utilization is better.

---

## 10. Throughput

**Definition:** Number of processes completed per unit time.

$$
\boxed{
Throughput
=
\frac{\text{Completed Processes}}
{\text{Total Time}}
}
$$

Higher Throughput is better.

---

## 11. Context Switch Overhead

A **Context Switch** occurs when the CPU switches from one process to another.

Every context switch consumes CPU time but performs **no useful work**.

Effects:

- Decreases CPU Utilization.
- Decreases Throughput.
- Increases Waiting Time.

Round Robin generally has the highest context-switch overhead because it switches processes frequently.

---

# Formula Summary ⭐

$$
CT = \text{Finish Time}
$$

$$
TAT = CT - AT
$$

$$
WT = TAT - BT
$$

$$
RT = \text{First Start Time} - AT
$$

$$
Average\ WT = \frac{\sum WT}{n}
$$

$$
Average\ TAT = \frac{\sum TAT}{n}
$$

$$
Average\ RT = \frac{\sum RT}{n}
$$

$$
CPU\ Utilization =
\frac{CPU\ Busy\ Time}{Total\ Time}
\times100\%
$$

$$
Throughput =
\frac{\text{Completed Processes}}
{\text{Total Time}}
$$

---

# GATE Tips ⭐

- Always **draw the Gantt Chart first**.
- Calculate **CT** first, then **TAT**, then **WT**, and finally **RT**.
- In **non-preemptive algorithms**, **RT = WT**.
- In **preemptive algorithms**, **RT may be different from WT**.
- More context switches ⇒ Higher overhead.
- Small Time Quantum in Round Robin ⇒ More context switches.