# Throughput

## Definition

**Throughput** is the **number of processes (or jobs) completed per unit time**.

It measures the **overall productivity** of the CPU scheduling system.

> In simple words:
> **Throughput tells us how much work the system finishes in a given amount of time.**

---

## Formula

$$
\boxed{
\text{Throughput}=
\frac{\text{Number of Completed Processes}}
{\text{Total Time}}
}
$$

### Units

- Processes/second
- Jobs/minute
- Tasks/hour

---

## Why Do We Need Throughput?

Suppose there are two operating systems.

### System A

```text
Completes 20 processes in 1 minute
```

### System B

```text
Completes 40 processes in 1 minute
```

System B has **higher throughput** because it completes more work in the same amount of time.

Throughput helps us measure the **efficiency and productivity** of a scheduling algorithm.

---

## Example

Suppose:

```text
50 processes are completed in 10 seconds.
```

Then,

$$
\text{Throughput}
=
\frac{50}{10}
=
5
$$

**Answer:**

```text
5 processes/second
```

---

## High vs Low Throughput

### High Throughput

- More processes completed
- Better CPU productivity
- Better performance (generally)

Example:

```text
100 processes completed in 10 seconds
```

$$
10\ \text{processes/second}
$$

---

### Low Throughput

- Fewer completed processes
- Lower system productivity

Example:

```text
100 processes completed in 50 seconds
```

$$
2\ \text{processes/second}
$$

---

## Factors Affecting Throughput

Throughput increases when:

- CPU spends less time idle.
- Context switching overhead is low.
- Scheduling algorithm efficiently utilizes the CPU.
- Processes finish quickly.

Throughput decreases when:

- CPU is idle frequently.
- Too many context switches occur.
- Long-running processes occupy the CPU for extended periods.
- Excessive waiting or blocking occurs.

---

## Throughput vs CPU Utilization

| Throughput | CPU Utilization |
|------------|-----------------|
| Measures completed processes per unit time | Measures how busy the CPU is |
| Focuses on productivity | Focuses on CPU usage |
| Unit: Processes/second | Unit: Percentage (%) |

**Example:**

A CPU may have **95% utilization** but **low throughput** if it is busy executing one very long process.

---

## Throughput vs Turnaround Time

| Throughput | Turnaround Time |
|------------|-----------------|
| System performance metric | Individual process metric |
| Number of completed processes | Time taken for one process to finish |

---

## Throughput vs Response Time

| Throughput | Response Time |
|------------|---------------|
| Measures completed work | Measures first response to the user |
| Important for batch systems | Important for interactive systems |

---

# GATE Corner ⭐

## Must Remember

- Throughput = Number of completed processes per unit time.
- Higher throughput generally indicates better overall system productivity.
- Goal of CPU scheduling is usually to **maximize throughput**.
- Throughput is a **system-wide performance metric**, not a per-process metric.

---

## Common GATE Traps

❌ Throughput ≠ CPU Utilization

❌ Throughput ≠ Turnaround Time

❌ Throughput ≠ Response Time

---

## PYQ Focus

GATE often asks:

- Formula-based numerical questions.
- Comparison between scheduling algorithms based on throughput.
- Difference between throughput, turnaround time, waiting time, and response time.

---

## Memory Trick 💡

Imagine a factory.

- **Throughput** = **Number of finished products leaving the factory per hour.**

It does **not** measure:
- How many products are inside the factory.
- How busy the workers are.

It only measures **how many products are completed and delivered**, just like an operating system measures how many processes are completed per unit time.