# 🎯 GATE Corner – Context Switching

These are the points GATE repeatedly tests, either directly or indirectly.

---

# 1. Definition ⭐⭐⭐

> **Context Switching is the process of saving the context of the currently running process into its PCB and restoring the context of another process from its PCB so the CPU can resume execution.**

**Keywords to remember:**

- Save
    
- Restore
    
- PCB
    
- CPU state
    
- Resume
    

---

# 2. Context Means ⭐⭐⭐

Context = Complete execution state of a process.

Includes:

- Program Counter (PC)
    
- CPU Registers
    
- Stack Pointer (SP)
    
- Processor Status Word (PSW)
    
- Memory Management Information
    
- Scheduling Information
    

---

# 3. Where is Context Stored? ⭐⭐⭐

✅ **PCB (Process Control Block)**

Never:

- RAM (too vague)
    
- Cache
    
- CPU
    

The PCB is the OS data structure that stores the saved process state.

---

# 4. Context Switch Sequence ⭐⭐⭐

```text
Running Process
      ↓
Interrupt/System Call
      ↓
Kernel Mode
      ↓
Save Context → PCB
      ↓
Scheduler Chooses Next Process
      ↓
Dispatcher Restores Context
      ↓
Return to User Mode
      ↓
Next Process Executes
```

---

# 5. Scheduler vs Dispatcher ⭐⭐⭐

|Scheduler|Dispatcher|
|---|---|
|Chooses next process|Performs the switch|
|Decision|Action|
|Policy|Mechanism|

**Remember:**

> Scheduler decides.

> Dispatcher executes.

---

# 6. Does Every Interrupt Cause Context Switching? ⭐⭐⭐

**No.**

Example:

```text
Keyboard Interrupt

↓

ISR executes

↓

Same Process Continues
```

No process changed.

No context switch.

---

# 7. Does Every System Call Cause Context Switching? ⭐⭐⭐

**No.**

Example:

```c
getpid();
```

Kernel executes quickly.

Returns to same process.

No context switch.

---

# 8. Mode Switch vs Context Switch ⭐⭐⭐⭐

|Mode Switch|Context Switch|
|---|---|
|User ↔ Kernel|P1 ↔ P2|
|Same process|Different process|
|Faster|Slower|
|No scheduler|Scheduler required|

**Very common GATE question.**

---

# 9. Context Switching Happens In ⭐⭐⭐

✅ **Kernel Mode**

User processes cannot:

- Save CPU registers
    
- Modify page tables
    
- Change scheduling queues
    

Only the kernel has the required privileges.

---

# 10. What Causes Context Switching? ⭐⭐⭐

- Time quantum expires
    
- Blocking I/O
    
- Higher-priority process arrives
    
- Process terminates
    
- Voluntary yield/sleep
    

---

# 11. Why is Context Switching Overhead? ⭐⭐⭐

During switching:

- CPU saves registers
    
- Loads another process
    
- Executes kernel code
    

No user program progresses.

Hence,

> **Context switching is pure overhead.**

---

# 12. Dispatcher Latency ⭐⭐

**Definition:**

Time required to stop one process and start another.

Includes:

- Save context
    
- Restore context
    
- Switch to user mode
    

---

# 13. Process Switch vs Thread Switch ⭐⭐⭐

|Process Switch|Thread Switch|
|---|---|
|Different address spaces|Same address space|
|Heavy|Light|
|TLB may change|No page-table change|
|More overhead|Less overhead|

---

# 14. Modern CPU Effects ⭐⭐

Context switching can cause:

- Cache misses
    
- TLB misses/flushes
    

Result:

Lower CPU performance.

---

# 15. Common GATE Statements ⭐⭐⭐⭐

✅ Context is stored in PCB.

✅ Context switching is performed by the dispatcher.

✅ Scheduler selects the next process.

✅ Context switching requires kernel mode.

✅ Not every interrupt causes a context switch.

✅ Not every system call causes a context switch.

✅ Context switching introduces overhead.

✅ Process switching is costlier than thread switching.

---

# Previous GATE Focus Areas

GATE has asked questions related to:

- PCB contents
    
- Context switching overhead
    
- Scheduler vs Dispatcher
    
- Mode switch vs Context switch
    
- Process state transitions leading to a context switch
    
- Time quantum and preemption
    
- Process vs thread switching
    

---

# ⚡ 30-Second Revision

```text
Context = Execution state of a process

↓

Stored in PCB

↓

Interrupt/System Call

↓

Kernel Mode

↓

Save Current Context

↓

Scheduler selects next process

↓

Dispatcher restores next context

↓

Return to User Mode

↓

Resume execution
```

## 🧠 One-Liners to Memorize

- **PCB stores context.**
    
- **Scheduler chooses, Dispatcher switches.**
    
- **Context switch always occurs in kernel mode.**
    
- **Every context switch includes a mode switch, but every mode switch is not a context switch.**
    
- **Context switching is overhead because useful user work stops during the switch.**
    
- **Process switches are slower than thread switches due to address-space changes.**
    

These are the highest-yield facts for GATE CSE and frequently appear in both conceptual and numerical questions.