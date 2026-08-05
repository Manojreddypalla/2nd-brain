# 🐧 Linux Internals – Day 24

## Linux Scheduling – How the CPU Chooses Which Process Runs

> [!goal] Goal  
> Understand how the Linux kernel decides **which task gets CPU time**, how scheduling priorities work, what nice values mean, and how Linux attempts to distribute CPU time fairly.

---

# 1. Why Do We Need a Scheduler?

A Linux system may have hundreds or thousands of processes.

Example:

```text
Chrome
Terminal
VS Code
Docker
SSH
Music Player
System Services
Background Jobs
```

But a CPU core can execute only **one hardware thread at a particular instant**.

Therefore Linux needs something to decide:

```text
Who runs?
When?
For how long?
Who runs next?
```

That component is the:

# CPU Scheduler

The scheduler is part of the **Linux kernel**.

---

# 2. Basic Mental Model

Imagine one CPU core:

```text
Runnable Tasks

P1
P2
P3
P4
P5
 │
 ▼
Linux Scheduler
 │
 ▼
CPU Core
```

The scheduler repeatedly chooses a runnable task.

Conceptually:

```text
P1 runs
   ↓
scheduler decision
   ↓
P3 runs
   ↓
scheduler decision
   ↓
P2 runs
   ↓
...
```

Switching execution from one task to another involves a:

# Context Switch

---

# 3. Process States Matter

The scheduler doesn't choose randomly from **every process in Linux**.

Consider:

```text
P1 → Running
P2 → Runnable
P3 → Sleeping
P4 → Waiting for disk
P5 → Stopped
```

A sleeping process doesn't currently need CPU time.

The scheduler is mainly interested in **runnable tasks**.

Simplified:

```text
                ┌── P1
Runnable Tasks ─┼── P2
                └── P3
                     │
                     ▼
                 Scheduler
                     │
                     ▼
                    CPU
```

---

# 4. Running vs Runnable

These are different concepts.

### Running

The task is currently executing on a CPU.

```text
Process
   ↓
CPU executing instructions
```

### Runnable

The task is ready to execute but may currently be waiting for CPU time.

```text
Process
   ↓
Ready
   ↓
Waiting for scheduler
```

Mental model:

```text
Runnable
   │
scheduler selects it
   ▼
Running
   │
preempted
   ▼
Runnable
```

---

# 5. Scheduler With Multiple CPU Cores

Suppose your CPU has 8 logical CPUs.

Linux can execute multiple tasks simultaneously.

```text
Scheduler
   │
   ├── P1 → CPU 0
   ├── P2 → CPU 1
   ├── P3 → CPU 2
   ├── P4 → CPU 3
   ├── P5 → CPU 4
   └── ...
```

Scheduling therefore happens **per CPU**, with Linux also performing load balancing between CPUs.

Conceptually:

```text
CPU 0 runqueue
P1 P3 P7

CPU 1 runqueue
P2 P5

CPU 2 runqueue
P4 P6 P8
```

Linux tries to avoid situations such as:

```text
CPU 0 → overloaded
CPU 1 → idle
```

and may migrate tasks between CPUs.

---

# 6. What is a Runqueue?

A **runqueue** represents runnable tasks associated with a CPU.

Simplified:

```text
CPU 0
 │
 ▼
Runqueue
 │
 ├── P1
 ├── P2
 └── P3
      │
      ▼
 Scheduler
      │
      ▼
    CPU 0
```

Modern Linux scheduling structures are more sophisticated than a simple FIFO queue.

The important idea is:

> The scheduler maintains information about runnable tasks and selects which task should execute next.

---

# 7. Preemptive Scheduling

Linux uses **preemptive multitasking**.

That means the kernel can stop one runnable task from continuing on the CPU so another task can run.

Conceptually:

```text
P1 running
   │
   │ scheduler decides
   ▼
P1 preempted
   │
   ▼
P2 runs
```

P1 hasn't terminated.

It simply becomes runnable again.

```text
Running
   ↓
Preempted
   ↓
Runnable
   ↓
Scheduled again
   ↓
Running
```

This happens extremely quickly.

That's why it appears that hundreds of processes are running simultaneously.

---

# 8. Context Switching

Suppose:

```text
CPU
 │
 ▼
P1
```

The scheduler decides P2 should run.

Linux must preserve enough execution state of P1 and switch to P2.

Conceptually:

```text
P1 running
   ↓
Save P1 execution state
   ↓
Select P2
   ↓
Restore/use P2 state
   ↓
P2 running
```

This is a:

# Context Switch

Relevant state includes things such as:

- CPU registers
    
- Stack pointer
    
- Program counter/instruction pointer
    
- Scheduling state
    

Context switching has overhead.

Therefore:

> More context switches are not automatically better.

---

# 9. Scheduler Goals

A good general-purpose scheduler has competing goals.

Linux tries to balance things such as:

### Fairness

Don't let one normal process permanently monopolize the CPU.

### Responsiveness

Interactive applications should remain responsive.

Example:

```text
Mouse
Terminal
Desktop
Browser
```

### Throughput

Complete useful work efficiently.

### Latency

Tasks shouldn't wait unnecessarily long before receiving CPU time.

### Scalability

Scheduling must continue working efficiently on systems with many CPU cores and huge numbers of tasks.

---

# 10. Process Priority

Not every process should necessarily receive exactly the same scheduling treatment.

Linux supports different scheduling priorities and scheduling classes.

For ordinary user processes, one important control is:

# Nice Value

---

# 11. Nice Value

The nice value influences the scheduling weight/preference of ordinary tasks.

Range:

```text
-20                         0                         19
 │                          │                          │
 ▼                          ▼                          ▼
Higher CPU              Default                    Lower CPU
preference                                         preference
```

Remember:

```text
Lower nice
    ↓
Less "nice" to other processes
    ↓
Higher scheduling weight/preference
```

while:

```text
Higher nice
    ↓
More "nice" to others
    ↓
Lower scheduling weight/preference
```

---

# 12. Why is it Called "Nice"?

Think about two processes competing for CPU.

Process A:

```text
nice = 0
```

Process B:

```text
nice = 15
```

Process B is effectively saying:

> "I'm willing to yield more CPU opportunity to other competing tasks."

Therefore it is being more **nice**.

---

# 13. Important Nice Values

```text
Nice -20
   ↓
Highest normal scheduling preference

Nice 0
   ↓
Default

Nice +19
   ↓
Lowest normal scheduling preference
```

Important:

> Nice value is not a direct percentage of CPU.

For example:

```text
nice 10
```

does NOT mean:

```text
10% CPU
```

Nice values influence the **relative scheduling weight** when runnable tasks compete for CPU.

---

# 14. Nice Matters Under CPU Contention

Suppose:

```text
Process A
nice = 0

Process B
nice = 15
```

If you have many idle CPU cores, both may simply get a CPU.

Then you might observe:

```text
A → ~100% of one CPU
B → ~100% of another CPU
```

and think:

> "Nice isn't doing anything!"

But there is no CPU contention.

Nice becomes easy to observe when both tasks compete for the **same CPU capacity**.

This is extremely important for the lab.

---

# 15. `nice` Command

`nice` starts a **new process** with an adjusted nice value.

Example:

```bash
nice -n 10 yes > /dev/null
```

Breakdown:

```text
nice
 │
 └── start command with adjusted niceness

-n
 │
 └── specify nice adjustment/value

10
 │
 └── requested niceness adjustment

yes
 │
 └── CPU-intensive command

> /dev/null
 │
 └── discard its output
```

Check:

```bash
ps -eo pid,ni,comm | grep yes
```

---

# 16. `renice` Command

`renice` changes the nice value of an **existing process**.

Example:

```bash
renice 10 -p 1234
```

Meaning:

```text
Process already running
       │
       ▼
PID 1234
       │
    renice
       ▼
New nice value
```

Difference:

```text
nice
  ↓
start new process
with chosen niceness

renice
  ↓
change niceness
of existing process
```

---

# 17. Increasing vs Decreasing Nice

Normal users can generally make their own processes **nicer**:

```text
0 → 5
0 → 10
10 → 15
```

This reduces their scheduling preference.

But decreasing niceness:

```text
10 → 0
0 → -10
```

increases scheduling preference and normally requires appropriate privilege/capability.

Example:

```bash
sudo renice -10 -p <PID>
```

Why?

Otherwise any process could simply declare:

```text
"I am the most important process!"
```

and unfairly compete for CPU.

---

# 18. `PRI` vs `NI`

Run:

```bash
ps -eo pid,ni,pri,comm | head
```

You may see:

```text
PID    NI   PRI   COMMAND
1234    0    19   bash
1250   10    29   worker
```

### NI

```text
NI = Nice value
```

User-visible niceness adjustment for normal scheduling.

### PRI

```text
PRI = Priority
```

A representation of the scheduler priority exposed by tools such as `ps`.

Do not assume the displayed `PRI` number follows the intuitive rule:

```text
larger number = more important
```

The representation depends on the tool/kernel priority model.

For Day 24, focus mainly on:

```text
NI
```

and remember:

```text
lower NI → greater scheduling weight
higher NI → smaller scheduling weight
```

---

# 19. Scheduling Classes

Linux doesn't schedule every task using exactly the same policy.

There are multiple scheduling classes/policies.

Conceptually:

```text
Linux Scheduler
      │
      ├── Deadline scheduling
      │
      ├── Real-time scheduling
      │
      └── Fair scheduling
             │
             ▼
       Normal processes
```

Examples of policies you'll encounter:

```text
SCHED_DEADLINE
SCHED_FIFO
SCHED_RR
SCHED_OTHER
SCHED_BATCH
SCHED_IDLE
```

Most ordinary applications normally use:

```text
SCHED_OTHER
```

which belongs to Linux's fair scheduling behavior.

---

# 20. Real-Time Scheduling

Linux also supports real-time scheduling.

Examples:

```text
SCHED_FIFO
SCHED_RR
```

These are fundamentally different from ordinary nice-based scheduling.

Conceptually:

```text
Real-time task
      │
      ▼
Higher scheduling class precedence
      │
      ▼
Normal fair-scheduled task
```

Nice values mainly concern ordinary fair-scheduled tasks.

Do NOT think:

```text
nice -20 = real-time
```

It isn't.

---

# 21. CFS — Completely Fair Scheduler

Historically, the major scheduler for ordinary Linux tasks has been:

# CFS — Completely Fair Scheduler

Its core idea was:

> Try to distribute CPU execution fairly among runnable tasks while accounting for their scheduling weights.

Instead of thinking only:

```text
P1 gets fixed 10 ms
P2 gets fixed 10 ms
P3 gets fixed 10 ms
```

think:

```text
How much CPU service has each task effectively received?
```

---

# 22. Virtual Runtime — `vruntime`

One of the key concepts associated with CFS is:

```text
vruntime
```

Short for:

# Virtual Runtime

Conceptually it represents a task's CPU execution progress adjusted according to scheduling weight.

Simplified example:

```text
P1 vruntime = 100
P2 vruntime = 130
P3 vruntime = 180
```

P1 has received the least normalized service.

So historically CFS strongly centered selection around the task with the smallest virtual runtime.

Mental model:

```text
P1 → 100  ← behind
P2 → 130
P3 → 180

Scheduler:
"Give P1 CPU opportunity."
```

---

# 23. How Nice Connects to `vruntime`

Nice values influence task weights.

Conceptually:

```text
Lower nice
   ↓
Higher weight
   ↓
vruntime progresses more slowly
   ↓
Task receives a larger share under contention
```

Higher nice:

```text
Higher nice
   ↓
Lower weight
   ↓
vruntime progresses faster relative to CPU time
   ↓
Task receives a smaller share
```

This is much more accurate than thinking:

```text
nice = fixed CPU priority
```

Instead:

```text
nice
 ↓
weight
 ↓
fair scheduling calculation
 ↓
CPU share under contention
```

---

# 24. Historical CFS Data Structure

Classic CFS organized runnable tasks using a:

# Red-Black Tree

Conceptually:

```text
                vruntime
                   │
            ┌──────┴──────┐
           P2             P4
          /  \            /
        P1   P3          P5
```

Tasks were ordered based largely on virtual runtime.

The leftmost eligible candidate represented the task that was furthest behind in fair CPU service.

This allowed efficient operations around:

```text
Insert task
Remove task
Find next task
```

---

# 25. Modern Linux — EEVDF

Modern Linux fair scheduling evolved beyond classic CFS selection.

Linux started transitioning fair scheduling toward:

# EEVDF

```text
Earliest Eligible Virtual Deadline First
```

EEVDF improves scheduling decisions by considering concepts such as:

```text
Eligibility
Virtual runtime/service
Virtual deadlines
Task latency
```

Very simplified mental model:

```text
Runnable Tasks
      │
      ▼
Which tasks are eligible?
      │
      ▼
Compare virtual deadlines
      │
      ▼
Choose suitable earliest deadline
      │
      ▼
CPU
```

You do NOT need to master EEVDF implementation details today.

Remember:

```text
Older mental model:
CFS → smallest vruntime

Modern direction:
EEVDF → eligibility + virtual deadlines
```

The underlying goal remains:

> Fairly distribute CPU service while maintaining good responsiveness and latency.

---

# 26. CPU-Bound vs I/O-Bound Tasks

Consider:

### CPU-bound

```bash
yes > /dev/null
```

Pattern:

```text
CPU
CPU
CPU
CPU
CPU
```

It constantly wants CPU time.

---

### I/O-bound

Imagine:

```text
read file
   ↓
wait for disk
   ↓
process data
   ↓
wait
```

Pattern:

```text
CPU
 ↓
Sleep
 ↓
Wake
 ↓
CPU
```

The scheduler must handle both efficiently.

Interactive and I/O-heavy workloads often sleep frequently and need good wake-up latency.

---

# 27. Scheduling State Flow

A process might behave like:

```text
          scheduled
Runnable ───────────► Running
   ▲                    │
   │                    │ preempted
   └────────────────────┘

Running
   │
   │ waits for I/O
   ▼
Sleeping
   │
   │ I/O completed
   ▼
Runnable
```

This connects scheduling directly to process states.

---

# 28. Scheduler + Signals

From Day 23:

```bash
kill -STOP PID
```

changes the task into a stopped state.

Stopped tasks aren't normal runnable candidates.

Then:

```bash
kill -CONT PID
```

allows execution to continue.

Connection:

```text
SIGSTOP
   ↓
Stopped
   ↓
Not runnable

SIGCONT
   ↓
Can become runnable
   ↓
Scheduler may select it
   ↓
Running
```

---

# 29. Scheduler + cgroups

Nice controls **relative task scheduling preference**.

But Linux also has:

```text
cgroups
```

which can manage CPU resources for groups of processes.

Conceptually:

```text
                 CPU
                  │
             Scheduler
             /         \
            ▼           ▼
       cgroup A      cgroup B
       Web Server    Background Jobs
```

This is extremely important for:

```text
Docker
Kubernetes
systemd
Containers
Cloud workloads
```

So:

```text
nice
 ↓
individual process scheduling weight

cgroups
 ↓
group-level CPU resource management
```

---

# 30. Scheduler + CPU Affinity

Linux can also restrict which CPUs a task may execute on.

This is called:

# CPU Affinity

Example:

```bash
taskset -cp <PID>
```

Conceptually:

```text
P1
 │
allowed CPUs
 │
 ├── CPU 2
 └── CPU 3
```

rather than:

```text
CPU 0–7
```

Affinity becomes useful for:

- Performance testing
    
- Benchmarking
    
- Low-latency workloads
    
- Debugging scheduler behavior
    

It is also very useful when experimenting with `nice`.

---

# 31. Why Nice Experiments Can Be Misleading

Suppose your machine has 8 logical CPUs.

You start:

```bash
nice -n 0 yes > /dev/null &
nice -n 15 yes > /dev/null &
```

Linux can simply do:

```text
yes nice=0  → CPU 2 → 100%
yes nice=15 → CPU 5 → 100%
```

No competition.

Therefore you may see almost no difference.

To properly demonstrate niceness, make both tasks compete for **one CPU**.

Conceptually:

```text
            CPU 0
              ▲
              │
        ┌─────┴─────┐
        │           │
     nice 0      nice 15
```

Now scheduler weighting becomes visible.

---

# 32. Useful Commands

View scheduling information:

```bash
ps -eo pid,ni,pri,stat,comm
```

Monitor processes:

```bash
top
```

Start with niceness:

```bash
nice -n 10 command
```

Change niceness:

```bash
renice 10 -p PID
```

View process:

```bash
ps -o pid,ni,pri,stat,comm -p PID
```

Check scheduling policy:

```bash
chrt -p PID
```

Check CPU affinity:

```bash
taskset -cp PID
```

---

# 🧠 Complete Scheduler Mental Model

```text
                     Linux System
                          │
                    Many Processes
                          │
        ┌─────────────────┴─────────────────┐
        │                                   │
     Sleeping                            Runnable
                                            │
                                            ▼
                                    Scheduling Class
                                            │
                                     Fair Scheduler
                                            │
                               ┌────────────┴────────────┐
                               │                         │
                         Nice / Weight            Runtime History
                               │                         │
                               └────────────┬────────────┘
                                            │
                                      Scheduler
                                            │
                                            ▼
                                         CPU Core
                                            │
                                      Task Executes
                                            │
                       ┌────────────────────┴──────────────────┐
                       │                                       │
                   Preempted                              Wait / Sleep
                       │                                       │
                       ▼                                       ▼
                    Runnable                                Sleeping
```

---

# ⚡ Nice vs Renice

|`nice`|`renice`|
|---|---|
|Starts a new process|Modifies existing process|
|Set niceness at launch|Change niceness while running|
|`nice -n 10 command`|`renice 10 -p PID`|

---

# ⚡ CPU Priority Mental Model

Do NOT memorize:

```text
nice = exact CPU priority
```

Think:

```text
Nice value
    ↓
Scheduling weight
    ↓
Fair scheduler
    ↓
Relative CPU share
when tasks compete
```

---

# 🎯 Revision Questions

### Why does Linux need a scheduler?

Because there can be more runnable tasks than available CPUs. The scheduler decides which runnable task executes.

### What is a runnable process?

A task that is ready to execute and waiting for CPU time.

### What is preemption?

Stopping the execution of one task so another task can execute.

### What is a context switch?

Changing CPU execution from one task to another while preserving the necessary execution state.

### What is the nice range?

```text
-20 → highest normal preference
  0 → default
+19 → lowest normal preference
```

### Does nice directly specify CPU percentage?

No.

It influences the task's **relative scheduling weight**.

### `nice` vs `renice`?

```text
nice
→ launch process with adjusted niceness

renice
→ modify niceness of existing process
```

### What was the central idea of CFS?

Distribute CPU service fairly between runnable tasks, using weighted virtual runtime as a major mechanism.

### What is `vruntime`?

A scheduling measure representing normalized CPU service received by a task.

### What is EEVDF?

```text
Earliest Eligible Virtual Deadline First
```

The modern fair scheduling approach used by current Linux kernels, incorporating eligibility and virtual deadlines.

### Why might two differently niced processes both show ~100% CPU?

Because they may be running on different CPU cores and therefore aren't actually competing.

---

# 🔗 Connections to Previous Days

```text
Processes
    ↓
Which processes are runnable?
    ↓
Scheduler
    ↓
CPU

Memory
    ↓
Process address space

Signals
    ↓
Change process state
    ↓
Scheduler sees runnable/stopped state

/proc
    ↓
Observe process information

cgroups
    ↓
Control CPU resources for groups

CPU affinity
    ↓
Control which CPUs may run a task
```

---

# 💻 Command Cheat Sheet

```bash
# Process priority information
ps -eo pid,ni,pri,stat,comm | head

# Interactive process monitor
top

# CPU-intensive process
yes > /dev/null

# Start with nice 10
nice -n 10 yes > /dev/null

# Background CPU task
nice -n 10 yes > /dev/null &

# PID of latest background process
PID=$!

# Inspect nice value
ps -o pid,ni,pri,stat,comm -p $PID

# Change niceness
renice 15 -p $PID

# Terminate yes processes
pkill yes

# Check scheduling policy
chrt -p $PID

# Check CPU affinity
taskset -cp $PID
```

---

# 🔥 One-Line Memory

> **The Linux scheduler decides which runnable task executes on each CPU; nice values influence the relative scheduling weight of normal processes rather than assigning a fixed CPU percentage.**

And visualize the whole topic as:

```text
Runnable Tasks
      │
      ▼
Scheduling Policy
      │
      ▼
Nice → Weight
      │
      ▼
Fair Scheduler
      │
      ▼
Select Task
      │
      ▼
CPU Core
      │
      ▼
Execution
      │
 ┌────┴─────┐
 ▼          ▼
Sleep    Preempted
            │
            ▼
         Runnable
```