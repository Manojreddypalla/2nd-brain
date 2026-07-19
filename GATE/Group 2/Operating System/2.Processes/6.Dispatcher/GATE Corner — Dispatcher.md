# 🎯 GATE Corner — Dispatcher

## Previous Year Focus

Dispatcher is a **small but high-weight concept** because it is closely related to:

- CPU Scheduling
- Context Switching
- PCB
- Process Management

Questions are usually **1–2 marks**.

---

# Must Remember Facts ⭐⭐⭐

### 1. Dispatcher

> **Dispatcher gives control of the CPU to the process selected by the CPU Scheduler.**

**Keyword:** *Transfers CPU Control*

---

### 2. Scheduler vs Dispatcher ⭐⭐⭐

| Scheduler | Dispatcher |
|-----------|------------|
| Selects next process | Gives CPU to selected process |
| Decision Maker | Executor |
| Uses scheduling algorithm | Performs context switch |

---

### 3. Dispatcher Responsibilities ⭐⭐⭐

Dispatcher performs:

- Save current process context
- Load next process context
- Restore CPU registers
- Restore Program Counter (PC)
- Switch Kernel Mode → User Mode
- Start execution

---

### 4. Dispatch Latency ⭐⭐⭐

> **Dispatch Latency = Time taken to stop one process and start another.**

Includes:

- Saving context
- Loading context
- Mode switching
- Jumping to new process

Lower latency ⇒ Better performance.

---

### 5. Dispatcher Uses PCB ⭐⭐

Dispatcher saves and restores

- Program Counter
- Registers
- Stack Pointer
- CPU State

from the **PCB**.

---

# Common MCQs

### Q1

Who transfers CPU control to the selected process?

A. Long-Term Scheduler

B. Medium-Term Scheduler

C. CPU Scheduler

D. Dispatcher

✅ **Answer: D**

---

### Q2

Dispatch latency is

A. Time taken to select a process

B. Time taken to create a process

C. Time taken to stop one process and start another

D. Waiting time of a process

✅ **Answer: C**

---

### Q3

Dispatcher is responsible for

A. Choosing the scheduling algorithm

B. Performing context switching

C. Creating processes

D. Maintaining ready queue

✅ **Answer: B**

---

### Q4

Which component restores CPU registers from PCB?

A. Loader

B. Scheduler

C. Dispatcher

D. Compiler

✅ **Answer: C**

---

### Q5

The scheduler selects process **P2**. Which OS component actually starts executing **P2**?

A. PCB

B. Dispatcher

C. Loader

D. Process Manager

✅ **Answer: B**

---

# Confusing Statements (GATE Tricks)

### Statement 1

Scheduler chooses the next process.

✅ True

---

### Statement 2

Dispatcher selects the next process.

❌ False

---

### Statement 3

Dispatcher performs context switching.

✅ True

---

### Statement 4

Dispatcher transfers CPU control.

✅ True

---

### Statement 5

Dispatcher restores CPU registers.

✅ True

---

### Statement 6

Dispatch latency should be minimized.

✅ True

---

# One-Line Revision

```
Scheduler → Decides

Dispatcher → Executes

Context Switch → Save + Restore

Dispatch Latency → Switching Time
```

---

# Memory Trick 🧠

```
Scheduler = Brain 🧠

Dispatcher = Hands ✋

CPU = Machine ⚙️
```

Brain decides.

Hands perform.

Machine runs.

---

# Exam Checklist ✅

- [ ] Dispatcher definition
- [ ] Scheduler vs Dispatcher
- [ ] Dispatcher responsibilities
- [ ] Context Switching
- [ ] Dispatch Latency
- [ ] PCB relationship
- [ ] Common MCQs
