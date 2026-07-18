Absolutely. PCB is a favorite GATE topic, but direct questions are relatively few. More often, PCB appears indirectly in **context switching**, **process management**, and **scheduling** questions.

Here are some of the most important GATE-style and previous-year questions.

---

# PYQ 1 (GATE CSE 2004)

### Which of the following information is **NOT** stored in the Process Control Block (PCB)?

A) Process State

B) Program Counter

C) CPU Registers

D) Process Instructions (Program Code)

✅ **D) Process Instructions (Program Code)**

### Why?

PCB stores **information about** the process, not the actual program.

It stores:

- PID
    
- State
    
- PC
    
- Registers
    
- Scheduling info
    
- Memory info
    

The actual instructions are stored in the **process's code segment** in memory.

---

# PYQ 2 (GATE CSE 2017)

### During a context switch, which of the following must be saved?

A) CPU Registers

B) Program Counter

C) Stack Pointer

D) All of the above

✅ **D) All of the above**

Context switching saves the entire CPU context.

---

# PYQ 3 (GATE CSE)

### The PCB contains

A) CPU Scheduling Information

B) Memory Management Information

C) Accounting Information

D) All of the above

✅ **D) All of the above**

PCB contains much more than registers.

---

# PYQ 4 (Very Popular)

### Where is the Program Counter saved during a context switch?

A) RAM

B) Cache

C) PCB

D) Hard Disk

✅ **C) PCB**

The current PC register value is copied into the PCB before switching.

---

# PYQ 5 (Concept)

### The Process Control Block is primarily used for

A) Memory Allocation

B) Process Scheduling and Context Switching

C) Compilation

D) Loading Executables

✅ **B**

PCB exists so the OS can manage processes and resume them later.

---

# PYQ 6 (Scheduler vs Dispatcher)

### Which component performs the actual context switch?

A) Scheduler

B) Dispatcher

C) Loader

D) Compiler

✅ **B) Dispatcher**

Scheduler decides.

Dispatcher performs the switch.

---

# PYQ 7 (Very Common)

### During a context switch, which process state is stored?

A) Ready

B) Running

C) Waiting

D) All Process States

When the currently running process is preempted, its PCB is updated and its state usually becomes **Ready** (or **Waiting** if it blocked for I/O).

The important point is that **the PCB stores the current state of the process**.

---

# GATE-Level Practice (Expected)

### Q1

Suppose a process is interrupted after executing instruction at address 420.

The next instruction is at address 424.

Which value is saved inside the PCB?

A) 420

B) 424

C) Both

D) Depends on scheduler

✅ **Answer:** **B**

Because the Program Counter stores the **next instruction** to execute.

---

### Q2

The CPU currently contains

```text
PC = 100
SP = 800
R1 = 50
```

After a context switch, these values are copied into

A) Process Memory

B) Stack

C) PCB

D) Cache

✅ **Answer:** **C**

---

### Q3

Which statement is correct?

A) Scheduler performs context switching.

B) Dispatcher performs context switching.

C) Program Counter chooses the next process.

D) PCB stores the process code.

✅ **Answer:** **B**

---

### Q4 (Favorite GATE Trap)

A PCB stores

1. Program Counter
    
2. CPU Registers
    
3. Open File Information
    
4. Page Table Pointer
    

Choose the correct option.

A) 1 and 2 only

B) 1, 2 and 3

C) 2 and 4

D) All of the above

✅ **Answer:** **D**

---

# Expected GATE Notes

These are the PCB facts that GATE repeatedly tests:

|Concept|Importance|
|---|---|
|PCB Definition|⭐⭐⭐⭐⭐|
|Program Counter|⭐⭐⭐⭐⭐|
|CPU Registers|⭐⭐⭐⭐⭐|
|Context Switching|⭐⭐⭐⭐⭐|
|Scheduler vs Dispatcher|⭐⭐⭐⭐⭐|
|Process States|⭐⭐⭐⭐|
|Memory Management Info|⭐⭐⭐|
|Open File Table|⭐⭐⭐|
|Accounting Info|⭐⭐|
|Signals|⭐⭐|

## 🎯 Challenge (Without Looking)

Try these three questions yourself:

1. **Why doesn't the PCB store the actual program code?**
    
2. **Why is the Program Counter saved in the PCB during a context switch instead of keeping it only in the CPU?**
    
3. **What is the difference between the Scheduler and the Dispatcher?**
    

If you can answer these correctly, you've mastered PCB at the GATE level.