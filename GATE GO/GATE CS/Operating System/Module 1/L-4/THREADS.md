Absolutely. I went through the **entire 70-page Threads lecture** and condensed it into **write-in-your-notebook short notes**. I’m keeping the lecture’s scope, but preserving the **GATE traps/nuances**—especially **shared vs separately maintained vs accessible**.

# OS — THREADS — SHORT NOTES

## 1. What is a Thread?

- **Thread = smallest unit of execution within a process.**
    
- A single process may need to perform multiple tasks simultaneously.
    
- Analogy:
    
    - **One restaurant = Process**
        
    - **Many workers = Threads**
        
    - Workers perform different tasks but work inside the **same restaurant**.
        
- Example: Chrome process → different activities such as:
    
    - Rendering
        
    - JavaScript execution
        
    - Network requests
        
    - User interaction
        

### Key idea

> **Process = resource container**  
> **Thread = execution unit**

---

# 2. Single-threaded vs Multithreaded Process

### Single-threaded process

Contains:

- Code
    
- Data
    
- Files
    
- Registers
    
- **One stack**
    
- One thread
    

### Multithreaded process

Threads share process resources:

- **Code**
    
- **Data**
    
- **Files**
    
- **Address space**
    

But each thread has its own:

- **Program Counter (PC)**
    
- **Registers**
    
- **Stack**
    
- Thread state / **TCB**
    

Why?

Because different threads may execute **different functions/instructions at the same time**.

---

# 3. What is Shared vs Private?

## Shared among threads of same process

|Resource|Shared?|
|---|---|
|Code/Text segment|✅|
|Data segment / global variables|✅|
|Heap|✅|
|Files|✅|
|Address space|✅|
|Stack|❌|
|Registers|❌|
|Program Counter|❌|

### ⚠️ GATE TRAP

**Stack is NOT shared.**

Each thread has its **own stack**.

BUT:

> A thread's stack is inside the **same process address space**, so another thread may be able to access it if it has the address.

So distinguish:

**Shared/owned separately ≠ inaccessible.**

This is why a question can say:

> "Can T1 access a variable in T2's stack?"

**Yes, potentially**, because threads of the same process share an address space.

### Remember

> **Each thread owns its stack, but threads share the address space.**

This distinction is **VERY important for GATE**.

---

# 4. Thread's Own State

Every thread maintains its own:

- **PC (Program Counter)**
    
- **Registers**
    
- **Stack**
    
- Thread state / **TCB**
    

Reason:

Each thread can be executing a different function at a different point.

### Mental picture

```text
PROCESS
│
├── Code       ← Shared
├── Data       ← Shared
├── Heap       ← Shared
├── Files      ← Shared
│
├── Stack T1   ← Private
├── Stack T2   ← Private
└── Stack T3   ← Private

T1 → own PC + registers
T2 → own PC + registers
T3 → own PC + registers
```

---

# 5. Process Address Space with Threads

### Old single-threaded process

```text
High Address
┌──────────────┐
│    Stack     │
├──────────────┤
│    Heap      │
├──────────────┤
│ Static Data  │
├──────────────┤
│    Code      │
└──────────────┘
Low Address
```

- Stack → dynamic memory
    
- Heap → dynamic memory
    
- Static data → data segment
    
- Code → text segment
    
- PC points into code.
    
- SP points into stack.
    

### Multithreaded process

```text
High Address
┌──────────────┐
│  Stack T1    │ ← SP(T1)
├──────────────┤
│  Stack T2    │ ← SP(T2)
├──────────────┤
│  Stack T3    │ ← SP(T3)
├──────────────┤
│    Heap      │ ← SHARED
├──────────────┤
│ Static Data  │ ← SHARED
├──────────────┤
│    Code      │ ← SHARED
└──────────────┘
Low Address
```

Each thread has a separate stack, but **code/data/heap belong to the common process address space**.

---

# 6. GATE 2017 — Shared Resources

**Question:** Threads of a process share:

A. Global variables but not heap  
B. Heap but not global variables  
C. Neither  
D. **Both heap and global variables**

### Answer: ✅ D

Why?

- Global variables → Data segment → shared
    
- Heap → shared
    

### Shortcut

> **Same process → same address space → code + data + heap shared.**

---

# 7. Types of Threads

Two major types:

1. **User-Level Threads (ULT)**
    
2. **Kernel-Level Threads (KLT)**
    

---

# 8. User-Level Threads (ULT)

### Definition

Thread is managed by a **user-level thread library**.

The **OS/kernel does NOT know about individual threads**.

Kernel sees only:

```text
Process P
```

rather than:

```text
P
├── T1
├── T2
└── T3
```

The programmer/thread library manages:

- Thread creation
    
- Thread deletion
    
- Scheduling
    
- Synchronization
    

### Thread creation

ULT → thread library / function calls.

Example mentioned:

`pthread` / `libpthread`

No kernel involvement is required for basic thread operations.

---

# 9. ULT Characteristics

- Managed entirely by **user-level library**
    
- Kernel is unaware of individual threads.
    
- Kernel schedules the **process**, not individual ULTs.
    
- Each thread has:
    
    - PC
        
    - Registers
        
    - Stack
        
    - Small TCB
        
- Thread creation/switching/synchronization can happen through **procedure/library calls**.
    
- No kernel involvement required for these operations.
    
- Therefore, ULT operations are **very fast**.
    
- Lecture notes mention ULT operations can be roughly **10–100× faster** than kernel threads.
    

---

# 10. Major Advantage of ULT

### ✅ Very fast

Why?

No kernel involvement is needed for thread management/context switching.

So:

```text
ULT switch
→ User library
→ No kernel mode switch
→ Fast
```

---

# 11. Major Disadvantage of ULT

### ❌ Blocking problem

Kernel doesn't know individual threads.

Suppose:

```text
Process P
├── T1
├── T2
└── T3
```

If T1 makes a blocking I/O system call:

```text
T1 → BLOCKED
```

Kernel sees the **process** as blocked.

Therefore:

```text
T1 blocks
     ↓
Process blocks
     ↓
T2 and T3 cannot run
```

So:

> **If one ULT blocks, the entire process may block.**

### GATE keyword

**ULT = Fast but blocking one thread can block the entire process.**

---

# 12. Kernel-Level Threads (KLT)

### Definition

Threads are directly supported/managed by the **OS kernel**.

Kernel knows individual threads:

```text
P
├── T1
├── T2
└── T3
```

The kernel can:

- Create threads
    
- Destroy threads
    
- Schedule threads
    
- Block/wake threads
    
- Perform synchronization-related operations
    

---

# 13. Advantages of KLT

### 1. Independent scheduling

Kernel schedules threads independently.

Therefore:

```text
T1 → CPU1
T2 → CPU2
```

can happen simultaneously.

Thus, multiple threads of the **same process can execute on multiple CPUs/cores simultaneously**.

### 2. Blocking one thread doesn't necessarily block process

If:

```text
T1 → BLOCKED
```

then kernel can schedule:

```text
T2 → RUNNING
```

So the process can continue as long as other threads are runnable.

---

# 14. Disadvantage of KLT

### ❌ Slower than ULT

Because kernel involvement is required.

Thread operations may require:

```text
User mode
   ↓
Kernel mode
   ↓
OS scheduling/management
   ↓
User mode
```

Hence higher overhead.

---

# 15. ULT vs KLT — MUST MEMORIZE TABLE

|Feature|User-Level Thread|Kernel-Level Thread|
|---|---|---|
|Managed by|User library|OS kernel|
|Kernel knows individual threads?|❌ No|✅ Yes|
|Thread scheduling|User library|Kernel|
|Speed|**Fast**|Slower|
|Kernel involvement|No/low|Yes|
|Blocking one thread|**May block entire process**|Other threads can continue|
|Independent scheduling|❌|✅|
|Multiple CPUs simultaneously|❌ Not with pure ULT|✅|
|Context switch|Faster|Slower|
|Creation|Library/function call|System call/kernel support|

---

# 16. Performance Example

Lecture gives approximate comparison:

```text
Process creation/switching → 251 μs

Kernel thread operation → 94 μs
                         ≈ 2.5× faster than process

User thread operation → 4.5 μs
                      ≈ another 20× faster
```

The exact numbers are machine-dependent; the important exam idea is:

> **ULT < KLT < Process** in context-switch/management overhead.

---

# 17. Multithreading Models

Relationship between:

```text
User Threads ↔ Kernel Threads
```

Three major models:

1. **Many-to-One**
    
2. **One-to-One**
    
3. **Many-to-Many**
    

---

# 18. Many-to-One Model

```text
T1 ─┐
T2 ─┤
T3 ─┼──→ K1
T4 ─┘
```

### Meaning

> **Many user-level threads → One kernel thread**

### Consequences

- Only one kernel thread exists.
    
- Kernel cannot schedule individual user threads.
    
- If one user thread blocks, the whole process can block.
    
- Cannot achieve true parallel execution of these ULTs on multiple CPUs.
    

### Memory trick

**Many U → One K**

---

# 19. One-to-One Model

```text
T1 → K1
T2 → K2
T3 → K3
T4 → K4
```

### Meaning

> **Each user-level thread → one kernel thread**

Advantages:

- Kernel can schedule threads independently.
    
- Blocking one thread doesn't block other threads.
    
- Threads can execute simultaneously on multiple CPUs.
    

### Disadvantage

- More kernel threads → greater kernel overhead.
    
- Number of threads may be limited by OS resources.
    

### Memory trick

**One U → One K**

---

# 20. Many-to-Many Model

```text
T1 ─┐
T2 ─┤
T3 ─┼──→ K1
T4 ─┤
    └──→ K2
```

### Meaning

> **Many user threads → Many kernel threads**

- OS can create a sufficient number of kernel threads.
    
- User threads are multiplexed over kernel threads.
    
- Provides more flexibility than many-to-one.
    

### Memory trick

**Many U → Many K**

---

# 21. Hybrid / Combined Approach

A combined ULT/KLT approach attempts to obtain advantages of both.

- Thread creation can happen in user space.
    
- Scheduling/synchronization can be performed largely in user space.
    
- Multiple ULTs can be mapped onto some number of KLTs.
    
- Programmer/application can adjust number of KLTs according to requirements.
    

Goal:

> **Combine advantages of ULT + KLT while reducing disadvantages.**

---

# 22. Context Switching

### Fastest → Slowest

```text
ULTs of SAME PROCESS
        ↓
KLTs of SAME PROCESS
        ↓
TWO DIFFERENT PROCESSES
```

### Why?

## ULT → fastest

User-level library can switch between threads without kernel involvement.

## KLT → slower

Kernel must participate in the switch.

## Process → slowest

Process switch involves changing process-specific state/address-space information.

---

# 23. Thread Context Switch vs Process Context Switch

### Thread switch — same process

The **virtual address space remains the same**.

Need to save/restore thread-specific state such as:

- Registers
    
- Stack Pointer (SP)
    
- Program Counter (PC)
    
- Thread execution state
    

But:

> No need to switch to another process's address space.

### Process switch

Need to change:

- Processor/thread state
    
- Address-space information
    
- Page-table-related state
    

The lecture highlights that changing virtual memory spaces can affect the **TLB/cache-related state**, making process switching more expensive.

---

# 24. What is Saved During Context Switch?

### Same process → thread switch

Save/restore:

- **PC**
    
- **Registers**
    
- **Stack Pointer**
    
- Thread-specific state
    

Address space remains same.

### Different process → process switch

Save/restore above **plus address-space-related state**.

Important distinction:

> **Thread switch → same address space**  
> **Process switch → different address space**

---

# 25. Important GATE Traps 🔥

### Trap 1

**"Threads have separate stacks."**

✅ TRUE.

### Trap 2

**"Threads cannot access another thread's stack."**

❌ Not necessarily.

Threads share the same process address space, so another thread can potentially access that memory.

### Trap 3

**"Threads share the stack."**

❌ FALSE.

They have **separate stacks**.

### Trap 4

**"Threads share heap."**

✅ TRUE.

### Trap 5

**"Threads share global variables."**

✅ TRUE.

### Trap 6

**"Threads share address space."**

✅ TRUE.

### Trap 7

**"Threads share registers."**

❌ FALSE.

Each thread has its own registers.

### Trap 8

**"Threads share PC."**

❌ FALSE.

Each thread has its own PC.

---

# 26. ULT/KLT High-Yield Statements

### User-level

```text
OS doesn't know individual threads
        ↓
OS schedules process
        ↓
Fast
        ↓
Blocking one thread may block entire process
```

### Kernel-level

```text
OS knows individual threads
        ↓
OS schedules threads
        ↓
More overhead
        ↓
One blocked thread doesn't necessarily block process
        ↓
Can execute threads of same process on multiple CPUs
```

---

# 27. GATE Questions — Concepts You Must Know

### GATE 2007

Question asks about ULT vs KLT.

Important facts tested:

- KLT context switching is **not slower?** → Careful with wording/options.
    
- ULT does not require kernel support for thread management.
    
- Related KLTs can be scheduled on different processors.
    
- Blocking behavior differs.
    

---

### GATE 2014

Tests:

- ULTs are not directly scheduled by kernel.
    
- If a ULT blocks, **process may block**.
    
- ULT switching is faster.
    
- KLTs of same process share code segment.
    

---

### GATE 2011

Tests what a thread maintains separately:

- CPU register state → **per thread**
    
- Separate stack → **per thread**
    
- Virtual memory/address space → **shared by threads of same process**
    

---

### GATE 2004

Key statements:

1. Context switch is faster with kernel-supported threads → ❌
    
2. For user-level threads, a system call can block entire process → ✅
    
3. Kernel-supported threads can be scheduled independently → ✅
    
4. User-level threads are transparent to kernel → ✅
    

So:

> **II, III and IV are TRUE.**

---

# 28. FINAL ONE-PAGE REVISION 🔥

```text
THREAD
│
├── Process = resource container
├── Thread = execution unit
│
├── SHARED
│   ├── Code
│   ├── Data / Global variables
│   ├── Heap
│   ├── Files
│   └── Address space
│
└── PRIVATE / PER THREAD
    ├── PC
    ├── Registers
    ├── Stack
    └── Thread state / TCB
```

### ULT

```text
User library manages
        ↓
Kernel doesn't know individual threads
        ↓
FAST
        ↓
One blocking thread
        ↓
May block whole process
```

### KLT

```text
Kernel manages
        ↓
Kernel knows individual threads
        ↓
SLOWER than ULT
        ↓
Independent scheduling
        ↓
Can run same-process threads on multiple CPUs
        ↓
One blocked thread ≠ entire process blocked
```

### Mapping

```text
Many-to-One:
Many ULT → 1 KLT

One-to-One:
1 ULT → 1 KLT

Many-to-Many:
Many ULT → Many KLT
```

### Context Switch

```text
ULT same process  → FASTEST
KLT same process  → FASTER than process switch
Different process → SLOWEST
```

### ⭐ Golden GATE Rule

> **"Shared address space" does NOT mean "every thread has the same copy of everything."**

Instead:

> **Same address space + shared process resources + separate execution state.**

And the most important stack nuance:

> **Stacks are separately maintained per thread, but they reside within the same process address space, so accessibility and ownership are different concepts.**

That is the exact distinction behind the confusing questions you've been asking about stacks.