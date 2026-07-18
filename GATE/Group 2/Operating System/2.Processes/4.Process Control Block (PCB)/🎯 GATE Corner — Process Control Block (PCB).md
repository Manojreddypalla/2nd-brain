# 🎯 GATE Corner — Process Control Block (PCB)

This section contains **only what is important from a GATE perspective**: repeated concepts, common traps, PYQ patterns, and facts worth memorizing.

---

# Weightage

- ⭐⭐⭐ Frequently tested directly or indirectly
    
- Appears with:
    
    - Process Life Cycle
        
    - Context Switching
        
    - Scheduling
        
    - Threads
        
    - Process Creation (`fork`)
        
    - Interrupts
        

---

# Must Remember

### 1. Definition

> **PCB is a kernel data structure that represents a process.**

---

### 2. Location

✅ Kernel Space

❌ User Space

---

### 3. One Process ↔ One PCB

Every process has exactly one PCB.

---

### 4. PCB stores Metadata

Not

- Program code
    
- Heap
    
- Stack
    

It stores

- Process State
    
- PID
    
- Program Counter
    
- CPU Registers
    
- Scheduling Info
    
- Memory Info
    
- I/O Info
    

---

### 5. Program Counter

Stores

> Address of the **next instruction** to execute.

Very common MCQ.

---

### 6. PCB is Created

```
Process Creation

↓

PCB Created

↓

Ready Queue
```

NOT after execution starts.

---

### 7. PCB is Destroyed

After

```
Process Terminates
```

---

### 8. Context Switching

Current Process

↓

Save CPU Context

↓

PCB

↓

Scheduler

↓

Load CPU Context

↓

Next Process

---

### 9. Context Saved

During Context Switching

Saved

- Program Counter
    
- Registers
    
- Stack Pointer
    
- CPU Flags
    

NOT

- Entire Heap
    
- Entire Stack
    
- Entire Program
    

---

### 10. Linux

PCB implementation

```
task_struct
```

---

# Most Important PCB Fields

|Field|Why Important|
|---|---|
|PID|Process identification|
|Process State|Scheduling|
|Program Counter|Resume execution|
|CPU Registers|Restore execution|
|Scheduling Info|CPU scheduling|
|Memory Info|Address space|
|I/O Info|Open files/devices|

---

# Common GATE Statements

✔ PCB resides in Kernel Space.

✔ PCB stores process information.

✔ PCB is required for Context Switching.

✔ Scheduler works using PCBs.

✔ Context is stored inside PCB.

✔ Every process has its own PCB.

✔ Context switching saves CPU state into PCB.

---

# GATE Traps

### Trap 1

PCB contains program code.

❌ False

---

### Trap 2

PCB is stored in user memory.

❌ False

---

### Trap 3

One PCB can represent multiple processes.

❌ False

---

### Trap 4

Context switching copies entire process memory.

❌ False

Only CPU execution state is saved/restored.

---

### Trap 5

PCB stores variables.

❌ False

Variables are stored in the process address space (stack, heap, or data segment).

---

### Trap 6

Program Counter stores current instruction.

⚠ Be careful.

Most textbooks define it as

> **Address of the next instruction to execute.**

This is the convention GATE follows.

---

# Numerical Concepts

Know these (rare but possible).

### Context Switch Time

```
Context Switch Time

=

Save Time

+

Restore Time
```

---

### PCB Memory

```
PCB Memory

=

PCB Size

×

Number of Processes
```

---

### CPU Utilization

```
Execution Time
-------------------------------
Execution + Context Switch

×100
```

(Actually belongs to CPU Scheduling.)

---

# PYQ Pattern

GATE usually asks

- Which information is stored in PCB?
    
- Why PCB is required?
    
- Which field resumes execution?
    
- Where is PCB stored?
    
- What happens during context switching?
    
- Which scheduler uses PCB?
    
- Linux equivalent of PCB (`task_struct`) (occasionally in interviews rather than GATE).
    

---

# Memory Trick

**PCB = PSMICSA**

- **P** → PID
    
- **S** → State
    
- **M** → Memory information
    
- **I** → I/O information
    
- **C** → CPU context (PC + Registers + SP)
    
- **S** → Scheduling information
    
- **A** → Accounting information
    

---

# 30-Second Revision

- PCB = **Kernel data structure representing a process**.
    
- Stored in **Kernel Space**.
    
- One process has **one PCB**.
    
- Stores **PID, State, PC, Registers, Scheduling, Memory, I/O, Accounting**.
    
- **Does not store code, heap, or stack**.
    
- Used in **context switching** and **CPU scheduling**.
    
- Linux PCB = **`task_struct`**.
    

---

## ⭐ Expected GATE Difficulty: Easy–Medium

PCB questions are usually **conceptual**, not calculation-heavy. The examiner often combines PCB with **context switching**, **process states**, or **CPU scheduling**, so understanding the connections between these topics is more valuable than memorizing isolated facts.