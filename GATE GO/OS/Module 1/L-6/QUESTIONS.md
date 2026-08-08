# OS — CPU SCHEDULING

## Lecture 2 — Questions + Answers

---

# 1. GATE CSE 1996 — Round Robin

### Question

Four jobs execute on a single CPU in the order **A, B, C, D**.

|Process|CPU Burst|
|---|--:|
|A|4|
|B|1|
|C|8|
|D|1|

All arrive at time 0. Round Robin time quantum:

[  
q=1  
]

Find the **completion time of A**.

### Solution

RR execution:

```text
A B C D A C A C A C ...
```

Track A:

```text
A: 0-1
A: 4-5
A: 6-7
A: 8-9  ← finishes
```

Therefore:

[  
\boxed{CT_A=9}  
]

### Answer: **9**

**Pattern:** RR → don't finish one process completely; rotate through the ready queue according to `q`.

---

# 2. GATE CSE 1993 — Round Robin

### Question

All jobs arrive at time 0 in order `p, q, r, s, t`.

|Job|CPU Burst|
|---|--:|
|p|4|
|q|1|
|r|8|
|s|1|
|t|2|

Round Robin time slice:

[  
q=1  
]

Find the **completion/departure time of p**.

### RR sequence

```text
p q r s t p r p r p r ...
```

Track `p`:

```text
p → 1
p → 5
p → 7
p → 9  ← finishes
```

Therefore:

[  
\boxed{CT_p=9}  
]

Wait — the lecture's page shows the answer as **11**, because the actual rotation is:

```text
p q r s t p r p r p r...
```

Let's count the timeline properly:

```text
0-1   p
1-2   q
2-3   r
3-4   s
4-5   t
5-6   p
6-7   r
7-8   p
8-9   r
9-10  p
10-11 r
```

So:

[  
\boxed{CT_p=11}  
]

### Answer: **11** ✅

**Important:** Always build the timeline. Don't count "turns" mentally.

---

# 3. GATE CSE 1998 — Round Robin + Context Switch

### Question

There are `n` processes in Round Robin.

- Context-switch time = `s`
    
- Time quantum = `q`
    
- Each process must get CPU at least every `t` seconds.
    

Find the condition on `q` that minimizes switching overhead while satisfying the requirement.

### Think about P₁

Between two turns of P₁:

```text
P₁ → P₂ → P₃ → ... → Pₙ → P₁
```

There are:

- `n−1` **other processes** that execute
    
- `n` context switches
    

Therefore:

[  
(n-1)q+ns\le t  
]

So:

[  
(n-1)q\le t-ns  
]

[  
\boxed{q\le\frac{t-ns}{n-1}}  
]

### Answer: **A**

### Pattern

> **Between two turns of one process:** `(n−1)q + ns`

This is a very useful RR formula.

---

# 4. GATE CSE 2012 — FCFS vs RR

### Question

|Process|Arrival|CPU Time|
|---|--:|--:|
|P1|0|5|
|P2|1|7|
|P3|3|4|

Compare:

- FCFS
    
- RR with `q = 2`
    

Find the completion order.

---

### FCFS

Since:

```text
P1 arrives → P2 → P3
```

FCFS:

```text
P1 → P2 → P3
```

### RR

Build the Gantt chart:

```text
0    2    4    6    8    10   12   15
| P1 | P2 | P1 | P3 | P2 | P3 | P2 |
```

Completion:

```text
P1 → P3 → P2
```

Therefore:

[  
\boxed{\text{FCFS: }P1,P2,P3}  
]

[  
\boxed{\text{RR: }P1,P3,P2}  
]

### Answer: **D**

### Pattern

With RR, **arrival time matters**. A process that arrives later doesn't automatically get placed at the front.

---

# 5. GATE CSE 2020 — SJF vs RR

### Question

All processes arrive at time 0.

|Process|Burst|
|---|--:|
|P1|8|
|P2|7|
|P3|2|
|P4|4|

RR quantum:

[  
q=4  
]

Find the absolute difference between average turnaround times of **SJF and RR**.

---

## SJF

Shortest first:

```text
P3 → P4 → P2 → P1
```

Completion times:

```text
P3 = 2
P4 = 6
P2 = 13
P1 = 21
```

All arrive at 0, so:

[  
TAT=CT  
]

Average:

# [  
\frac{2+6+13+21}{4}

# \frac{42}{4}

10.5  
]

---

## RR, q = 4

```text
P1 → P2 → P3 → P4 → P1 → P2
```

Gantt chart:

```text
0   4   8   10  14  18  21
|P1 |P2 |P3 |P4 |P1 |P2 |
```

Completion times:

```text
P1 = 18
P2 = 21
P3 = 10
P4 = 14
```

Average:

# [  
\frac{18+21+10+14}{4}

15.75  
]

Difference:

# [  
|10.5-15.75|

\boxed{5.25}  
]

### Answer: **5.25 ms**

### Pattern

For all processes arriving at `0`:

[  
\boxed{TAT=CT}  
]

That makes these problems much faster.

---

# 6. GATE CSE 2021 — Identify the Algorithm

### Question

Three processes arrive at time 0 with CPU bursts:

```text
16, 20, 10 ms
```

Scheduler knows the burst lengths beforehand.

It is **non-preemptive** and asks for the **minimum achievable average waiting time**.

### Recognition

Keywords:

```text
Known burst time
+
Minimum average waiting time
+
Non-preemptive
        ↓
       SJF
```

Order:

```text
10 → 16 → 20
P3   P1   P2
```

Waiting:

```text
P3 = 0
P1 = 10
P2 = 10 + 16 = 26
```

Average:

# [  
\frac{0+10+26}{3}

\boxed{12}  
]

### Answer: **12 ms**

### ⭐ Recognition fact

> **Minimum average waiting time + known burst time → SJF**

---

# 7. GATE CSE 2026 — FCFS

### Question

Two types of processes:

|Type|Arrival Times|Burst|
|---|---|--:|
|A|10,20,30,40,50|6|
|C|11,22,33,44,55|8|

FCFS is used, and the first A starts at `t=10`.

Find the average waiting time of all 10 processes.

### Execution order

Because of FCFS:

```text
A1 C1 A2 C2 A3 C3 A4 C4 A5 C5
```

Gantt chart:

```text
10  16  24  30  38  44  52  58  66  72  80
|A1| C1|A2 |C2 |A3 |C3 |A4 |C4 |A5 |C5 |
```

Waiting times:

### A processes

```text
A1 = 10 - 10 = 0
A2 = 24 - 20 = 4
A3 = 38 - 30 = 8
A4 = 52 - 40 = 12
A5 = 66 - 50 = 16
```

### C processes

```text
C1 = 16 - 11 = 5
C2 = 30 - 22 = 8
C3 = 44 - 33 = 11
C4 = 58 - 44 = 14
C5 = 72 - 55 = 17
```

Total:

[  
0+4+8+12+16+5+8+11+14+17=95  
]

Average:

# [  
\frac{95}{10}

\boxed{9.5}  
]

### Answer: **9.5 seconds**

### Pattern

For FCFS:

> **First sort/process according to arrival order, then make the Gantt chart.**

---

# 8. SRTF Practice Question

The lecture gives:

|Process|AT|BT|
|---|--:|--:|
|P1|0|8|
|P2|1|4|
|P3|2|9|
|P4|3|5|

Use **SRTF**.

### Gantt chart

```text
0   1   5   10       17       26
|P1 |P2 | P4 |   P1   |   P3   |
```

Why?

At `t=1`:

```text
P1 remaining = 7
P2 = 4
```

So P2 preempts P1.

At `t=2`:

```text
P2 remaining = 3
P3 = 9
```

P2 continues.

At `t=3`:

```text
P2 remaining = 2
P4 = 5
```

P2 continues.

Then P4, P1, P3.

### Waiting times

From the lecture:

```text
P1 = 9
P2 = 0
P3 = 15
P4 = 2
```

Average:

# [  
\frac{9+0+15+2}{4}

\boxed{6.5}  
]

### ⭐ Pattern

> Every time a process arrives, compare its burst with the **remaining time** of the running process.

---

# 9. LJF — Longest Job First Practice

The lecture introduces **LJF** as the opposite of SJF:

> Select the process with the **largest CPU burst**.

Characteristics given:

- **Non-preemptive**
    
- First criterion → Burst Time
    
- Data structure → **Max Heap**
    

### Question

|Process|AT|BT|
|---|--:|--:|
|P1|0|2|
|P2|1|1|
|P3|2|6|
|P4|3|3|

Calculate:

- Completion Time
    
- Turnaround Time
    
- Waiting Time
    

using LJF.

### Gantt chart

```text
0    2        8       11   12
| P1 |   P3   |  P4   | P2 |
```

Why?

At `t=0` only P1 exists → P1.

At `t=2`:

```text
P2 = 1
P3 = 6
```

Choose P3.

At `t=8`:

```text
P2 = 1
P4 = 3
```

Choose P4.

Finally P2.

### Results

|Process|CT|TAT = CT−AT|WT = TAT−BT|
|---|--:|--:|--:|
|P1|2|2|0|
|P2|12|11|10|
|P3|8|6|0|
|P4|11|8|5|

Totals:

[  
\boxed{\text{Total TAT}=27}  
]

[  
\boxed{\text{Total WT}=15}  
]

---

# 10. LRTF — Longest Remaining Time First

The lecture introduces:

> **LRTF = preemptive version of LJF.**

So:

```text
LJF
 ↓ make preemptive
LRTF
```

### LJF

```text
Longest burst
```

### LRTF

```text
Longest remaining burst
```

The lecture's example uses:

|Process|AT|BT|
|---|--:|--:|
|P1|1|2|
|P2|2|4|
|P3|3|6|
|P4|4|8|

When there is a tie, the lecture breaks it based on **arrival time**.

The resulting schedule is repeatedly recalculated as processes arrive and their remaining burst times change.

---

# 11. GATE CSE 2004 — SRTF

### Question

|Process|AT|BT|
|---|--:|--:|
|P1|0|5|
|P2|1|3|
|P3|2|3|
|P4|4|1|

Find average turnaround time using **preemptive SRTF**.

### Gantt chart

```text
0   1       4   5       8       12
|P1 |  P2   |P4 |  P3   |   P1   |
```

Completion:

```text
P1 = 12
P2 = 4
P3 = 8
P4 = 5
```

Turnaround:

[  
P1=12-0=12  
]

[  
P2=4-1=3  
]

[  
P3=8-2=6  
]

[  
P4=5-4=1  
]

Average:

# [  
\frac{12+3+6+1}{4}

\boxed{5.5}  
]

### Answer: **5.5 ms — A**

---

# 12. GATE CSE 2006 — SRTF

### Question

Three CPU-intensive processes:

- CPU bursts = **10, 20, 30**
    
- Arrivals = **2, 4, 6**
    
- SRTF scheduling
    
- Don't count context switches at time 0 and at the end.
    

How many context switches are needed?

### Think carefully

At `t=2`:

```text
P1 = 10
```

At `t=4`, P2 arrives:

```text
P1 remaining = 8
P2 = 20
```

P1 stays.

At `t=6`, P3 arrives:

```text
P1 remaining = 6
P2 = 20
P3 = 30
```

P1 stays.

So there is **no preemption caused by arrivals**.

After P1 finishes:

```text
P1 → P2 → P3
```

There are two process-to-process switches:

```text
P1 → P2
P2 → P3
```

### Answer

[  
\boxed{2}  
]

**Answer: B**.

### ⭐ Pattern

> **A process arriving does NOT automatically cause a context switch in SRTF.**

It only does so if the new process has a **shorter remaining time**.

---

# 13. GATE CSE 2006 — LRTF

### Question

Three processes:

|Process|Burst|
|---|--:|
|P0|2|
|P1|4|
|P2|8|

All arrive at time 0.

Use **LRTF**. If there is a tie, give priority to the process with the **lowest process ID**.

Find average turnaround time.

### Result

The LRTF execution produces completion times:

```text
P0 → 12
P1 → 13
P2 → 14
```

All arrive at 0:

[  
TAT=CT  
]

Therefore:

# [  
Average\ TAT

\frac{12+13+14}{3}  
]

[  
=\boxed{13}  
]

### Answer: **13 units — A**

---

# 14. GATE CSE 2024 — SRTF vs Non-Preemptive SJF

### Question

Processes are represented as:

```text
A(0,10)
B(2,6)
C(4,3)
D(6,7)
```

where:

```text
(first value)  = Arrival Time
(second value) = CPU Burst
```

Find average waiting times for:

1. **Preemptive SRTF**
    
2. **Non-preemptive SJF**
    

---

## SRTF

Schedule:

```text
0-2   A
2-4   B
4-7   C
7-11  B
11-18 D
18-26 A
```

Completion:

```text
A = 26
B = 11
C = 7
D = 18
```

Waiting:

[  
WT=CT-AT-BT  
]

```text
A = 26-0-10 = 16
B = 11-2-6 = 3
C = 7-4-3 = 0
D = 18-6-7 = 5
```

Average:

# [  
\frac{16+3+0+5}{4}

\boxed{6}  
]

---

## Non-preemptive SJF

At `t=0`, only A exists → A must run completely.

```text
0-10 A
10-13 C
13-19 B
19-26 D
```

Waiting:

```text
A = 0
C = 10-4 = 6
B = 13-2 = 11
D = 19-6 = 13
```

Average:

# [  
\frac{0+6+11+13}{4}

\boxed{7.5}  
]

### Answer

[  
\boxed{\text{SRTF}=6,\quad \text{SJF}=7.5}  
]

### **Answer: B**

---

# 🔥 FINAL QUESTION-SOLVING CHEAT SHEET

## When you see...

|Question wording|Immediately think|
|---|---|
|**Arrival order**|FCFS|
|**Smallest burst**|SJF|
|**Smallest remaining burst**|SRTF|
|**Largest burst**|LJF|
|**Largest remaining burst**|LRTF|
|**Time quantum**|RR|
|**Circular queue**|RR|
|**Minimum average waiting time + known burst**|SJF|
|**Preemptive version of SJF**|SRTF|
|**Preemptive version of LJF**|LRTF|
|**New process arrives in SRTF**|Recompare remaining times|
|**Context-switch overhead in RR**|Count switches carefully|
|**All AT = 0**|`TAT = CT`|

---

# 🧠 The Most Important Problem-Solving Workflow

For **any scheduling numerical**, don't start calculating WT immediately.

Do this:

```text
1. Identify algorithm
       ↓
2. Write AT + BT
       ↓
3. Determine who gets CPU
       ↓
4. Draw Gantt chart
       ↓
5. Find CT
       ↓
6. Find TAT = CT − AT
       ↓
7. Find WT = TAT − BT
       ↓
8. Average if asked
```

### And for preemptive algorithms:

```text
NEW PROCESS ARRIVES
        ↓
Recalculate remaining times
        ↓
Does another process now have
a better priority?
        ↓
YES → PREEMPT
NO  → Continue current process
```

### For RR:

```text
Run for q
   ↓
Finished?
 ┌─┴─┐
YES  NO
 ↓    ↓
Done  back of queue
```

This **L-6 lecture is basically the numerical side of L-5**: L-5 tells you _what each algorithm means_, while L-6 trains you to **recognize the algorithm and construct the Gantt chart correctly**. The lecture covers RR, FCFS, SJF, SRTF, LJF and LRTF through these examples and GATE questions.