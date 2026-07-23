# Scheduling Criteria (Quick Notes)

## CPU Utilization

**Definition:** Percentage of time the CPU is busy executing processes.

**Goal:** Maximize CPU Utilization.

> Higher CPU utilization means less idle CPU time.

---

## Throughput

**Definition:** Number of processes completed per unit time.

$$
\text{Throughput}=\frac{\text{Completed Processes}}{\text{Total Time}}
$$

**Goal:** Maximize Throughput.

> Measures overall system productivity.

---

## Turnaround Time (TAT)

**Definition:** Total time taken by a process from arrival to completion.

$$
TAT = CT - AT
$$

**Goal:** Minimize Turnaround Time.

> Measures how long a process stays in the system.

---

## Waiting Time (WT)

**Definition:** Total time a process spends waiting in the Ready Queue.

$$
WT = TAT - BT
$$

**Goal:** Minimize Waiting Time.

> Measures how long a process waits before getting CPU time.

---

## Response Time (RT)

**Definition:** Time from process arrival until it gets the CPU for the **first time**.

$$
RT = \text{First Start Time} - AT
$$

**Goal:** Minimize Response Time.

> Important for interactive systems.

---

## Fairness

**Definition:** Every process should get a fair opportunity to execute.

**Goal:** Prevent indefinite postponement of any process.

> No process should be ignored forever.

---

## Starvation

**Definition:** A process waits indefinitely because other processes are continuously given higher priority.

**Common In:**
- Priority Scheduling
- SJF (possible)

**Goal:** Avoid Starvation.

---

## Aging

**Definition:** A technique to prevent starvation by gradually increasing the priority of waiting processes.

**Used In:** Priority Scheduling.

> Longer a process waits, higher its priority becomes.

---

# GATE One-Liners ⭐

- **CPU Utilization** → CPU busy percentage.
- **Throughput** → Completed processes per unit time.
- **Turnaround Time** → Arrival → Completion.
- **Waiting Time** → Time spent in Ready Queue.
- **Response Time** → Arrival → First CPU allocation.
- **Fairness** → Every process gets a chance.
- **Starvation** → Process waits forever.
- **Aging** → Prevents starvation by increasing priority over time.