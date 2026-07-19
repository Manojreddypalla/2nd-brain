# Module 2 — Dispatcher

## Definition

> **Dispatcher** is the OS component that **gives control of the CPU to the process selected by the CPU Scheduler.**

- Scheduler **decides** *who runs next*.
- Dispatcher **makes that process run**.

---

# Intuition

Imagine there is only **one CPU** and multiple processes waiting.

```text
Ready Queue

P1
P2
P3
P4
```

The Scheduler says:

> "Run P3."

The Dispatcher then performs everything necessary to stop the current process and start **P3**.

---

# Scheduler vs Dispatcher

| Scheduler | Dispatcher |
|------------|------------|
| Chooses the next process | Gives CPU control to the chosen process |
| Uses scheduling algorithms | Performs the actual switch |
| Decision Maker | Executor |
| Runs before dispatcher | Runs after scheduler |

### Easy Trick

> **Scheduler = Think**
>
> **Dispatcher = Do**

---

# Position in Process Execution

```text
Running Process
      │
      ▼
Interrupt
      │
      ▼
CPU Scheduler
      │
      ▼
Dispatcher
      │
      ▼
Selected Process Executes
```

---

# Responsibilities of Dispatcher

The dispatcher performs the following tasks:

## 1. Save Current Process Context

Before removing the current process from the CPU, its state must be saved.

Saved inside its **PCB**.

Includes:

- Program Counter (PC)
- CPU Registers
- Stack Pointer (SP)
- Processor Status Register (PSW)
- Other CPU state

```text
CPU
 │
 ▼
Save Registers
Save PC
Save SP
 ↓
PCB of Current Process
```

---

## 2. Load Next Process Context

The dispatcher restores the selected process using its PCB.

```text
PCB of P2

PC = 500
R1 = 20
R2 = 15
SP = 4000

↓

CPU Registers Restored
```

Now the CPU behaves exactly as if P2 had never stopped.

---

## 3. Switch CPU Mode

If required, the dispatcher switches

```text
Kernel Mode
      ↓
User Mode
```

before transferring control to a user process.

---

## 4. Jump to Correct Instruction

The Program Counter (PC) is restored.

Example:

```text
Previous PC = 720

↓

CPU resumes execution from instruction 720
```

Execution **continues**, not restarts.

---

# Dispatcher Workflow

```text
Current Process Running
        │
        ▼
Interrupt
        │
        ▼
Scheduler selects P2
        │
        ▼
Dispatcher
   ├── Save P1 Context
   ├── Load P2 Context
   ├── Switch Mode
   └── Restore PC
        │
        ▼
P2 Starts Executing
```

---

# Dispatcher and Context Switching

> **Context Switching** is the process of saving one process and restoring another.

The **Dispatcher performs the context switch**.

```text
Dispatcher
      │
      ├── Save Context
      ├── Load Context
      ├── Switch Mode
      └── Transfer CPU
```

---

# Dispatch Latency ⭐

## Definition

> **Dispatch Latency** is the time taken by the dispatcher to stop one process and start another.

It includes:

- Saving context
- Loading context
- Mode switching
- Jumping to the new process

```text
P1 Stops
    │
Save Context
    │
Load Context
    │
Mode Switch
    │
P2 Starts
```

The total time taken is called **Dispatch Latency**.

---

# Why is Dispatch Latency Important?

During dispatch latency,

- CPU is **not executing any user process**.
- It is performing OS overhead.

Higher dispatch latency means

- Lower CPU utilization
- Lower system performance

Therefore,

> **Operating Systems try to minimize dispatch latency.**

---

# Complete Flow

```text
Running Process
        │
        ▼
Interrupt
        │
        ▼
CPU Scheduler
(Chooses Next Process)
        │
        ▼
Dispatcher
        │
        ├── Save Current Context
        ├── Load New Context
        ├── Switch Kernel → User Mode
        └── Restore Program Counter
        │
        ▼
New Process Executes
```

---

# Memory Connection

```text
PCB
 │
 ├── Process ID
 ├── Process State
 ├── Program Counter
 ├── CPU Registers
 ├── Stack Pointer
 └── Scheduling Information
```

Dispatcher continuously **reads from** and **writes to** the PCB during process switching.

---

# GATE Points ⭐

- Dispatcher gives CPU control to the process selected by the scheduler.
- Dispatcher performs context switching.
- Dispatcher restores CPU registers and Program Counter.
- Dispatcher switches from Kernel Mode to User Mode (if required).
- Dispatch Latency is the time required to stop one process and start another.
- Lower dispatch latency improves CPU utilization and system performance.

---

# Interview One-Liner

> **Scheduler decides *who* runs next. Dispatcher performs the context switch and hands the CPU to that process.**

---

# Quick Revision

```text
Scheduler
      │
      ▼
Select Process
      │
      ▼
Dispatcher
      ├── Save Current Context
      ├── Load Next Context
      ├── Switch Mode
      └── Start Execution
```

## Keywords

- CPU Scheduler
- Dispatcher
- Context Switch
- PCB
- Program Counter (PC)
- CPU Registers
- Kernel Mode
- User Mode
- Dispatch Latency
-