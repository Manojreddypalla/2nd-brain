# Swap Instruction — Complete GATE Notes

---

# What is Swap Instruction?

**Swap** is an **atomic hardware synchronization instruction**.

It **atomically exchanges (swaps)** the values of two variables.

It is used to implement:

- Mutual Exclusion
- Spin Locks
- Critical Section Protection

---

# Why Do We Need Swap?

Suppose

```text
lock = 0
```

where

```text
0 → Free

1 → Locked
```

If two processes execute

```cpp
lock = 1;
```

simultaneously,

both may enter the Critical Section.

To avoid this, the CPU provides an **atomic Swap instruction**.

---

# Definition

> **Swap is an atomic hardware instruction that exchanges the contents of two variables in one indivisible operation.**

---

# Syntax

```cpp
Swap(lock,key);
```

where

```text
lock → Shared variable

key → Local variable
```

---

# Internal Working

Conceptually

```cpp
temp = lock;

lock = key;

key = temp;
```

> **Note:** These three operations happen atomically.

No process can interrupt in between.

---

# Initial Values

Usually,

```text
lock = 0

key = 1
```

Meaning

```text
lock = Free

key = Wants Lock
```

---

# Example

Initially

```text
lock = 0

key = 1
```

Execute

```cpp
Swap(lock,key);
```

Result

```text
lock = 1

key = 0
```

Meaning

```text
The process successfully acquired the lock.
```

---

# Another Example

Initially

```text
lock = 1

key = 1
```

Execute

```cpp
Swap(lock,key);
```

Result

```text
lock = 1

key = 1
```

Nothing changes.

The process keeps waiting.

---

# Dry Run

Initially

```text
lock = 0
```

---

### Process P1

```text
key = 1
```

Execute

```cpp
Swap(lock,key);
```

Result

```text
lock = 1

key = 0
```

Since

```text
key == 0
```

P1 enters Critical Section.

---

### Process P2

Initially

```text
lock = 1

key = 1
```

Execute

```cpp
Swap(lock,key);
```

Result

```text
lock = 1

key = 1
```

Since

```text
key == 1
```

P2 keeps waiting.

---

# Lock Implementation

```cpp
key = true;

while(key)
{
    Swap(lock,key);
}

// Critical Section

lock = false;
```

---

# How It Works

Initially

```text
lock = 0
```

P1

```text
Swap(lock,key)

↓

lock = 1

key = 0

↓

Enter
```

P2

```text
Swap(lock,key)

↓

lock = 1

key = 1

↓

Wait
```

P1 exits

```cpp
lock = false;
```

Now another process can acquire the lock.

---

# Memory Visualization

Initially

```text
lock = 0

key = 1
```

Swap

```text
lock = 1

key = 0
```

Another Process

```text
lock = 1

key = 1
```

Swap

```text
lock = 1

key = 1
```

No change.

---

# Advantages

- Atomic
- Simple
- Hardware Supported
- Prevents Race Conditions
- Guarantees Mutual Exclusion

---

# Disadvantages

- Busy Waiting
- CPU Wastage
- No Fairness
- Starvation Possible

---

# Applications

Used for

- Spin Locks
- Mutual Exclusion
- Hardware Synchronization

---

# Comparison

| Feature | Test-and-Set | Compare-and-Swap | Swap |
|----------|--------------|------------------|------|
| Hardware Instruction | ✅ | ✅ | ✅ |
| Atomic | ✅ | ✅ | ✅ |
| Returns Old Value | ✅ | ✅ | ❌ |
| Conditional Update | ❌ | ✅ | ❌ |
| Exchanges Values | ❌ | ❌ | ✅ |
| Busy Waiting | ✅ | ✅ | ✅ |
| Spin Lock | ✅ | ✅ | ✅ |

---

# Important GATE Points

- Swap is a **hardware synchronization instruction**.
- Swap is **atomic**.
- It exchanges two values.
- Used for **Mutual Exclusion**.
- Busy Waiting still exists.
- Fairness is not guaranteed.
- Starvation is possible.

---

# Common GATE Traps

## Trap 1

> Swap is a software algorithm.

❌ False

Swap is a hardware instruction.

---

## Trap 2

> Swap guarantees fairness.

❌ False

Starvation is possible.

---

## Trap 3

> Swap eliminates Busy Waiting.

❌ False

Processes still spin while waiting.

---

## Trap 4

> Swap exchanges two variables atomically.

✅ True

That is its primary purpose.

---

# Interview / GATE One-Liner

> **Swap is an atomic hardware instruction that exchanges the contents of two variables in one indivisible operation, allowing mutual exclusion to be implemented safely.**

---

# Cheat Sheet

```text
Swap Instruction

Purpose
│
├── Mutual Exclusion
├── Spin Lock
│
├── Hardware Instruction
│
├── Atomic
│
├── Exchange Values
│
├── lock ↔ key
│
├── key == 0
│      │
│      └── Enter Critical Section
│
├── key == 1
│      │
│      └── Busy Wait
│
└── Release
       │
       └── lock = 0
```

---

# Memory Trick 🧠

Think of Swap as:

> **"Exchange the lock and my key."**

```text
Before

lock = 0

key = 1

↓

Swap

↓

lock = 1

key = 0

↓

Enter Critical Section
```

Unlike Test-and-Set (always sets to 1) and CAS (compares before updating), **Swap simply exchanges two values atomically.**