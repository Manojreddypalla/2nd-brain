# 🎯 GATE Corner — Test-and-Set (TSL)

> [!tip] What GATE Expects
> GATE rarely asks you to write the Test-and-Set algorithm. Instead, it tests your understanding of **atomicity**, **busy waiting**, **mutual exclusion**, and the **return value** of the instruction.

---

## ⭐ Frequently Tested Concepts

- Test-and-Set is a **hardware synchronization instruction**.
- It is **atomic** (cannot be interrupted).
- It **returns the old value** of the lock.
- It **sets the lock to 1**.
- It is used to implement **spin locks**.
- Waiting processes perform **busy waiting**.
- It guarantees **Mutual Exclusion**.
- It **does NOT** guarantee fairness.
- **Starvation is possible**.

---

## ⭐ GATE Trap #1

> Test-and-Set returns the **new** value of the lock.

❌ False

It returns the **old** value.

Example

```text
Initially

lock = 0

↓

old = TestAndSet(lock)

↓

old = 0
lock = 1
```

---

## ⭐ GATE Trap #2

> Test-and-Set removes Busy Waiting.

❌ False

Processes continuously execute

```c
while(TestAndSet(lock));
```

This is **Busy Waiting (Spin Waiting).**

---

## ⭐ GATE Trap #3

> Test-and-Set guarantees fairness.

❌ False

A process may repeatedly lose the race.

Therefore,

- Fairness ❌
- Starvation Possible ✅

---

## ⭐ GATE Trap #4

> Test-and-Set releases the lock automatically.

❌ False

The process itself must execute

```c
lock = false;
```

after leaving the Critical Section.

---

## ⭐ GATE Trap #5

> Two processes can receive `0` simultaneously.

❌ Impossible.

Since Test-and-Set is **atomic**, only one process can observe the lock as free.

Exactly one process gets

```text
old = 0
```

All others receive

```text
old = 1
```

until the lock is released.

---

# Important MCQ Facts

| Property | Test-and-Set |
|----------|--------------|
| Hardware Instruction | ✅ |
| Atomic | ✅ |
| Returns Old Value | ✅ |
| Sets Lock = 1 | ✅ |
| Mutual Exclusion | ✅ |
| Busy Waiting | ✅ |
| Spin Lock | ✅ |
| Fairness | ❌ |
| Starvation | Possible |
| Deadlock Free | ❌ Not Guaranteed |

---

# One-Liners for Revision

> Test-and-Set = **Read + Set + Return** atomically.

> `old = 0` → Lock acquired.

> `old = 1` → Keep spinning.

> Busy Waiting = CPU continuously checks the lock.

> Test-and-Set is a **hardware solution**, not a software algorithm.

---

# GATE Previous-Year Pattern

GATE commonly asks about:

- ✅ Atomic operations
- ✅ Busy Waiting vs Blocking
- ✅ Mutual Exclusion
- ✅ Starvation
- ✅ Trace-based questions (which process enters?)
- ✅ MSQs on synchronization properties

---

# 30-Second Revision

```text
Race Condition
      ↓
Need Atomic Operation
      ↓
Test-and-Set
      ↓
Returns Old Value
      ↓
old = 0 → Enter CS
old = 1 → Wait
      ↓
Busy Waiting (Spin Lock)
      ↓
Process exits
      ↓
lock = 0
```

---

# Interview Question

**Q:** Why is Test-and-Set preferred over:

```c
if(lock == 0)
    lock = 1;
```

**Answer:**

Because the above code executes as multiple CPU instructions (read, compare, write) and can be interrupted, leading to race conditions. Test-and-Set performs these operations **atomically**, ensuring only one process acquires the lock at a time.

---

# Memory Trick 🧠

**TSL = Test → Set → Lock**

- **Test** the current lock value.
- **Set** the lock to `1`.
- Return the **old** value.

If the old value is:

- **0** → You got the lock.
- **1** → Someone already has it.