Since you're studying **GATE OS Module 2 (Processes)**, this is one of the **highest-weightage basics**. GATE doesn't just ask "draw the diagram"; it asks **what causes transitions**, **which scheduler is involved**, and **state-based reasoning**.

---

# Process Life Cycle (GATE Notes)

## First Intuition

Imagine you apply for a government job.

- Application submitted
    
- Waiting for interview
    
- Interview happening
    
- Selected
    
- Job finished
    

A process behaves similarly.

A process is **always in one state**.

It moves from one state to another depending on events.

---

# The Five-State Model

```
               admit
          +------------+
          |            |
          v            |
       +--------+      |
       |  NEW   |------+
       +--------+
            |
            | admitted by OS
            v
      +-------------+
      |   READY     |
      +-------------+
            |
            | CPU Scheduler selects
            v
      +-------------+
      |  RUNNING    |
      +-------------+
       ^      |     \
       |      |      \
preempt|      |I/O    \ exit
       |      |        \
       |      v         v
      +-------------+ +------------+
      |  READY      | |TERMINATED  |
      +-------------+ +------------+
            ^
            |
      +--------------+
      | WAIT/BLOCKED |
      +--------------+
            |
            | I/O completes
            |
            +------------>
```

---

# 1. NEW State

```
Program
   ↓
Create Process
   ↓
NEW
```

The OS has **just created the PCB (Process Control Block).**

Things happening:

- PID assigned
    
- PCB created
    
- Resources allocated
    
- Program loaded
    

But...

❌ CPU has not executed it yet.

Think:

> "The process exists but hasn't entered the execution queue."

---

# Transition

```
NEW
  |
  | admitted
  v
READY
```

The OS places it into the **Ready Queue**.

---

# 2. READY State

This is one of the most misunderstood states.

Ready DOES NOT mean executing.

Ready means

> "I have everything except the CPU."

The process already has

- Memory
    
- Code
    
- Stack
    
- Heap
    
- Registers (saved)
    

It only needs

**CPU time.**

---

Example

Suppose

```
P1
P2
P3
```

Only one CPU.

```
CPU → P1

Ready Queue

P2
P3
```

P2 and P3 are READY.

---

# Transition

```
READY
   |
CPU Scheduler
   |
   v
RUNNING
```

This is performed by the

## Short-Term Scheduler

Very important for GATE.

---

# 3. RUNNING State

Now instructions are actually executing.

The CPU fetches

```
Fetch

Decode

Execute
```

continuously.

Now many things can happen.

---

## Case 1

Time Quantum expires

```
RUNNING
    |
    | timer interrupt
    |
    v
READY
```

Example

Round Robin Scheduling.

The OS says

> "Enough.  
> Someone else gets CPU."

---

## Case 2

Needs I/O

```
printf()

scanf()

read()

write()

disk

keyboard

network
```

CPU cannot help here.

So

```
RUNNING
      |
      | I/O request
      v
WAITING
```

---

## Case 3

Process finishes

```
return 0;
```

or

```
exit()
```

Then

```
RUNNING
      |
      v
TERMINATED
```

---

# 4. WAITING (BLOCKED)

This is another favorite GATE concept.

The process **cannot continue even if the CPU is free.**

Why?

Because it's waiting for an event.

Examples

```
Disk read

Keyboard input

Semaphore

Network packet

Child process

Sleep()
```

Think

```
"I'm not ready.
Don't give me CPU."
```

Very important difference:

READY

↓

Needs CPU

WAITING

↓

Needs EVENT

---

Example

```
scanf();
```

User hasn't typed.

CPU is free.

Can process execute?

No.

It must wait.

So

```
Running

↓

Blocked
```

---

# Transition

```
WAITING
     |
     | Event completes
     |
     v
READY
```

Example

Disk finishes.

Network arrives.

Keyboard pressed.

Now the process is ready again.

---

# 5. TERMINATED

Execution completed.

The OS

- Frees memory
    
- Removes PCB
    
- Closes files
    
- Releases resources
    

The process no longer exists.

---

# Complete Transition Table

|From|Event|To|
|---|---|---|
|NEW|Admitted|READY|
|READY|CPU allocated|RUNNING|
|RUNNING|Time quantum expires|READY|
|RUNNING|I/O request|WAITING|
|WAITING|I/O complete|READY|
|RUNNING|Exit|TERMINATED|

Memorize this table—it covers most GATE questions on process states.

---

# Who Causes Each Transition?

|Transition|Responsible|
|---|---|
|NEW → READY|Long-Term Scheduler (Job Scheduler)|
|READY → RUNNING|Short-Term Scheduler (CPU Scheduler)|
|RUNNING → READY|Timer interrupt / Preemption|
|RUNNING → WAITING|Process requests I/O or waits for an event|
|WAITING → READY|Interrupt indicating event/I/O completion|
|RUNNING → TERMINATED|Process finishes or calls `exit()`|

---

# Ready vs Running vs Waiting

|READY|RUNNING|WAITING|
|---|---|---|
|Has everything except CPU|Currently executing|Waiting for an external event|
|Can execute immediately if CPU is assigned|Using CPU now|Cannot execute even if CPU is idle|
|In Ready Queue|On CPU|In Device/Event Queue|

A quick mental check:

- **Ready** → "Give me the CPU."
    
- **Running** → "I'm using the CPU."
    
- **Waiting** → "Don't give me the CPU yet; I'm waiting for something else."
    

---

# Dry Run

Suppose this code:

```c
printf("Hi");
scanf("%d",&x);
printf("%d",x);
return 0;
```

State transitions:

```
NEW
 ↓
READY
 ↓
RUNNING
 ↓
WAITING   (scanf waits for keyboard input)
 ↓
READY     (user enters input)
 ↓
RUNNING
 ↓
TERMINATED
```

Notice that after `scanf()`, the process **does not go directly back to Running**. It first goes to **Ready**, because after the I/O completes it must wait for the CPU scheduler to assign the CPU again.

---

# GATE Trap Questions

### Trap 1

**Can a Waiting process become Running directly?**

❌ No.

It must go:

```
WAITING
   ↓
READY
   ↓
RUNNING
```

---

### Trap 2

**Can a Ready process perform I/O?**

❌ No.

Only the **Running** process can issue system calls like `read()`, `write()`, or `scanf()`.

---

### Trap 3

**If the CPU becomes idle, do all Ready processes become Running?**

❌ No.

Only **one process per CPU core** can be in the Running state at a time.

---

### Trap 4

**Is Waiting the same as Ready?**

❌ No.

- **Ready** → waiting **for the CPU**.
    
- **Waiting/Blocked** → waiting **for an event**.
    

---

## GATE Takeaways

- A process is always in exactly **one state** at a given instant.
    
- The common five states are **New, Ready, Running, Waiting (Blocked), and Terminated**.
    
- **Ready → Running** is controlled by the **Short-Term Scheduler**.
    
- **Running → Ready** occurs due to **preemption** (e.g., timer interrupt).
    
- **Running → Waiting** happens when the process requests I/O or another blocking event.
    
- **Waiting → Ready** occurs after the awaited event completes.
    
- A **Waiting** process can **never** move directly to **Running**.
    
- **Ready** means "waiting for the CPU"; **Waiting** means "waiting for an event."