# Context Switching (GATE Notes + Deep Intuition)

---

# 1. Core Idea

> **Context Switching is the process of saving the execution state (context) of the currently running process and restoring the saved state of another process so that the CPU can resume its execution.**

Since **one CPU core executes only one process at a time**, the OS rapidly switches between processes to create the illusion of parallel execution.

---

# 2. What is "Context"?

A process is more than just its code. At any instant, it has a complete execution state called its **context**.

The context includes:

- Program Counter (PC)
    
- CPU Registers
    
- Stack Pointer (SP)
    
- Processor Status Word (PSW/Flags)
    
- Page Table Base Register (memory mapping)
    
- Kernel Stack Pointer
    
- Scheduling Information
    
- Process State
    
- CPU usage/accounting information
    

All this information is stored in the **Process Control Block (PCB).**

---

# 3. Why Context Switching is Needed

Without context switching:

```text
P1 starts Disk Read
        ↓
CPU waits...
CPU waits...
CPU waits...
```

CPU becomes idle while waiting for I/O.

Instead,

```text
P1 blocks
      ↓
Save Context
      ↓
Run P2
```

CPU remains busy, increasing utilization.

Other reasons:

- Time sharing
    
- Multiprogramming
    
- Multitasking
    
- Preemptive scheduling
    
- Handling interrupts
    

---

# 4. Who Performs Context Switching?

Many students confuse these components.

```text
Timer Interrupt
       ↓
Kernel
       ↓
Scheduler
       ↓
Dispatcher
       ↓
CPU resumes next process
```

### Roles

**Scheduler**

- Decides _which_ process should run next.
    

**Dispatcher**

- Performs the actual switch.
    
- Saves old context.
    
- Restores new context.
    
- Transfers CPU control.
    

---

# 5. Context Switching Steps

Suppose CPU is executing **P1**.

```text
Running P1
      ↓
Timer Interrupt
      ↓
CPU enters Kernel Mode
      ↓
Save P1 Registers → PCB(P1)
      ↓
Save PC & SP
      ↓
Update Process State
      ↓
Scheduler selects P2
      ↓
Load PCB(P2)
      ↓
Restore Registers
      ↓
Restore PC & SP
      ↓
Return to User Mode
      ↓
Continue executing P2
```

Notice that **P2 resumes exactly where it had stopped earlier**, not from the beginning.

---

# 6. What Exactly Gets Saved?

|Information|Why?|
|---|---|
|Program Counter|Next instruction to execute|
|CPU Registers|Current computation values|
|Stack Pointer|Resume function calls correctly|
|Processor Status Word|Restore CPU flags and mode|
|Kernel Stack|Resume kernel execution if needed|
|Scheduling Info|Priority, quantum, state|
|Memory Info|Correct virtual address space|

---

# 7. Where is the Context Stored?

Inside the **PCB (Process Control Block).**

```text
PCB
├── PID
├── Process State
├── Program Counter
├── CPU Registers
├── Stack Pointer
├── Scheduling Info
├── Memory Info
├── Open Files
└── Accounting Info
```

Think of the PCB as the **save file** for a process.

---

# 8. Hardware vs Software Responsibilities

### Hardware

When an interrupt occurs, hardware usually:

- Saves Program Counter
    
- Saves Status Register
    
- Switches to Kernel Mode
    
- Jumps to Interrupt Handler
    

### Operating System

The kernel then:

- Saves remaining registers
    
- Updates PCB
    
- Calls Scheduler
    
- Restores another process
    
- Executes return-from-interrupt instruction
    

---

# 9. What Triggers Context Switching?

### 1. Timer Interrupt (Most Common)

Time quantum expires.

```text
P1
 ↓
Quantum over
 ↓
Switch
```

---

### 2. Blocking I/O

```c
read()
recv()
write()
sleep()
```

Process waits.

CPU switches to another process.

---

### 3. Higher Priority Process Arrives

```text
P1 (Low Priority)

↓

P2 (High Priority)

↓

Immediate Switch
```

---

### 4. Process Terminates

CPU schedules another ready process.

---

# 10. Context Switch vs Mode Switch

Very important for GATE.

|Mode Switch|Context Switch|
|---|---|
|User → Kernel → User|P1 → P2|
|Same Process|Different Process|
|No Scheduler Required|Scheduler Required|
|Faster|Slower|

Example:

```text
printf()
```

Only mode switch.

No context switch.

---

# 11. Does Every Interrupt Cause Context Switching?

**No.**

Example:

```text
Keyboard Interrupt
       ↓
ISR executes
       ↓
Return to same process
```

No process changed.

No context switch.

---

# 12. Does Every System Call Cause Context Switching?

**No.**

Example:

```c
getpid();
```

Kernel executes quickly.

Returns to the same process.

No context switch.

---

# 13. Why is Context Switching Expensive?

During switching:

- CPU isn't executing user instructions.
    
- Kernel code is running.
    
- Registers are saved/restored.
    
- Scheduler executes.
    

This time is called **Context Switch Overhead**.

It reduces CPU efficiency.

---

# 14. Cache Effects

Suppose:

```text
CPU Cache

↓

Contains P1 Data
```

After switching:

```text
Run P2
```

Now cache must load P2's data.

Result:

- Cache misses increase.
    
- Performance decreases.
    

This is called **cache pollution**.

---

# 15. TLB Effects

Each process has a different virtual address space.

When switching processes:

- Old TLB entries may become invalid.
    
- TLB may need flushing.
    

Modern CPUs reduce this overhead using **ASIDs (Address Space Identifiers)** or **PCIDs (Process Context IDs)**, allowing TLB entries for different processes to coexist.

---

# 16. Process Switch vs Thread Switch

### Process Switch

```text
Process A
      ↓
Process B
```

Changes:

- Registers
    
- Stack
    
- Address Space
    
- Page Table
    
- TLB
    

Heavy.

---

### Thread Switch (Same Process)

```text
Thread 1
      ↓
Thread 2
```

Changes:

- Registers
    
- Program Counter
    
- Stack Pointer
    

Shared:

- Address Space
    
- Heap
    
- Open Files
    

Much faster.

---

# 17. Dispatcher Latency

**Definition:**

> Time required by the dispatcher to stop one process and start another.

Includes:

- Saving context
    
- Restoring context
    
- Switching to user mode
    

Lower dispatcher latency means better responsiveness.

---

# 18. Complete Flow

```text
        Running Process (P1)
                │
                ▼
      Interrupt / Quantum Expired
                │
                ▼
        Enter Kernel Mode
                │
                ▼
      Save Context into PCB(P1)
                │
                ▼
     Scheduler Chooses Process P2
                │
                ▼
      Dispatcher Loads PCB(P2)
                │
                ▼
 Restore PC, SP, Registers, Memory Info
                │
                ▼
        Return to User Mode
                │
                ▼
         CPU Continues Process P2
```

---

# GATE Corner ⭐

### Must Remember

- **Context = Complete execution state of a process.**
    
- **PCB stores the saved context.**
    
- **Context switching occurs only in kernel mode.**
    
- **Scheduler selects; Dispatcher performs.**
    
- **Not every interrupt or system call causes a context switch.**
    
- **Context switching is overhead because no user process makes progress during the switch.**
    
- **Process switching is more expensive than thread switching due to address-space changes.**
    
- **Cache misses and TLB effects make context switching costly on modern processors.**
    

### Frequently Asked GATE Questions

- Difference between **mode switch** and **context switch**
    
- What is stored in the **PCB**
    
- What is **dispatcher latency**
    
- Why is **context switching overhead**
    
- Which events trigger a context switch
    
- Difference between **process switch** and **thread switch**