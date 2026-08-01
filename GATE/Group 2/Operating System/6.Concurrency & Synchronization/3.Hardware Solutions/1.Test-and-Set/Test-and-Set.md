# Test-and-Set (TSL) — Complete GATE Notes

---

# What is Test-and-Set?

**Test-and-Set (TSL)** is a **hardware synchronization instruction** provided by the CPU.

It is used to implement **mutual exclusion** by allowing only one process/thread to enter the **critical section** at a time.

The key property is that it executes **atomically**.

---

# Why Do We Need Test-and-Set?

Consider a lock variable.

```text
lock = 0
```

where

- `0` = Unlocked (Free)
    
- `1` = Locked (Busy)
    

A normal implementation would be:

```cpp
if(lock == 0)
    lock = 1;
```

This is **not safe** because it consists of multiple instructions:

```text
Read lock
↓

Compare lock with 0
↓

Write 1 into lock
```

The CPU may switch to another process after any step.

Result:

```text
P1 reads lock = 0

↓

CPU switches

↓

P2 reads lock = 0

↓

P2 locks it

↓

CPU switches

↓

P1 also locks it
```

Now both processes enter the critical section.

❌ Mutual Exclusion is violated.

---

# Solution

The CPU provides **Test-and-Set**, which combines

```text
Read

+

Modify

+

Return
```

into **one atomic operation**.

No process can interrupt it.

---

# Definition

> **Test-and-Set is an atomic instruction that returns the previous value of a lock and simultaneously sets the lock to 1.**

---

# Syntax

```cpp
boolean TestAndSet(boolean &lock)
{
    boolean old = lock;
    lock = true;
    return old;
}
```

This is **conceptual pseudocode**.

In reality, the CPU executes it as **one atomic machine instruction**.

---

# Understanding `old`

This is the most important concept.

Suppose

```text
lock = 0
```

When Test-and-Set executes,

```cpp
old = lock;
```

copies the **previous value**.

Then

```cpp
lock = 1;
```

locks it.

Finally

```cpp
return old;
```

returns the copied value.

Think of it like this:

```text
Memory

lock = 0

↓

CPU copies it

old = 0

↓

CPU changes memory

lock = 1

↓

Returns old (=0)
```

Notice

```text
old
```

is **NOT another lock**.

It is just a temporary variable used to remember the previous value.

---

# Working Example 1

Initially

```text
lock = 0
```

Process P1 executes

```cpp
TestAndSet(lock)
```

Internally

```text
old = 0

↓

lock = 1

↓

return 0
```

Returned value

```text
0
```

Meaning

```text
The lock was free.
```

P1 enters the Critical Section.

Current memory

```text
lock = 1
```

---

# Working Example 2

Now Process P2 executes

```cpp
TestAndSet(lock)
```

Memory currently

```text
lock = 1
```

Internally

```text
old = 1

↓

lock = 1

↓

return 1
```

Returned value

```text
1
```

Meaning

```text
Someone already owns the lock.
```

P2 cannot enter.

---

# Lock Implementation

```cpp
while(TestAndSet(lock))
{
    // Busy Waiting
}

// Critical Section

lock = false;

// Remainder Section
```

---

# Dry Run

Initially

```text
lock = 0
```

---

### Process P1

Calls

```cpp
TestAndSet(lock)
```

Returns

```text
0
```

Memory becomes

```text
lock = 1
```

P1 enters Critical Section.

---

### Process P2

Calls

```cpp
TestAndSet(lock)
```

Returns

```text
1
```

Condition becomes

```cpp
while(1)
```

P2 keeps trying.

---

### P1 finishes

Executes

```cpp
lock = false;
```

Memory

```text
lock = 0
```

---

### P2 tries again

Now

```cpp
TestAndSet(lock)
```

returns

```text
0
```

P2 enters.

---

# Memory Visualization

```text
Initially

Memory
------

lock = 0



P1 executes

old = 0

lock = 1



Memory

lock = 1



P2 executes

old = 1

lock = 1



Memory

lock = 1



P1 exits

lock = 0



Memory

lock = 0
```

---

# Busy Waiting (Spin Lock)

Suppose

```cpp
while(TestAndSet(lock));
```

The process repeatedly checks the lock.

```text
Try

↓

Failed

↓

Try Again

↓

Failed

↓

Try Again

↓

Failed
```

The CPU keeps executing instructions.

This is called

- Busy Waiting
    
- Spinning
    
- Spin Lock
    

---

# Advantages

✅ Atomic operation

✅ Prevents race conditions

✅ Very fast

✅ Simple implementation

✅ Supported directly by hardware

---

# Disadvantages

### 1. Busy Waiting

Waiting processes continuously consume CPU time.

---

### 2. Starvation

Some processes may never acquire the lock if others repeatedly obtain it first.

---

### 3. Not Fair

There is no guarantee that waiting processes will get the lock in the order they arrived.

---

### 4. Wastes CPU

If the critical section is long, spinning wastes processor cycles.

---

# GATE Points

|Property|Test-and-Set|
|---|---|
|Type|Hardware Instruction|
|Atomic|✅ Yes|
|Returns Old Value|✅ Yes|
|Sets Lock|Yes (to 1)|
|Mutual Exclusion|✅ Yes|
|Busy Waiting|✅ Yes|
|Spin Lock|✅ Yes|
|Fairness|❌ No|
|Starvation|Possible|

---

# Common GATE Trap

### Wrong

```cpp
if(lock == 0)
    lock = 1;
```

❌ Not atomic.

---

### Correct

```cpp
TestAndSet(lock);
```

✅ Atomic.

---

# Interview / GATE One-Liner

> **Test-and-Set is an atomic hardware instruction that reads the current value of a lock, returns that old value, and simultaneously sets the lock to 1. If the returned value is 0, the process acquires the lock; if it is 1, the process spins until the lock becomes free.**

---

# Cheat Sheet

```text
Test-and-Set (TSL)

Purpose
│
├── Implement Mutual Exclusion
│
├── Hardware Instruction
│
├── Atomic
│
├── Returns Previous Value (old)
│
├── Sets lock = 1
│
├── old = 0
│     └── Lock was free → Enter Critical Section
│
├── old = 1
│     └── Lock already occupied → Busy Wait
│
├── Process exits
│     └── lock = 0
│
└── Used to build Spin Locks
```

---

# How to Think About It (Mental Model)

Imagine a hotel room with an **Occupied/Vacant** sign.

```text
Vacant (0)

↓

You check the sign.

↓

At the exact same instant,
you flip it to Occupied (1).

↓

If it was Vacant before (old = 0),
you enter.

↓

If it was already Occupied (old = 1),
you wait outside.

↓

When leaving,
you flip it back to Vacant (0).
```

The important idea is that **checking the sign and flipping it happen in one indivisible action**. That's what makes **Test-and-Set atomic**, and that's why it prevents two processes from entering the critical section simultaneously.