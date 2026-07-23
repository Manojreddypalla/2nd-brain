# I/O Burst

## Definition

An **I/O Burst** is the continuous period during which a process **waits for an Input/Output operation to complete** instead of executing on the CPU.

> **Keywords:** Waiting • I/O Device • No CPU Execution

---

# CPU-I/O Burst Cycle

Every process alternates between computation and waiting.

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

# What Happens During an I/O Burst?

During an I/O Burst, the process:

- Does **not** execute instructions.
- Waits for an external device.
- Enters the **Waiting (Blocked)** state.
- Releases the CPU for another process.

---

# Common I/O Operations

### User Input
- Keyboard
- Mouse
- Touchscreen

### Storage
- SSD Read/Write
- HDD Read/Write

### Network
- HTTP Request
- Download
- Upload
- Socket Communication

### Other Devices
- Printer
- Scanner
- USB Devices

---

# Process State Transition

```text
Running
    │
    │ Requests I/O
    ▼
Waiting (Blocked)
    │
    │ I/O Completed
    ▼
Ready
    │
    ▼
Running
```

> **Note:** A process in the **Waiting** state cannot be scheduled.

---

# Example

Opening a Web Page

```text
CPU Burst
(Create HTTP Request)
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
(Render Webpage)
```

---

# CPU During an I/O Burst

When one process enters an I/O Burst:

- The process releases the CPU.
- The CPU Scheduler selects another process from the Ready Queue.
- CPU Utilization increases.

---

# CPU Burst vs I/O Burst

| CPU Burst | I/O Burst |
|------------|-----------|
| Computation | Waiting |
| Running State | Waiting (Blocked) State |
| CPU Executes Instructions | CPU Does Not Execute This Process |
| Uses CPU | Waits for I/O Device |

---

# Why is I/O Burst Important?

Without CPU Scheduling:

```text
Process waits
        ↓
CPU remains idle
```

With CPU Scheduling:

```text
Process waits
        ↓
CPU executes another Ready Process
```

This improves:

- CPU Utilization
- Throughput
- Overall System Performance

---

# Relationship with CPU-bound & I/O-bound Processes

### CPU-bound Process

- Long CPU Bursts
- Short I/O Bursts

Examples:
- Video Rendering
- AI Training
- Scientific Computing

---

### I/O-bound Process

- Short CPU Bursts
- Long I/O Bursts

Examples:
- Browser
- Database Query
- File Download
- Text Editor

---

# Important Facts

- A process may have **multiple I/O Bursts** during its lifetime.
- I/O Burst duration depends on device speed.
- CPU is free while the process waits.
- The process returns to the **Ready Queue** after I/O completion.

---

# GATE Points

- I/O Burst = Waiting for an I/O operation.
- During an I/O Burst, the process is in the **Waiting (Blocked)** state.
- Waiting processes cannot be scheduled.
- CPU executes another Ready process during an I/O Burst.
- Every process alternates between CPU Bursts and I/O Bursts.

---

# Common Mistakes

❌ During an I/O Burst, the CPU is idle.

✔ The **process** is waiting; the CPU usually executes another process.

---

❌ Waiting State = Ready State.

✔ **Ready** → Waiting for CPU.

✔ **Waiting (Blocked)** → Waiting for an I/O event.

---

❌ I/O Burst means the process has finished.

✔ After I/O completion:

```text
Waiting
    ↓
Ready
    ↓
Running
```

---

# Quick Revision

```text
CPU Burst
(Computation)
      ↓
Requests I/O
      ↓
I/O Burst
(Waiting)
      ↓
I/O Completed
      ↓
Ready Queue
      ↓
CPU Burst
```

## One-Liner

> **An I/O Burst is the continuous waiting period during which a process waits for an I/O operation to complete and does not execute on the CPU.**