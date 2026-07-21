# 🎯 GATE Corner – CPU Burst

## Definition

A **CPU Burst** is the continuous period during which a process executes instructions on the CPU before requesting an I/O operation or terminating.

---

## Key Facts

- CPU Burst = CPU Computation Time
- I/O Burst = Waiting for I/O Devices
- Every process alternates between CPU Bursts and I/O Bursts.
- A process usually consists of **multiple CPU Bursts**.
- CPU Burst lengths are **not constant**.

---

## CPU-I/O Burst Cycle

```text
CPU Burst
    ↓
I/O Burst
    ↓
CPU Burst
    ↓
I/O Burst
    ↓
CPU Burst
```

---

## CPU-bound vs I/O-bound

| CPU-bound | I/O-bound |
|------------|------------|
| Long CPU Bursts | Short CPU Bursts |
| Short I/O Bursts | Long I/O Bursts |
| More Computation | More Waiting |
| CPU Intensive | I/O Intensive |

Examples:

**CPU-bound**
- AI Training
- Video Rendering
- Scientific Computing
- Encryption

**I/O-bound**
- Browser
- File Download
- Database Query
- Text Editor

---

## CPU Burst Prediction

Future CPU Burst is **unknown**.

Operating System estimates it using **Exponential Averaging**.

### Formula

\[
\tau_{n+1}=\alpha t_n+(1-\alpha)\tau_n
\]

Where

- τₙ₊₁ → Predicted Next CPU Burst
- tₙ → Actual Current CPU Burst
- τₙ → Previous Prediction
- α → Smoothing Constant (0 ≤ α ≤ 1)

---

## Relation with Scheduling Algorithms

| Algorithm | Uses CPU Burst? |
|------------|-----------------|
| FCFS | ❌ No |
| SJF | ✅ Yes |
| SRTF | ✅ Remaining CPU Burst |
| Priority | ❌ No |
| Round Robin | ❌ Time Quantum |
| MLFQ | Indirectly |

---

## Why is CPU Burst Important?

CPU Burst length helps the scheduler:

- Reduce Waiting Time
- Increase Throughput
- Improve CPU Utilization

---

## Important Observation

Most CPU Bursts are:

✅ Very Short

Very few are long.

This is why **Shortest Job First (SJF)** achieves the **minimum average waiting time**.

---

# Previous GATE Concepts

### Q1. Can the Operating System know the next CPU Burst exactly?

**Answer:** ❌ No

It predicts the burst using exponential averaging.

---

### Q2. Which scheduling algorithm uses CPU Burst length?

**Answer:** SJF and SRTF.

---

### Q3. Which process generally has long CPU Bursts?

**Answer:** CPU-bound Process.

---

### Q4. Which process frequently enters the Waiting state?

**Answer:** I/O-bound Process.

---

### Q5. Why does a process have multiple CPU Bursts?

**Answer:** Because processes repeatedly alternate between computation and I/O operations.

---

### Q6. Why is CPU Scheduling necessary?

**Answer:** When one process enters an I/O Burst (Waiting State), another ready process can utilize the CPU, improving CPU Utilization.

---

# Common GATE Traps

❌ CPU Burst = Total Execution Time

✔ CPU Burst is only one computation phase of a process.

---

❌ CPU-bound processes rarely use the CPU.

✔ CPU-bound processes spend most of their execution time on the CPU.

---

❌ SJF knows CPU Burst exactly.

✔ SJF estimates future CPU Burst using prediction techniques.

---

# Memory Tricks

🧠 **Burst = Continuous Activity**

- CPU Burst → Computing
- I/O Burst → Waiting

🧠 **CPU-bound → Long Computation**

🧠 **I/O-bound → Long Waiting**

🧠 **SJF → Shortest CPU Burst First**
