## Preemptive Scheduling

### Definition

In **Preemptive Scheduling**, the Operating System can **interrupt (preempt)** a running process and allocate the CPU to another process.

> **Keywords:** Interrupt • Resume Later • Better Response Time

---

### When Can Preemption Occur?

- Higher-priority process arrives.
- Time quantum expires (Round Robin).
- Timer interrupt occurs.
- Certain hardware/software interrupts.

---

### Process Flow

```text
P1 Running
     │
     ▼
Interrupted
     │
Save Context
     │
Run P2
     │
Restore Context
     │
Continue P1
```

---

### Advantages

- Better Response Time
- Fair CPU Sharing
- Suitable for Interactive Systems
- Prevents CPU Monopolization

---

### Disadvantages

- Frequent Context Switches
- Higher Overhead
- More Complex Implementation

---

### Examples

- Round Robin (RR)
- Shortest Remaining Time First (SRTF)
- Preemptive Priority
- Multilevel Feedback Queue (MLFQ)

---

# Non-Preemptive Scheduling

## Definition

In **Non-Preemptive Scheduling**, once a process gets the CPU, it continues execution until:

- It terminates, or
- It requests an I/O operation (blocks).

The Operating System **cannot forcibly remove** the CPU.

> **Keywords:** No Interruption • Simpler • Lower Overhead

---

### Process Flow

```text
P1 Running
     │
     ▼
Completes
     │
CPU Assigned
     ▼
P2 Runs
```

---

### Advantages

- Simple to Implement
- Low Scheduling Overhead
- Fewer Context Switches

---

### Disadvantages

- Poor Response Time
- Long Waiting for Short Jobs
- Convoy Effect (FCFS)

---

### Examples

- First Come First Serve (FCFS)
- Shortest Job First (SJF)
- Non-Preemptive Priority

---

# Comparison

| Feature | Preemptive | Non-Preemptive |
|----------|------------|----------------|
| CPU Can Be Taken Away | Yes | No |
| Context Switches | More | Fewer |
| Scheduling Overhead | High | Low |
| Response Time | Better | Poor |
| Interactive Systems | Suitable | Not Suitable |
| Complexity | High | Low |
| Fairness | Better | Lower |

---

# Context Switch

Whenever a running process is interrupted:

```text
Running Process
        │
Save CPU Context
        │
Load New Process Context
        │
Resume Execution
```

> Context Switching is **pure overhead** because no useful user process executes during the switch.

---

# Real-Life Analogy

### Non-Preemptive

A person finishes speaking before anyone else starts.

---

### Preemptive

A fire alarm interrupts everyone immediately.

Emergency is handled first.

Afterward, the meeting resumes.

---

# Important Facts

- Preemption requires **Context Switching**.
- Timer interrupts enable preemptive scheduling.
- Non-preemptive scheduling generally has fewer context switches.
- Preemptive scheduling improves responsiveness but increases overhead.

---

# Common Mistakes

❌ Preemptive Scheduling makes a process execute faster.

✔ It only changes **when** the process executes, not **how fast** it executes.

---

❌ Non-Preemptive means a process cannot stop.

✔ A process can still leave the CPU voluntarily by requesting an I/O operation or terminating.

---

❌ Context Switch occurs only in Preemptive Scheduling.

✔ Context switches occur in **both** types. They are **more frequent** in Preemptive Scheduling.

---

# Quick Revision

```text
Non-Preemptive

P1 -------------->
              P2 --->

-------------------------

Preemptive

P1 -----
      P2 --->
           P1 ---------->
```

## One-Liner

> **Preemptive Scheduling allows the OS to interrupt a running process, whereas Non-Preemptive Scheduling lets the process keep the CPU until it terminates or blocks for I/O.**