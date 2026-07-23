# CPU Burst

## Definition
A **CPU Burst** is the continuous period during which a process executes instructions on the CPU **without waiting for any I/O operation**.

> **Keywords:** Continuous • Computation • CPU Execution

---

# CPU-I/O Burst Cycle

Every process alternates between CPU execution and I/O waiting.

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
    ↓
Terminate
```

---

# CPU Burst

During a CPU Burst, the process performs computations.

### Examples
- Arithmetic operations
- Sorting
- Matrix multiplication
- Encryption/Decryption
- Loop execution
- Function calls

**State:** Running

---

# I/O Burst

During an I/O Burst, the process waits for an I/O device.

### Examples
- Keyboard input
- Mouse input
- Reading/Writing files
- SSD/HDD access
- Network communication
- Printer access

**State:** Waiting (Blocked)

---

# Example

Opening a Web Page

```text
CPU Burst
↓
Create HTTP Request
↓
I/O Burst
(Waiting for Server Response)
↓
CPU Burst
(Process HTML)
↓
I/O Burst
(Download Images)
↓
CPU Burst
(Render Page)
```

---

# CPU-bound Process

A process that spends most of its time performing computations.

### Characteristics
- Long CPU Bursts
- Short I/O Bursts
- High CPU Utilization

### Examples
- Video Rendering
- AI Training
- Scientific Computing
- Encryption
- Gaming Physics

---

# I/O-bound Process

A process that spends most of its time waiting for I/O.

### Characteristics
- Short CPU Bursts
- Long I/O Bursts
- Frequent Waiting

### Examples
- Web Browser
- Text Editor
- File Download
- Database Query
- Chat Applications

---

# Why CPU Burst is Important?

CPU Scheduling algorithms decide **which process gets the CPU** based primarily on CPU Burst characteristics.

Examples:

- **FCFS** → Arrival Order
- **SJF** → Shortest CPU Burst
- **SRTF** → Shortest Remaining CPU Burst

---

# CPU Burst Prediction

The Operating System **cannot know the next CPU Burst exactly**.

It predicts the next burst using exponential averaging.

Formula:

τₙ₊₁ = αtₙ + (1 − α)τₙ

Where:

- τₙ₊₁ = Predicted next CPU Burst
- tₙ = Actual current CPU Burst
- τₙ = Previous prediction
- α = Smoothing factor (0 ≤ α ≤ 1)

> **Used in:** Shortest Job First (SJF)

---

# Important Observation

Research shows:

- Most CPU Bursts are **very short**
- Few CPU Bursts are **very long**

This is why **SJF performs well** on average.

---

# GATE Points

- CPU Burst = Computation Time
- I/O Burst = Waiting Time
- A process consists of multiple CPU and I/O Bursts
- CPU-bound → Long CPU Bursts
- I/O-bound → Short CPU Bursts
- SJF schedules based on CPU Burst length
- Future CPU Bursts are predicted, not known exactly

---

# Common Mistakes

❌ CPU Burst = Entire Process Execution

✔ A process usually has **multiple CPU Bursts**.

---

❌ CPU Burst always has the same length

✔ Every CPU Burst can have a different duration.

---

❌ CPU Burst means CPU is 100% utilized

✔ It only means **that process** is currently executing on the CPU.

---

# Quick Revision

```text
Process
   ↓
CPU Burst
   ↓
I/O Burst
   ↓
CPU Burst
   ↓
I/O Burst
   ↓
CPU Burst
   ↓
Terminate
```

## One-Liner

> **CPU Burst is the continuous execution time of a process on the CPU before it blocks for an I/O operation.**