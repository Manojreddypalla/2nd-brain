# 🎯 GATE Corner – Functions of the Dispatcher

## The Dispatcher Performs

### 1. Context Switching ✅

- Saves the context of the currently running process.
- Restores the context of the selected process.

**Includes:**
- Program Counter (PC)
- CPU Registers
- Stack Pointer (SP)
- Process State

---

### 2. Mode Switching ✅

Switches the CPU from:

```text
Kernel Mode
      │
      ▼
User Mode
```

so the selected user process can execute.

---

### 3. Transfer of CPU Control ✅

- Loads the Program Counter.
- Transfers CPU execution to the selected process.
- The selected process starts or resumes execution.

---

### 4. Dispatch Latency Measurement ✅

The dispatcher contributes to **Dispatcher Latency**, which is the time required to:

- Stop the current process.
- Perform context switching.
- Switch modes.
- Start the next process.

---

# Operations NOT Performed by the Dispatcher ❌

The Dispatcher **does NOT**:

- Select the next process.
- Decide the scheduling algorithm.
- Maintain the Ready Queue.
- Admit new processes into memory.
- Perform long-term scheduling.
- Perform medium-term scheduling.

These tasks belong to the **Schedulers**.

---

# Scheduler vs Dispatcher

| Operation | CPU Scheduler | Dispatcher |
|------------|---------------|------------|
| Select next process | ✅ | ❌ |
| Context Switching | ❌ | ✅ |
| Switch Kernel → User Mode | ❌ | ✅ |
| Transfer CPU Control | ❌ | ✅ |
| Start/Resume Process | ❌ | ✅ |

---

# Most Expected GATE MCQs

### Q1. Which of the following are performed by the Dispatcher?

✅ Context Switching

✅ Mode Switching

✅ Transfer of CPU Control

---

### Q2. Which component selects the next process?

✅ CPU Scheduler

---

### Q3. Which component performs Context Switching?

✅ Dispatcher

---

### Q4. Dispatcher Latency includes?

✅ Context Switching

✅ Mode Switching

✅ Transfer of Control

---

# Memory Trick

🧠 **Dispatcher = CMT**

- **C** → Context Switching
- **M** → Mode Switching
- **T** → Transfer of CPU Control

Remember:

> **Scheduler Selects → Dispatcher Delivers**