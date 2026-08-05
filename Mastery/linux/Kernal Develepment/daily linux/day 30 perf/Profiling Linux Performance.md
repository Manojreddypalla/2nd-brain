# 🐧 Linux Internals — Day 30: `perf`

> 🎯 **Core idea:** `perf` tells you **where CPU time is going and what the CPU/system was doing while your program ran.**

---

## 1. Why Do We Need `perf`?

Suppose your program takes:

```text
10 seconds
```

You know it's slow.

But **why?**

```text
Too many instructions?
Too many CPU cycles?
Cache misses?
Context switching?
One slow function?
Kernel work?
```

Simply knowing:

```text
Program took 10 seconds
```

isn't enough.

We want:

```text
WHY did it take 10 seconds?
```

That's where `perf` comes in.

---

# 2. What is `perf`?

`perf` is Linux's performance profiling/analysis toolset.

It can collect information from:

- CPU hardware performance counters
    
- kernel performance events
    
- software events
    
- sampling/profiling infrastructure
    

Conceptually:

```text
Application
     │
     ▼
Linux Kernel
     │
     ▼
CPU
 │
 ├── cycles
 ├── instructions
 ├── cache events
 ├── branches
 └── other events
     │
     ▼
   👀 perf
```

Think:

> **`perf` = "Show me what happened while this program was running."**

---

# 3. `time` vs `perf`

This distinction is important.

Run:

```bash
time ./program
```

It tells you roughly:

```text
real    5.2s
user    4.8s
sys     0.3s
```

So `time` answers:

> **How long did it take?**

But:

```bash
perf stat ./program
```

can give statistics such as:

```text
CPU cycles
Instructions
Context switches
Page faults
Branches
Cache events
...
```

So `perf` helps answer:

> **What happened while it ran?**

Mental model:

```text
time
 ↓
HOW LONG?


perf
 ↓
WHAT HAPPENED / WHERE WAS TIME SPENT?
```

---

# 4. CPU Cycles

A CPU operates using clock cycles.

Very simplified:

```text
Clock
 ↓
tick tick tick tick tick...
 ↓
CPU performs work
```

`perf` can count CPU cycles used while your workload executes.

```text
Program
   ↓
CPU
   ↓
3,000,000,000 cycles
```

More cycles can indicate more CPU work, though cycles alone don't tell you whether the program is inefficient.

---

# 5. Instructions

Your C/C++ code:

```c
x = a + b;
```

eventually becomes machine instructions executed by the CPU.

Conceptually:

```text
C/C++
 ↓
Compiler
 ↓
Machine Instructions
 ↓
CPU
```

`perf` can count retired/executed instruction-related hardware events when supported.

Example output might include:

```text
3 billion cycles
2 billion instructions
```

This leads to an important metric.

---

# 6. IPC — Instructions Per Cycle

Here IPC means:

> **Instructions Per Cycle**

⚠️ Not your previous:

> Inter-Process Communication.

Formula:

```text
IPC =
Instructions
────────────
Cycles
```

Example:

```text
Instructions = 4 billion
Cycles       = 2 billion

IPC = 2
```

Meaning approximately:

```text
2 instructions completed per CPU cycle
```

Higher/lower IPC isn't automatically "good/bad"; it depends heavily on workload and CPU architecture.

---

# 7. Cache Misses

Remember the memory hierarchy:

```text
CPU
 │
 ▼
L1 Cache     ← very fast
 │
 ▼
L2 Cache
 │
 ▼
L3 Cache
 │
 ▼
RAM          ← much slower
```

The CPU prefers finding required data in cache.

### Cache hit

```text
CPU needs data
      ↓
Cache
      ↓
Found ✅
```

### Cache miss

```text
CPU needs data
      ↓
Cache
      ↓
Not found ❌
      ↓
Search lower cache / memory
```

More costly memory accesses can slow CPU-heavy programs.

`perf` can measure supported cache-related events.

---

# 8. Context Switches

Remember scheduling:

```text
Process A running
       ↓
Scheduler
       ↓
Process B running
```

That transition involves a:

> **Context Switch**

`perf stat` can count context switches.

```text
1,500 context switches
```

A high count may be relevant for some workloads, but context switches are also perfectly normal.

Always interpret them in context.

---

# 9. Page Faults

From Day 28:

```text
Process accesses virtual page
          ↓
Page fault
          ↓
Kernel handles it
```

`perf` can count page-fault events too.

So your topics are starting to connect:

```text
Virtual Memory
      ↓
Page Fault
      ↓
Kernel
      ↓
perf measures/counts events
```

---

# 10. The Five Commands You Need

Don't try to memorize the whole `perf` ecosystem.

Remember:

```text
perf stat
perf top
perf record
perf report
perf list
```

---

# 11. `perf stat` — Quick Statistics ⭐

Run:

```bash
perf stat ls
```

Think:

> **"Give me performance counters/statistics for this command."**

You may see things such as:

```text
task-clock
context-switches
page-faults
cycles
instructions
branches
branch-misses
```

This gives you a quick overview.

### Mental model

```text
Program starts
     ↓
perf starts counters
     ↓
Program runs
     ↓
perf counts events
     ↓
Program ends
     ↓
Show statistics
```

Use `perf stat` when you want:

> **A quick performance summary.**

---

# 12. `perf top` — Live View

Think of:

```bash
top
```

`top` shows processes consuming CPU.

`perf top` goes deeper and shows **functions/symbols where CPU samples are landing**.

```text
CPU
 │
 ├── function_A()  █████████ 40%
 │
 ├── function_B()  ██████    30%
 │
 ├── kernel_func() ███       15%
 │
 └── others        ███       15%
```

So:

```text
top
 ↓
Which PROCESS?


perf top
 ↓
Which FUNCTION/SYMBOL?
```

This is a useful mental distinction.

---

# 13. `perf record`

Suppose your program is slow and you want deeper profiling.

Run:

```bash
perf record ./program
```

Conceptually:

```text
Program running

function A
function A
function B
function A
function C
...
      ↑
perf periodically samples execution
```

Instead of recording every CPU instruction, profiling commonly uses **sampling**.

Think of taking photos:

```text
📸 Where is CPU now? → function A

📸 Where now? → function A

📸 Where now? → function B

📸 Where now? → function A
```

If most samples land in function A:

```text
function A → 65%
```

it's probably a CPU hotspot worth investigating.

The data is saved by default into:

```text
perf.data
```

---

# 14. `perf report`

After:

```bash
perf record ./program
```

run:

```bash
perf report
```

Now `perf` analyzes:

```text
perf.data
```

and presents the collected profile.

Conceptually:

```text
Program
   ↓
perf record
   ↓
perf.data
   ↓
perf report
   ↓

Function A → 60%
Function B → 20%
Kernel     → 10%
Others     → 10%
```

So remember the pair:

```text
RECORD
   ↓
perf.data
   ↓
REPORT
```

---

# 15. `perf list`

Run:

```bash
perf list
```

This shows performance events available on your system.

Depending on CPU/kernel, you'll find categories related to:

```text
CPU cycles
Instructions
Branches
Cache
Software events
Tracepoints
...
```

You don't need to memorize them.

Use `perf list` as:

> **"What can this machine/kernel measure?"**

---

# 16. Real Example

Imagine you wrote:

```text
myserver
```

and users complain it's slow.

First:

```bash
time ./myserver
```

You discover:

```text
Slow → confirmed
```

Then:

```bash
perf stat ./myserver
```

Maybe you notice interesting statistics around:

```text
cycles
instructions
cache misses
context switches
```

Now you want to know **where CPU time goes**:

```bash
perf record ./myserver
```

Then:

```bash
perf report
```

Suppose:

```text
65% → parse_data()
20% → calculate_result()
10% → kernel
 5% → other
```

Now you have evidence:

```text
Performance problem
       ↓
Don't randomly optimize everything
       ↓
Investigate parse_data()
```

That's profiling.

---

# 17. Why Profiling Before Optimization Matters

Without profiling:

```text
Program slow
    ↓
"I think this function is bad."
    ↓
Rewrite it
    ↓
Still slow 💀
```

With profiling:

```text
Program slow
    ↓
Measure
    ↓
Find hotspot
    ↓
Understand WHY
    ↓
Optimize
    ↓
Measure again
```

This is the engineering mindset:

> **Measure → Understand → Optimize → Measure again**

---

# 18. `strace` vs `perf`

Yesterday you learned `strace`.

This distinction is extremely useful.

### `strace`

Asks:

> **What is the program asking the kernel to do?**

```text
Program
  │
  ├── openat()
  ├── read()
  ├── write()
  └── mmap()
       │
       ▼
     Kernel
```

### `perf`

Asks:

> **Where is execution time going and what performance events are happening?**

```text
Program
  │
  ├── function A → 50%
  ├── function B → 25%
  ├── kernel     → 15%
  └── other      → 10%
```

So:

```text
strace
   ↓
Behavior / system calls


perf
   ↓
Performance / profiling
```

---

# 🔗 Connect Everything You've Learned

`perf` brings together several previous topics:

```text
                     perf
                      │
        ┌─────────────┼─────────────┐
        ▼             ▼             ▼
       CPU          Memory       Scheduler
        │             │             │
     cycles       cache misses    context
  instructions    page faults     switches
        │             │             │
        └─────────────┼─────────────┘
                      ▼
                  Performance
```

You're no longer only asking:

> "How does Linux work?"

You're starting to ask:

> **"How efficiently is it working?"**

---

# ⚡ 2-Minute Revision

**`perf`** → Linux performance profiling/analysis toolset.

**`perf stat`**

```text
Quick statistics
```

**`perf top`**

```text
Live CPU hotspots
```

**`perf record`**

```text
Collect samples → perf.data
```

**`perf report`**

```text
Analyze perf.data
```

**`perf list`**

```text
Show available performance events
```

Important concepts:

```text
Cycles          → CPU clock cycles
Instructions    → machine instructions
IPC             → instructions / cycle
Cache misses    → required data not found at a cache level
Context switch  → scheduler switches execution
Page fault      → virtual-memory access needs kernel handling
Sampling        → periodically inspect where execution occurs
Hotspot         → code where significant CPU samples/time accumulate
```

## ⭐ One Mental Model

```text
             PROGRAM
                │
                ▼
              CPU
                │
       ┌────────┼────────┐
       ▼        ▼        ▼
     Cycles   Cache    Instructions
       │      Misses       │
       └────────┼───────────┘
                │
             👀 perf
                │
                ▼
        "Where did the
          work go?"
```

And the most important lesson from Day 30 isn't actually a command:

> **Don't guess why software is slow. Measure it first.**