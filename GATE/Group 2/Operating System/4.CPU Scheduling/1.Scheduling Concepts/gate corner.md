# 🎯 GATE Corner – CPU Scheduling Concepts (Complete Revision)

This is the complete flow connecting all major scheduling concepts:

- CPU Burst
    
- I/O Burst
    
- Preemptive vs Non-Preemptive Scheduling
    
- Scheduling Queues
    
- Ready Queue
    
- Dispatcher
    
- Dispatcher Latency
    

---

# ⭐ Complete CPU Scheduling Flow

```text
                           Long-Term Scheduler
                                  │
                                  ▼
                         Job Queue (New)
                                  │
                                  ▼
                      Ready Queue (Ready State)
                                  │
                   Short-Term Scheduler (CPU Scheduler)
                     (Selects the next process)
                                  │
                                  ▼
                            Dispatcher
             (Context Switch + Mode Switch + CPU Handover)
                                  │
                                  ▼
                        Running (CPU Burst)
                      ┌────────────┴────────────┐
                      │                         │
              I/O Request                 Process Finishes
                      │                         │
                      ▼                         ▼
          Device Queue (Waiting)         Terminated
                      │
              I/O Completed
                      │
                      ▼
                Ready Queue
                      │
                      ▼
                Runs Again
```

---

# CPU Burst vs I/O Burst

|CPU Burst|I/O Burst|
|---|---|
|Process executes instructions|Process waits for I/O|
|Running State|Waiting (Blocked) State|
|Uses CPU|Does not use CPU|
|CPU Busy|CPU available for another process|

---

# CPU-I/O Burst Cycle

A process rarely executes from start to finish without interruption.

Instead, it alternates between computation and I/O.

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

# Scheduling Queues

## Job Queue

- Contains all submitted processes.
    
- Some may not yet be loaded into memory.
    
- Managed by the **Long-Term Scheduler**.
    

---

## Ready Queue ⭐

Contains processes that are:

- In main memory
    
- Ready to execute
    
- Waiting only for the CPU
    

> **Only processes in the Ready Queue can be scheduled.**

---

## Device Queue

Contains processes waiting for a particular I/O device.

These processes are in the **Waiting (Blocked)** state.

---

# Process States and Queues

|State|Queue|
|---|---|
|New|Job Queue|
|Ready|Ready Queue|
|Running|CPU|
|Waiting|Device Queue|
|Terminated|Exit|

---

# Scheduler Responsibilities

## Long-Term Scheduler

Moves processes from:

```text
Job Queue
     ↓
Ready Queue
```

Controls the degree of multiprogramming.

---

## Short-Term Scheduler

Chooses the next process from:

```text
Ready Queue
        ↓
CPU
```

Runs very frequently.

---

# Dispatcher

The dispatcher **implements the scheduler's decision** by giving CPU control to the selected process.

Responsibilities:

- Context Switch
    
- Mode Switch (Kernel → User)
    
- Transfer CPU Control
    

```text
Ready Queue
      │
      ▼
CPU Scheduler
      │
      ▼
Dispatcher
      │
      ▼
Running Process
```

---

# Dispatcher Latency

Dispatcher Latency is the **time required to stop one process and start another.**

It includes:

- Context Switch
    
- Mode Switch
    
- Control Transfer
    

```text
Dispatcher Latency =
Context Switch
+ Mode Switch
+ Control Transfer
```

Lower dispatcher latency improves system performance.

---

# Preemptive vs Non-Preemptive Scheduling

## Preemptive Scheduling

The operating system **can interrupt** a running process before it finishes its CPU burst.

Common reasons:

- Time quantum expires
    
- Higher-priority process arrives
    

Examples:

- Round Robin
    
- SRTF
    
- Preemptive Priority
    
- MLFQ
    

---

## Non-Preemptive Scheduling

Once a process gets the CPU, it keeps it until:

- CPU Burst finishes
    
- Requests I/O
    
- Terminates
    

Examples:

- FCFS
    
- Non-Preemptive SJF
    
- Non-Preemptive Priority
    

---

# CPU-bound vs I/O-bound

|CPU-bound|I/O-bound|
|---|---|
|Long CPU Bursts|Short CPU Bursts|
|Short I/O Bursts|Long I/O Bursts|
|More Computation|More Waiting|
|CPU Intensive|I/O Intensive|

---

# CPU Burst Prediction

Future CPU Burst is unknown.

The OS estimates it using **Exponential Averaging**:

$$  
\tau_{n+1}=\alpha t_n+(1-\alpha)\tau_n  
$$

Where:

- $\tau_{n+1}$ → Next predicted CPU Burst
    
- $t_n$ → Actual current CPU Burst
    
- $\tau_n$ → Previous prediction
    
- $\alpha$ → Smoothing constant $(0 \le \alpha \le 1)$
    

Used by:

- SJF
    
- SRTF
    

---

# Scheduling Algorithm Comparison

|Algorithm|Preemptive|Uses CPU Burst?|
|---|---|---|
|FCFS|❌|❌|
|SJF|❌|✅|
|SRTF|✅|✅ (Remaining Burst)|
|Priority|Both|❌|
|Round Robin|✅|❌ (Uses Time Quantum)|
|MLFQ|✅|Indirectly|

---

# Complete Process Movement

```text
New
 │
 ▼
Job Queue
 │
 ▼
Ready Queue
 │
 ▼
CPU Scheduler
 │
 ▼
Dispatcher
 │
 ▼
Running (CPU Burst)
 │
 ├───────────────┐
 │               │
 ▼               ▼
Requests I/O   Terminates
 │
 ▼
Waiting (Device Queue)
 │
 ▼
I/O Completed
 │
 ▼
Ready Queue
 │
 ▼
Runs Again
```

---

# High-Probability GATE Questions

### Q1. Which queue is used by the CPU Scheduler?

✅ Ready Queue

---

### Q2. During an I/O Burst, which state is the process in?

✅ Waiting (Blocked)

---

### Q3. Can a blocked process be scheduled?

❌ No

---

### Q4. Who selects the next process?

✅ Short-Term Scheduler

---

### Q5. Who gives CPU control to the selected process?

✅ Dispatcher

---

### Q6. What is Dispatcher Latency?

✅ Time to stop one process and start another.

---

### Q7. Which scheduler admits processes into memory?

✅ Long-Term Scheduler

---

### Q8. Which scheduling algorithms require CPU Burst prediction?

✅ SJF and SRTF

---

### Q9. During an I/O Burst, what does the CPU do?

✅ Executes another Ready process.

---

### Q10. Which queue contains Waiting processes?

✅ Device Queue

---

### Q11. Which queue contains Ready processes?

✅ Ready Queue

---

### Q12. Can a Running process be inside the Ready Queue?

❌ No

---

# Common GATE Traps

❌ CPU Burst = Total execution time.

✔ CPU Burst is only one computation phase.

---

❌ Ready State = Waiting State.

✔ Ready → Waiting for CPU.

✔ Waiting → Waiting for an I/O event.

---

❌ Scheduler starts the process.

✔ Scheduler **selects**; Dispatcher **starts**.

---

❌ CPU waits during an I/O Burst.

✔ The **process waits**; the CPU executes another Ready process.

---

❌ Waiting processes compete for the CPU.

✔ Only Ready processes compete for CPU scheduling.

---

❌ Dispatcher chooses the next process.

✔ The **Short-Term Scheduler** chooses; the **Dispatcher** performs the switch.

---

# Memory Tricks

🧠 **CPU Burst = Compute**

🧠 **I/O Burst = Wait**

🧠 **Ready = Waiting for CPU**

🧠 **Waiting = Waiting for I/O**

🧠 **Job Queue = Waiting to Enter Memory**

🧠 **Ready Queue = Only Schedulable Queue**

🧠 **Scheduler = Decide**

🧠 **Dispatcher = Execute**

🧠 **Preemptive = CPU Can Be Taken Away**

🧠 **Non-Preemptive = CPU Kept Until Burst Ends**

---

# 🎯 One-Page Mental Model

```text
                Long-Term Scheduler
                        │
                        ▼
                  Job Queue (New)
                        │
                        ▼
                Ready Queue (Ready)
                        │
             Short-Term Scheduler
                  (Decision)
                        │
                        ▼
                  Dispatcher
          (Context Switch + CPU Handover)
                        │
                        ▼
                Running (CPU Burst)
                  │             │
          I/O Request       Finish
                  │             │
                  ▼             ▼
        Device Queue      Terminated
        (Waiting State)
                  │
            I/O Complete
                  │
                  ▼
            Ready Queue
                  │
                  └────────────► Repeat
```

## 🎯 Final GATE Mantra

**A process spends its lifetime alternating between CPU Bursts and I/O Bursts. It moves among the Job Queue, Ready Queue, and Device Queue. The Short-Term Scheduler decides which ready process should run, and the Dispatcher performs the context switch to hand over the CPU. In preemptive scheduling, the OS may interrupt a running process; in non-preemptive scheduling, the process keeps the CPU until it blocks or finishes its current CPU burst.**