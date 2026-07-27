# 🎯 GATE-Level MCQs

## Q1 ⭐

A race condition can occur only when:

A) Multiple threads execute sequentially

B) Multiple execution units access shared data concurrently

C) Only one process accesses memory

D) Paging is enabled

<details> <summary>✅ Answer</summary>

**B**

A race condition requires concurrent access to shared data.

</details>

---

## Q2 ⭐⭐

Which of the following is **necessary** for a race condition?

A) Multiple CPU cores

B) Shared resource

C) Virtual Memory

D) Cache Memory

<details> <summary>✅ Answer</summary>

**B**

Multiple cores are **not** necessary. Even a single-core system can have race conditions because of context switching.

</details>

---

## Q3 ⭐⭐⭐ (Classic GATE)

Two threads increment a shared variable `x` without synchronization.

Initially:

```
x = 5;
```

Each executes:

```
x = x + 1;
```

What is the possible final value?

A) 6 only

B) 7 only

C) 6 or 7

D) 5

<details> <summary>✅ Answer</summary>

**C**

Correct execution → 7.

Lost update due to race condition → 6.

</details>

---

## Q4 ⭐⭐⭐

Which statement is TRUE?

A) Race conditions occur only in multiprocessor systems.

B) Race conditions require simultaneous execution.

C) Race conditions can occur due to context switching on a single-core processor.

D) Race conditions are impossible with threads.

<details> <summary>✅ Answer</summary>

**C**

Context switching can interleave operations and produce race conditions.

</details>

---

## Q5 ⭐⭐⭐⭐ (Statement-Based)

Consider the following statements:

1. A race condition requires shared data.
2. At least one concurrent operation must modify the shared data.
3. Synchronization mechanisms can prevent race conditions.

Choose the correct option:

A) 1 only

B) 1 and 2 only

C) 2 and 3 only

D) 1, 2 and 3

<details> <summary>✅ Answer</summary>

**D**

All three statements are correct.

</details>

---

## Q6 ⭐⭐⭐⭐ (Code-Based)

```
int count = 0;

// Thread A
count++;

// Thread B
count++;
```

Initially:

```
count = 0
```

Without synchronization, the final value of `count` can be:

A) 0

B) 1

C) 2

D) Both 1 and 2

<details> <summary>✅ Answer</summary>

**D**

If one increment is lost due to a race condition, the final value can be 1. If both increments are applied correctly, it will be 2.

</details>

---

## Q7 ⭐⭐⭐⭐⭐ (PYQ Style)

Which of the following can eliminate race conditions?

A) Increasing CPU speed

B) Context Switching

C) Mutual Exclusion

D) Virtual Memory

<details> <summary>✅ Answer</summary>

**C**

Mutual exclusion ensures only one execution unit enters the critical section at a time, preventing race conditions.

</details>

---

## Q8 ⭐⭐⭐⭐⭐ (GATE Trap)

A race condition depends primarily on:

A) Memory size

B) Cache capacity

C) Order of execution of concurrent operations

D) Number of CPU registers

<details> <summary>✅ Answer</summary>

**C**

The defining feature of a race condition is that the final result depends on the execution order (timing) of concurrent operations.

</details>

---

# 🧠 Memory Trick

```
Concurrency
      ↓
Shared Resource
      ↓
Simultaneous/Overlapping Access
      ↓
Write Operation
      ↓
Race Condition
      ↓
Wrong Result
```

### 🎯 GATE Memory Line

> **Race Condition = Shared Data + Concurrent Access + Unsynchronized Write = Unpredictable Result**

---

## 🔥 One Thought Question

Suppose **two threads only read** the same shared variable, and **neither thread writes** to it.

**Will a race condition occur? Why or why not?**

Think about it before moving to **Critical Section**, because the answer explains _why_ the operating system cares specifically about protecting **writes**, not just reads.