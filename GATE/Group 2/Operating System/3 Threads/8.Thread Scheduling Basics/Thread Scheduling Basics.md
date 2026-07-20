# Thread Scheduling Basics

> **Goal:** Understand how threads are scheduled, why thread scheduling is needed, who schedules threads, and how scheduling differs between User-Level Threads (ULTs) and Kernel-Level Threads (KLTs).

---

# Why is Thread Scheduling Needed?

Imagine a process has four threads:

```
Process P

├── Thread A
├── Thread B
├── Thread C
└── Thread D
```

But the CPU can execute **only one thread per core at a time**.

Question:

> **Who decides which thread gets the CPU next?**

This decision-making process is called **Thread Scheduling**.

---

# Definition

**Thread Scheduling** is the process of selecting the next thread from the **Ready Queue** and assigning it to the CPU.

Simply put:

> **Thread Scheduling decides "Which thread runs next?"**

---

# Why Can't All Threads Run Together?

Suppose you have:

- 1 CPU Core
- 4 Threads

```
CPU

↓

?
```

The CPU cannot execute all four simultaneously.

It must choose one.

The remaining threads wait in the **Ready Queue**.

---

# Core Idea

```
Ready Queue

↓

Scheduler

↓

Dispatcher

↓

CPU

↓

Running Thread
```

The scheduler chooses.

The dispatcher performs the context switch.

---

# Thread Life Cycle During Scheduling

```
NEW

↓

READY

↓

RUNNING

↓

WAITING

↓

READY

↓

RUNNING

↓

TERMINATED
```

Only threads in the **READY** state are candidates for scheduling.

---

# Internal Working

## Step 1

Thread is created.

```
NEW
```

---

## Step 2

Thread enters the Ready Queue.

```
READY
```

---

## Step 3

Scheduler selects one thread.

```
Scheduler

↓

Thread B
```

---

## Step 4

Dispatcher loads Thread B's context from its **TCB**.

```
Registers

PC

SP
```

---

## Step 5

CPU executes Thread B.

```
RUNNING
```

---

## Step 6

Execution stops because of:

- Time Quantum Expiry
- Blocking System Call
- Higher Priority Thread
- Thread Completion

---

## Step 7

Context is saved into the TCB.

Another READY thread is selected.

---

# Who Performs Thread Scheduling?

It depends on the thread model.

---

## User-Level Threads (ULT)

```
Application

↓

Thread Library

↓

ULT Scheduler

↓

CPU
```

The **Thread Library** schedules User-Level Threads.

The kernel does not know they exist.

---

## Kernel-Level Threads (KLT)

```
Application

↓

Kernel

↓

Kernel Scheduler

↓

CPU
```

The **Kernel Scheduler** schedules Kernel-Level Threads.

The kernel is aware of every thread.

---

# Scheduling Events

A thread may stop running when:

### Time Quantum Expires

```
Thread A

↓

Time Over

↓

Scheduler runs
```

---

### Blocking System Call

```
Thread A

↓

read()

↓

Waiting

↓

Schedule another thread
```

---

### Higher Priority Thread Arrives

```
Low Priority Thread

↓

High Priority Thread becomes READY

↓

Preemption
```

---

### Thread Finishes

```
Thread Ends

↓

Next READY thread runs
```

---

# Relationship with Context Switching

Thread Scheduling decides:

> **Who runs next?**

Context Switching performs:

> **Switching from the current thread to the selected thread.**

```
Scheduler

↓

Select Thread B

↓

Dispatcher

↓

Save TCB A

↓

Load TCB B

↓

CPU Runs Thread B
```

---

# Scheduling vs Dispatcher

| Scheduler | Dispatcher |
|------------|------------|
| Chooses next thread | Switches CPU to that thread |
| Decision making | Execution of decision |
| Uses scheduling algorithm | Performs context switch |

---

# Relationship with Scheduling Algorithms

Thread Scheduling uses CPU scheduling algorithms such as:

- FCFS
- Round Robin
- Priority Scheduling
- Shortest Job First (where applicable)
- Multilevel Queue

The scheduler applies these algorithms to decide which READY thread executes next.

---

# ULT vs KLT Scheduling

| Feature | ULT | KLT |
|----------|-----|-----|
| Scheduler | Thread Library | Kernel Scheduler |
| Kernel Awareness | No | Yes |
| Parallelism | No | Yes |
| Blocking Call | Entire Process | One Thread |
| Context Switch | User Space | Kernel Space |

---

# Real-World Example

Imagine four people waiting to use a printer.

```
Thread A
Thread B
Thread C
Thread D
```

The printer can print only one document at a time.

The office manager decides who prints next.

The office manager is like the **Scheduler**.

The person who physically hands over the printer to the next user is like the **Dispatcher**.

---

# Advantages of Thread Scheduling

- Efficient CPU utilization
- Fair CPU sharing
- Better responsiveness
- Supports multitasking
- Enables concurrent execution

---

# GATE Corner ⭐

## Must Remember

- Thread Scheduling decides **which READY thread executes next.**
- Scheduler makes the decision.
- Dispatcher performs the context switch.
- Scheduling uses information stored in the **TCB**.
- Only READY threads are eligible for scheduling.
- ULT → Thread Library schedules.
- KLT → Kernel Scheduler schedules.

---

## Common GATE Traps ⚠️

❌ Scheduler performs context switching.

✔ False.

The **Dispatcher** performs the context switch.

---

❌ Dispatcher decides which thread runs next.

✔ False.

The **Scheduler** makes that decision.

---

❌ Waiting threads are scheduled immediately.

✔ False.

Only **READY** threads are considered.

---

❌ ULTs are scheduled by the kernel.

✔ False.

ULTs are scheduled by the **Thread Library**.

---

❌ Thread Scheduling and CPU Scheduling are different concepts.

✔ Not exactly.

For **Kernel-Level Threads**, CPU scheduling is essentially **thread scheduling**, because the kernel schedules threads directly.

---

# PYQ Focus 🎯

- Scheduler vs Dispatcher
- Ready Queue
- Thread States
- Context Switching
- ULT vs KLT Scheduling
- Scheduling Algorithms
- TCB and Scheduling