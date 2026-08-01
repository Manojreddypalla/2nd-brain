# Compare-and-Swap (CAS) — Complete GATE Notes

---

# What is Compare-and-Swap (CAS)?

**Compare-and-Swap (CAS)** is an **atomic hardware synchronization instruction**.

It is used to implement:

- Mutual Exclusion
- Lock-Free Algorithms
- Atomic Updates

Unlike **Test-and-Set**, CAS **updates memory only if the current value matches an expected value**.

---

# Why Do We Need CAS?

Suppose

```text
lock = 0
```

Many processes are trying to acquire the lock.

With Test-and-Set,

every process executes

```text
lock = 1
```

even when the lock is already locked.

This creates unnecessary memory writes.

CAS improves this by saying

> **Only change the value if it is exactly what I expect.**

Otherwise,

do nothing.

---

# Intuition

Imagine a hotel room.

The sign says

```text
VACANT
```

You tell the receptionist:

> "Only change the sign to OCCUPIED if it is STILL VACANT."

If someone else already changed it,

don't touch it.

That is exactly what CAS does.

---

# Definition

> **Compare-and-Swap is an atomic hardware instruction that compares the current value of a memory location with an expected value. If both are equal, it replaces the memory value with a new value; otherwise, it leaves the memory unchanged.**

---

# Syntax

```cpp
CompareAndSwap(memory,
               expected,
               newValue)
```

Parameters

| Parameter | Meaning |
|-----------|---------|
| memory | Variable to check |
| expected | Expected current value |
| newValue | Value to write if comparison succeeds |

---

# Internal Working

Conceptually

```cpp
old = memory;

if(old == expected)
{
    memory = newValue;
}

return old;
```

> **Note:** These steps are executed atomically by the CPU.

---

# How CAS Works

Initially

```text
lock = 0
```

Execute

```cpp
CAS(lock,0,1)
```

Meaning

```text
Is lock equal to 0?

↓

YES

↓

Replace it with 1

↓

Return old value
```

Result

```text
old = 0

lock = 1
```

---

# Example 2

Initially

```text
lock = 1
```

Execute

```cpp
CAS(lock,0,1)
```

Comparison

```text
1 == 0 ?

↓

NO
```

Result

```text
old = 1

lock = 1
```

Memory remains unchanged.

---

# Dry Run

Initially

```text
lock = 0
```

### Process P1

```cpp
CAS(lock,0,1)
```

Comparison

```text
0 == 0
```

Success

```text
lock = 1
```

P1 enters Critical Section.

---

### Process P2

Current memory

```text
lock = 1
```

P2 executes

```cpp
CAS(lock,0,1)
```

Comparison

```text
1 == 0
```

Fails.

Memory remains

```text
lock = 1
```

P2 waits.

---

# Memory Visualization

Initially

```text
Memory

lock = 0
```

CAS

```text
Compare

0 == 0

↓

Success

↓

Write 1
```

Memory

```text
lock = 1
```

---

Another Case

```text
Memory

lock = 1
```

CAS

```text
Compare

1 == 0

↓

Fail

↓

No Write
```

Memory

```text
lock = 1
```

---

# Lock Implementation Using CAS

```cpp
while(CompareAndSwap(&lock,0,1)!=0)
{
    // Busy Waiting
}

// Critical Section

lock = 0;
```

---

# Difference Between CAS and Test-and-Set

## Test-and-Set

```cpp
old = lock;

lock = 1;

return old;
```

Always writes to memory.

---

## Compare-and-Swap

```cpp
if(lock == expected)

lock = newValue;
```

Writes **only when the comparison succeeds.**

---

# Visual Difference

## Test-and-Set

```text
lock = 1

↓

Write 1

↓

Write 1

↓

Write 1
```

Always writes.

---

## Compare-and-Swap

```text
lock = 1

↓

Compare

↓

Not Equal

↓

No Write
```

Only the successful process updates memory.

---

# Advantages

- Atomic operation
- Prevents Race Conditions
- Reduces unnecessary memory writes
- Efficient on multicore processors
- Used in lock-free programming
- More flexible than Test-and-Set

---

# Disadvantages

- Busy Waiting
- Starvation Possible
- More complex than Test-and-Set
- ABA Problem (Advanced topic)

---

# Applications

CAS is widely used in

- std::atomic (C++)
- Java AtomicInteger
- Linux Kernel
- Lock-Free Queue
- Lock-Free Stack
- Concurrent Programming

---

# Compare-and-Swap vs Test-and-Set

| Feature | Test-and-Set | Compare-and-Swap |
|----------|--------------|------------------|
| Hardware Instruction | ✅ | ✅ |
| Atomic | ✅ | ✅ |
| Returns Old Value | ✅ | ✅ |
| Always Writes | ✅ | ❌ |
| Conditional Update | ❌ | ✅ |
| Writes Only If Needed | ❌ | ✅ |
| Busy Waiting | ✅ | ✅ |
| Spin Lock | ✅ | ✅ |
| Used in Lock-Free Algorithms | ❌ | ✅ |

---

# Busy Waiting

Like Test-and-Set,

CAS is commonly used as

```cpp
while(CAS(lock,0,1)!=0);
```

The process continuously retries until the comparison succeeds.

This is called

- Busy Waiting
- Spin Waiting
- Spin Lock

---

# Important GATE Points

- CAS is a hardware synchronization instruction.
- CAS is atomic.
- CAS compares before updating.
- Memory changes only when the comparison succeeds.
- CAS reduces unnecessary memory writes.
- CAS is used in modern lock-free algorithms.
- Busy Waiting is still present.
- Fairness is not guaranteed.
- Starvation is possible.

---

# Common GATE Traps

## Trap 1

> CAS always updates memory.

❌ False

CAS updates memory **only if the comparison succeeds.**

---

## Trap 2

> CAS is a software algorithm.

❌ False

CAS is a hardware instruction.

---

## Trap 3

> CAS removes Busy Waiting.

❌ False

Busy Waiting still exists.

---

## Trap 4

> CAS guarantees fairness.

❌ False

Starvation is still possible.

---

## Trap 5

> CAS prevents Race Conditions.

✅ True

Because compare and update happen atomically.

---

# Interview / GATE One-Liner

> **Compare-and-Swap (CAS) is an atomic hardware instruction that compares the current value of a memory location with an expected value and updates it to a new value only if they match.**

---

# GATE Cheat Sheet

```text
Compare-and-Swap (CAS)

Purpose
│
├── Mutual Exclusion
├── Lock-Free Algorithms
│
├── Hardware Instruction
│
├── Atomic
│
├── Compare Memory
│
├── Match?
│     │
│     ├── Yes → Update Memory
│     └── No → Leave Memory Unchanged
│
├── Returns Old Value
│
├── Busy Waiting
│
├── Spin Lock
│
└── Process Releases Lock Manually
```

---

# Memory Trick 🧠

Think of CAS as:

> **"Compare first, then Swap."**

Remember the flow:

```text
Compare
     │
     ▼
Equal?
  │
  ├── Yes → Update
  │
  └── No → Do Nothing
```

Unlike Test-and-Set, CAS **does not blindly write**. It updates memory **only when the current value is exactly what you expected**, making it the foundation of modern lock-free concurrent programming.