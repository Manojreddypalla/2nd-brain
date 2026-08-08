# OS — CPU SCHEDULING ALGORITHMS

### Lecture 5 — Theory Notes

---

## 1. Process States

A process/thread generally moves through:

```text
NEW → READY → RUNNING → TERMINATED
          ↑       │
          │       ↓
          └── BLOCKED
```

### Important transitions

- **READY → RUNNING:** scheduler selects the process.
    
- **RUNNING → BLOCKED:** process requests an unavailable resource, e.g. I/O or lock.
    
- **BLOCKED → RUNNING:** ❌ **Not directly legal.**  
    It must first become **READY**, then the scheduler can select it.
    
- **READY → BLOCKED:** ❌ Not directly possible because a process must be running to execute an operation that causes blocking.
    

---

# 2. CPU Scheduling

When multiple processes are in the **ready state**, the OS has to decide:

> **Which process should get the CPU next?**

This decision is made by the **CPU scheduler**.

```text
READY QUEUE
P1   P2   P3   P4
 │    │    │    │
 └────┴────┴────┘
          ↓
      Scheduler
          ↓
         CPU
```

The choice of scheduling algorithm affects:

- Waiting time
    
- Turnaround time
    
- Response time
    
- CPU utilization
    
- Fairness
    
- Context-switch overhead
    

---

# 3. Basic Scheduling Terminology

Think of a process as a customer entering a restaurant.

```text
Enter → Wait → Get served → Finish
```

|OS term|Meaning|
|---|---|
|**Arrival Time (AT)**|Time process enters the ready queue/system|
|**Burst Time (BT)**|CPU time actually required by the process|
|**Completion Time (CT)**|Time process finishes execution|
|**Waiting Time (WT)**|Time spent waiting in the ready queue|
|**Turnaround Time (TAT)**|Total time from arrival until completion|
|**Response Time (RT)**|Time from arrival until process gets CPU **for the first time**|

### ⭐ Core formulas

[  
\boxed{TAT = CT - AT}  
]

[  
\boxed{WT = TAT - BT}  
]

[  
\boxed{RT = First\ CPU\ Start - AT}  
]

### Mental picture

```text
Arrival                    Completion
  ↓                            ↓
  |------ Waiting ------|-- CPU --|
  |<-------- Turnaround --------->|

             ↑
        First CPU start
             ↑
       Response time
```

---

# 4. Scheduling — Main Idea

Different algorithms answer the same question differently:

> **"Who should get the CPU next?"**

The important algorithms introduced in this lecture are:

```text
CPU Scheduling
│
├── FCFS
├── SJF
│    └── Preemptive version → SRTF
│
└── Round Robin
```

The lecture also introduces the idea of **preemptive vs non-preemptive scheduling**.

---

# 5. FCFS — First Come First Serve

### Idea

> **The process that arrives first gets the CPU first.**

It follows the same principle as a normal **FIFO queue**.

```text
Arrival:
P1 → P2 → P3

Execution:
P1 → P2 → P3
```

### Type

**Non-preemptive**

Once a process gets the CPU, it keeps it until its CPU burst finishes.

### Data structure

**Queue / FIFO**

### Characteristics

- Simplest CPU scheduling algorithm.
    
- Scheduling order depends on **arrival order**.
    
- Easy to implement.
    
- Does **not** generally minimize average waiting time.
    
- Average waiting time can change substantially depending on process order, especially when burst times differ greatly.
    

### Convoy Effect ⭐

A long CPU-bound process can occupy the CPU while many short processes wait behind it.

```text
Long process
██████████████████

Short processes
  P2 P3 P4 P5
  ↑  ↑  ↑  ↑
     waiting
```

This is called the **convoy effect**.

It can result in **lower CPU and device utilization** than would be possible if shorter processes were allowed to execute first.

### Pros

- Very simple.
    
- Easy to implement.
    
- Fair according to arrival order.
    
- No starvation due to scheduling priority.
    

### Cons

- Poor average waiting time in many cases.
    
- Convoy effect.
    
- Poor for interactive systems because a short process may wait behind a long one.
    

### Practical idea

Good when **simplicity and arrival-order fairness** matter more than minimizing waiting time.

---

# 6. SJF — Shortest Job First

### Idea

> When CPU becomes available, select the process having the **smallest next CPU burst**.

```text
P1 = 6
P2 = 3
P3 = 7
P4 = 3

Shortest:
P2/P4 → ...
```

If two processes have the same burst time:

> **FCFS breaks the tie.**

### Type

**Non-preemptive SJF**

Once the selected process begins execution, it completes its current CPU burst.

### ⭐ Most important property

> **SJF gives the minimum average waiting time for a given set of processes**, assuming their required CPU burst lengths are known.

This is the major reason SJF is important in scheduling theory.

### Why does it minimize waiting?

Suppose:

```text
Long job → Short job
```

If we reverse them:

```text
Short job → Long job
```

the short job saves a large amount of waiting time while the long job's waiting time increases by a smaller relative amount.

Therefore the **total/average waiting time decreases**.

### Problem with SJF ⭐

The OS does not normally know the **future CPU burst length** exactly.

That's the major difficulty:

> **How do we know how long the next CPU burst will be?**

Possible approach:

- Predict the next burst from **previous CPU bursts**.
    

The lecture introduces prediction as the practical way to estimate the next burst.

### Pros

- **Minimum average waiting time** for the given set when burst lengths are known.
    
- Generally gives good turnaround time.
    
- Short jobs finish quickly.
    

### Cons

- Future CPU burst length is not directly known.
    
- Requires prediction/estimation.
    
- Long processes can suffer **starvation** if short jobs keep arriving.
    

### Practical relevance

Excellent theoretical algorithm, but exact SJF is difficult to implement because **future burst time must be predicted**.

---

# 7. SRTF — Shortest Remaining Time First

This is the connection you just discovered:

> **SRTF = preemptive version of SJF.**

### SJF

```text
Choose shortest burst
        ↓
Run until completion
```

### SRTF

```text
Choose shortest remaining burst
        ↓
New process arrives?
        ↓
Compare remaining times again
        ↓
Preempt if new process is shorter
```

### Example

Suppose:

```text
P1 remaining = 6
P2 arrives with BT = 3
```

SRTF sees:

```text
P1 → 6 remaining
P2 → 3 remaining
```

Therefore:

```text
P1 running
    ↓
PREEMPT
    ↓
P2 runs
```

The currently executing process is preempted **if the newly arrived process has a shorter remaining CPU time**.

### Type

**Preemptive**

### Selection

**Smallest remaining CPU burst**

### Relationship

```text
SJF
 ↓ make it preemptive
SRTF
```

### Pros

- Can improve waiting time compared with non-preemptive SJF when short jobs arrive later.
    
- Short jobs can be executed quickly.
    
- More responsive than non-preemptive SJF.
    

### Cons

- More context switches.
    
- Requires remaining burst-time information/prediction.
    
- Long processes can suffer starvation.
    
- More complicated than SJF.
    

---

# 8. Round Robin — RR

### Idea

> Every process gets a fixed amount of CPU time called the **Time Quantum (q)**.

```text
P1 → P2 → P3 → P4
↑                 ↓
└─────────────────┘
```

If a process doesn't finish within its quantum:

```text
P1 runs q
   ↓
not finished
   ↓
move P1 to BACK of ready queue
```

Then the next process gets CPU.

### Type

**Preemptive**

The timer interrupts the process when its time quantum expires.

### Data structure ⭐

**Circular Queue**

The lecture's question explicitly identifies the circular queue as the best-suited structure.

Why?

```text
P1 → P2 → P3 → P4
↑                 ↓
└─────────────────┘
```

After everyone gets a turn, the first process gets another turn.

### Time Quantum

The choice of `q` is extremely important.

#### q very large

```text
RR → behaves approximately like FCFS
```

because processes can finish their CPU bursts before being preempted.

#### q very small

```text
Many context switches
        ↓
High overhead
```

So:

> **Small q → high context-switch overhead**  
> **Large q → RR approaches FCFS**

### Pros

- Fair CPU sharing.
    
- Good response for interactive/time-sharing systems.
    
- Prevents a single process from monopolizing CPU.
    
- Naturally supports multiple processes taking turns.
    

### Cons

- Too-small quantum → excessive context-switch overhead.
    
- Too-large quantum → loses much of RR's advantage and approaches FCFS.
    
- Usually not optimal for minimum average waiting time.
    

### Practical idea

RR is particularly suitable for **time-sharing / interactive systems**, where giving processes frequent CPU turns is important.

---

# 9. Preemptive vs Non-Preemptive

This distinction is fundamental.

### Non-preemptive

Once a process gets CPU:

```text
RUNNING
   ↓
continues until its CPU burst ends
```

The OS doesn't forcibly take CPU away during that burst.

Examples from this lecture:

```text
FCFS
SJF
```

### Preemptive

OS can take CPU away from the currently running process.

```text
P1 running
    ↓
new event / quantum / better process
    ↓
P1 PREEMPTED
    ↓
P2 runs
```

Examples:

```text
RR
SRTF
```

And importantly:

> **SJF can theoretically be either preemptive or non-preemptive.**

When made preemptive, it is called **SRTF**.

---

# 10. Scheduling Algorithms — Quick Comparison

|Algorithm|Selection|Type|Main DS / idea|Major characteristic|
|---|---|---|---|---|
|**FCFS**|Earliest arrival|Non-preemptive|FIFO Queue|Simple; convoy effect|
|**SJF**|Smallest next burst|Non-preemptive|Shortest-first selection|Minimum average WT|
|**SRTF**|Smallest remaining burst|Preemptive|Dynamic shortest-first|Preemptive SJF|
|**RR**|Cyclic + time quantum|Preemptive|**Circular Queue**|Fair/time-sharing|

---

# 11. The Important Relationships

Don't memorize these as isolated algorithms.

Think of them as variations of the same question:

### FCFS

> **Who came first?**

```text
Arrival order
     ↓
   FCFS
```

### SJF

> **Who needs the least CPU time?**

```text
Shortest burst
     ↓
    SJF
```

### SRTF

> **Who needs the least CPU time from NOW?**

```text
Shortest remaining burst
          ↓
         SRTF
```

### RR

> **Everyone gets a turn.**

```text
Fixed quantum
     ↓
Round Robin
```

---

# 12. GATE Facts to Lock In 🔥

### FCFS

- Non-preemptive.
    
- FIFO/arrival order.
    
- Can cause **convoy effect**.
    
- Average waiting time is **not generally minimum**.
    

### SJF

- Non-preemptive version.
    
- Chooses **smallest next CPU burst**.
    
- **Minimum average waiting time** for a given set when burst lengths are known.
    
- Main difficulty → **future burst time is unknown**.
    
- Can suffer starvation.
    

### SRTF

- **Preemptive SJF**.
    
- Chooses **shortest remaining CPU burst**.
    
- New shorter process can preempt current process.
    
- More context-switching than non-preemptive SJF.
    

### RR

- Preemptive.
    
- Uses **time quantum**.
    
- **Circular queue**.
    
- `q ↓` → context-switch overhead `↑`.
    
- `q ↑` → behaves more like FCFS.
    
- Designed around fairness/time-sharing.
    

---

# 13. One-Page Revision

```text
CPU SCHEDULING
│
├── FCFS
│   ├─ Non-preemptive
│   ├─ Arrival order / FIFO
│   ├─ Simple
│   └─ Convoy effect
│
├── SJF
│   ├─ Non-preemptive
│   ├─ Smallest next CPU burst
│   ├─ Minimum average waiting time
│   ├─ Problem: future BT unknown
│   └─ Burst prediction required
│
├── SRTF
│   ├─ Preemptive SJF
│   ├─ Smallest remaining burst
│   ├─ New shorter process → preempt
│   └─ More context switches
│
└── ROUND ROBIN
    ├─ Preemptive
    ├─ Time quantum
    ├─ Circular queue
    ├─ Fair / time-sharing
    ├─ q small → overhead ↑
    └─ q large → approaches FCFS
```

### Timing formulas

[  
\boxed{TAT=CT-AT}  
]

[  
\boxed{WT=TAT-BT}  
]

[  
\boxed{RT=First\ CPU\ Start-AT}  
]

### The single most useful mental map

```text
                    Scheduling
                        │
          ┌─────────────┴─────────────┐
          │                           │
     Non-preemptive              Preemptive
          │                           │
      ┌───┴───┐                  ┌────┴────┐
      │       │                  │         │
     FCFS    SJF                SRTF       RR
      │       │                  │         │
   Arrival  Shortest         Shortest    Quantum
    first    burst           remaining   + circular
                              burst       queue
```

**That's Lecture 1.** I deliberately left the detailed numerical problem-solving from the next lecture out so we don't mix **theory** with **question-solving patterns**. The next part can be handled separately as your **practice/question notes**.