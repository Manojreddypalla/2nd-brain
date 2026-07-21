# Dispatcher

## Definition

A **Dispatcher** is an Operating System component responsible for **giving control of the CPU to the process selected by the Short-Term Scheduler (CPU Scheduler).**

> **Keywords:** CPU Allocation • Context Switch • User Mode • Execution

---

# Purpose

The CPU Scheduler **selects** the next process.

The **Dispatcher executes that decision** by actually assigning the CPU to the selected process.

> **Scheduler decides, Dispatcher executes.**

---

# Responsibilities of the Dispatcher

The Dispatcher performs three major tasks:

### 1. Context Switching

- Saves the state of the currently running process.
- Loads the state of the next selected process.

Examples of saved information:

- Program Counter (PC)
- CPU Registers
- Stack Pointer (SP)
- Process State

---

### 2. Switch to User Mode

After kernel operations are completed, the dispatcher switches the CPU from:

```text
Kernel Mode
      │
      ▼
User Mode
```

This allows the selected process to execute safely.

---

### 3. Transfer Control to the Process

The dispatcher jumps to the instruction where the selected process last stopped.

Execution resumes from the saved **Program Counter (PC)**.

---

# Dispatcher Workflow

```text
Ready Queue
      │
      ▼
CPU Scheduler
(Select Process)
      │
      ▼
Dispatcher
      │
 ┌────┼─────────────┐
 │    │             │
 ▼    ▼             ▼
Context Switch
Switch to User Mode
Transfer CPU Control
      │
      ▼
Running Process
```

---

# Scheduler vs Dispatcher

| CPU Scheduler | Dispatcher |
|---------------|------------|
| Selects the next process | Gives CPU to the selected process |
| Makes scheduling decision | Implements the decision |
| Works on scheduling policy | Performs CPU transfer |
| Chooses process | Starts process execution |

---

# Dispatcher Latency

## Definition

**Dispatcher Latency** is the time required by the dispatcher to stop one process and start another.

It includes:

- Context Switching
- Mode Switching
- Jumping to the next process

---

# Importance of Dispatcher Latency

Lower dispatcher latency means:

- Better CPU Utilization
- Better Response Time
- Faster Context Switching
- Improved System Performance

---

# Factors Affecting Dispatcher Latency

- Context switch time
- Hardware support
- Number of CPU registers
- Cache/TLB effects
- Operating System implementation

---

# Process Flow

```text
Ready Queue
      │
CPU Scheduler
      │
      ▼
Dispatcher
      │
Context Switch
      │
Kernel → User Mode
      │
Transfer Control
      ▼
Running Process
```

---

# Important Facts

- Dispatcher works **after** the CPU Scheduler.
- Every CPU allocation passes through the Dispatcher.
- Dispatcher is responsible for starting/resuming execution.
- Dispatcher performs context switching.
- Dispatcher latency should be as small as possible.

---

# Common Mistakes

❌ Scheduler and Dispatcher are the same.

✔ Scheduler selects the process.

✔ Dispatcher gives the CPU to that process.

---

❌ Dispatcher decides which process runs.

✔ The Scheduler decides; the Dispatcher only executes that decision.

---

❌ Dispatcher works only during process creation.

✔ It works whenever the CPU is switched from one process to another.

---

# Quick Revision

```text
Ready Queue
      │
      ▼
CPU Scheduler
      │
      ▼
Dispatcher
      │
Context Switch
      │
Kernel → User
      │
Transfer Control
      ▼
Running
```

## One-Liner

> **The Dispatcher transfers CPU control to the process selected by the CPU Scheduler by performing context switching, switching to user mode, and starting/resuming process execution.**