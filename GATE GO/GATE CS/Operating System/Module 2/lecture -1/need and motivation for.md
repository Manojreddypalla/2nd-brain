# OS — Synchronization & Critical Section

## 1. Synchronization

### Why Synchronization?

When multiple processes/threads access or modify **shared memory/data concurrently**, the execution must be controlled.

Otherwise, the final result may become incorrect or inconsistent.

```text
Multiple Processes / Threads
            ↓
     Shared Memory/Data
            ↓
   Concurrent Access
            ↓
   Synchronization Needed
```

Synchronization is an important Operating Systems topic and is frequently tested in GATE.

---

# 2. Synchronization Problem

Consider several processes trying to **read/write shared memory**.

```text
P1 ───┐
P2 ───┼──→ Shared Data
P3 ───┘
```

The problem occurs because processes may interleave their operations in different ways.

The same shared data may therefore produce different results depending on execution order.

---

# 3. Chef and Bread Problem

The lecturer uses the **Chef and Bread** example to explain synchronization.

### Situation

- There is a bread packet in a store/kitchen.
    
- A shared list indicates whether bread is available.
    
- Two chefs want the bread.
    

A chef performs:

```text
1. Check whether bread is available.
2. Collect the bread.
3. Return and update the list.
```

### Problem

Suppose:

```text
List → Bread Available
```

Chef 1 checks the list.

Before Chef 1 updates the list, Chef 2 also checks it.

So both see:

```text
Bread Available
```

Then both try to collect the bread.

But only one bread packet actually exists.

### Result

The shared information becomes inconsistent with reality.

This is the basic idea behind a **race condition**.

---

# 4. Too Much Milk Problem

Another analogy used is the **Too Much Milk Problem**.

Suppose two roommates share a refrigerator.

Both check:

```text
No milk
```

So:

```text
Person 1 → goes to buy milk
Person 2 → goes to buy milk
```

Both return with milk.

### Result

Instead of buying one milk packet:

```text
Required = 1
Actual = 2
```

The problem occurs because both made decisions using the same shared state without coordination.

This demonstrates how concurrent actions can cause an unexpected result.

---

# 5. Shared Variable Example

Suppose:

```c
x = 50;
```

Two threads execute:

```c
x = x + 1;
```

Expected result:

```text
50 → 51 → 52
```

So we may expect:

```text
x = 52
```

But this is not guaranteed when both threads execute concurrently.

---

# 6. Why `x = x + 1` Is Dangerous

The statement:

```c
x = x + 1;
```

looks like a single operation in C.

Internally, it can be broken into operations such as:

```text
LOAD x
ADD 1
STORE x
```

For example:

```text
LOAD R1, 1000
ADD  R1, 1
STORE R1, 1000
```

where the shared variable `x` is stored at some memory location.

### Important Point

A single line of source code does **not necessarily mean one atomic machine operation**.

Two threads can interleave these low-level operations.

---

# 7. Example of Incorrect Interleaving

Initially:

```text
x = 50
```

### Thread 1

```text
LOAD x
R1 = 50
```

### Thread 2

```text
LOAD x
R1 = 50
```

Now both threads have their own local copy:

```text
T1 → 50
T2 → 50
```

Then:

```text
T1 → 50 + 1 = 51
T2 → 50 + 1 = 51
```

Both may store:

```text
x = 51
```

### Final result

```text
Expected = 52
Actual   = 51
```

One increment has effectively been lost.

This is a **lost update**.

The PDF uses this example to motivate the need for synchronization.

---

# 8. Race Condition

## Definition

A **Race Condition** occurs when:

- multiple processes access/manipulate the same data concurrently, and
    
- the final result depends on the **order in which the accesses occur**.
    

```text
Same Shared Data
       +
Concurrent Access
       +
Execution Order Matters
       ↓
   Race Condition
```

### Key phrase

> **Outcome depends on execution order.**

---

# 9. Naive Solution

A simple solution is:

> Force sequential execution.

Instead of:

```text
T1 ──────┐
         ├── Concurrent
T2 ──────┘
```

force:

```text
T1 completes
      ↓
T2 starts
```

This avoids race conditions because the two processes cannot interfere with each other.

### Problem

Sequential execution reduces the advantage of concurrency and may cause **CPU underutilization**.

```text
Concurrency removed
       ↓
Less CPU utilization
```

So this is not a good general solution.

---

# 10. Critical-Section Problem

The basic requirement is:

> When one process is executing its critical section, no other process should execute its critical section at the same time.

This is the **Critical-Section Problem**.

Example:

```c
x = x + 1;
```

If multiple processes execute this shared-data operation, it must be protected.

---

# 11. Critical Section

A **Critical Section (CS)** is a code segment that:

- accesses shared variables/data/resources
    
- must be executed as an atomic operation
    

The main goal is to ensure that **at most one process** is executing its critical section at a given time.

### Important GATE Point

Critical sections in different threads/processes are **not necessarily the same code segment**.

They are related by the shared resource/synchronization requirement.

---

# 12. Basic Critical-Section Example

Suppose:

```c
P1:
    x = x + 1;

P2:
    x = x + 1;
```

Both access shared variable `x`.

Therefore:

```text
x = x + 1
```

is the critical section that requires synchronization.

---

# 13. Mutual Exclusion

The first requirement of a critical-section solution is **Mutual Exclusion (ME)**.

### Definition

At most **one process** can be inside its critical section at any given time.

```text
ME ≤ 1
```

Allowed:

```text
0 processes in CS
1 process in CS
```

Not allowed:

```text
2 or more processes in CS simultaneously
```

---

# 14. Why Mutual Exclusion Alone Is Not Enough

Imagine an algorithm where:

```text
No process ever enters the critical section.
```

Then clearly:

```text
At most one process is inside
```

So mutual exclusion is satisfied.

But the system is useless because nobody makes progress.

Therefore, a correct solution requires more than ME.

---

# 15. Progress

### Definition

If:

- no process is currently inside the critical section, and
    
- one or more processes want to enter,
    

then the system should allow some process to enter.

```text
CS empty
   +
Processes waiting
   ↓
At least one should enter
```

The idea is to avoid unnecessary indefinite blocking.

---

# 16. Bounded Waiting

The third requirement is **Bounded Waiting (BW)**.

### Problem

Suppose:

```text
P1 enters CS
P2 waits
```

P1 leaves.

A bad algorithm might allow:

```text
P1 enters again
P1 leaves
P1 enters again
P1 leaves
...
```

while P2 keeps waiting.

This is unfair.

### Bounded Waiting

A process that is waiting to enter the critical section should not be postponed forever.

There must be a bound on how many times other processes can enter before the waiting process gets its chance.

---

# 17. Three Requirements

A correct critical-section solution must satisfy:

```text
1. Mutual Exclusion
2. Progress
3. Bounded Waiting
```

### Memory Trick

```text
ME       → Safety
Progress → Someone gets a chance
BW       → Waiting must be fair
```

---

# 18. Mutual Exclusion vs Progress

### Mutual Exclusion

```text
At most one
```

### Progress

```text
At least one
```

These are different requirements.

### Example

If nobody enters:

```text
ME ✓
Progress ✗
```

If two processes enter:

```text
ME ✗
```

A correct algorithm must satisfy both.

---

# 19. Bounded Waiting vs Fairness

Bounded waiting is essentially the fairness requirement.

It prevents a process from continuously being denied entry while another process repeatedly gets access.

```text
Waiting Process
      ↓
Must eventually get a chance
```

---

# 20. Lobby Analogy

Think of a critical section as a room with a single entry.

```text
P1 ─┐
P2 ─┼──→ Lobby ──→ Room
P3 ─┘
```

Rules:

### Mutual Exclusion

Only one person inside the room at a time.

### Progress

If the room is empty and people are waiting, someone should enter.

### Bounded Waiting

One person should not repeatedly enter while another person waits forever.

---

# 21. Why "Exactly One" Appears

When processes are competing for the critical section:

```text
Mutual Exclusion → at most one
Progress         → at least one
```

Together, in that situation, the intended behavior is that **one process gets the opportunity to enter**.

But remember:

- ME is not "exactly one".
    
- ME means **at most one**.
    
- Progress deals with making entry possible when needed.
    

---

# 22. Complete Critical-Section Requirement

Think of the solution as:

```text
        Critical Section Solution
                   |
        ┌──────────┼──────────┐
        ↓          ↓          ↓
       ME       Progress      BW
       ≤1         ≥1        Fairness
```

All three must be satisfied.

---

# 23. Systematic Way to Check a Solution

Whenever an algorithm is given, test it in this order:

```text
1. Mutual Exclusion
2. Progress
3. Bounded Waiting
```

This is the systematic method emphasized in the lecture.

---

# 24. Mutual Exclusion Test

### Situation

One process is already inside the critical section.

Another process tries to enter.

Question:

> Can the second process also enter?

If yes:

```text
ME ✗
```

If no:

```text
ME ✓
```

---

# 25. Progress Test — Case 1

Suppose:

```text
No process is in CS
```

and:

```text
P1 wants to enter
```

Question:

> Can P1 enter?

A correct solution should not unnecessarily prevent P1.

If P1 can enter:

```text
Progress ✓
```

---

# 26. Progress Test — Case 2

Suppose:

```text
P1 and P2
```

are both trying to enter.

At least one should eventually be able to make progress.

If both can remain stuck forever:

```text
Progress ✗
```

This resembles deadlock/waiting behavior.

---

# 27. Bounded Waiting Test

Suppose:

```text
P1 inside CS
P2 waiting
```

P1 exits.

Now ask:

> Can P1 repeatedly re-enter forever while P2 waits?

If yes:

```text
BW ✗
```

If P2 is guaranteed a chance after a bounded number of entries:

```text
BW ✓
```

---

# 28. Critical-Section Structure

Every process can be viewed as:

```text
do {

    Entry Section

    Critical Section

    Exit Section

    Remainder Section

} while (true);
```

---

# 29. Entry Section

The **Entry Section** is responsible for obtaining permission to enter the critical section.

Conceptually:

```text
"Can I enter?"
```

The synchronization mechanism is used here.

---

# 30. Critical Section

The **Critical Section** contains the code that accesses the shared resource/data.

Example:

```c
x = x + 1;
```

This is the part that must be protected.

---

# 31. Exit Section

The **Exit Section** releases whatever permission/control was obtained in the entry section.

Conceptually:

```text
"I'm finished.
Others may enter."
```

---

# 32. Remainder Section

The **Remainder Section** contains the rest of the program.

It does not involve the critical shared operation.

```text
Other normal work
```

---

# 33. Entry → CS → Exit

The overall sequence is:

```text
        Entry
          ↓
   enter critical section
          ↓
      Critical
          ↓
        Exit
          ↓
   Remainder section
```

The important idea:

```text
Entry  → acquire access
CS     → use shared resource
Exit   → release access
```

---

# 34. Process A and Process B

Consider two processes:

```text
Process A
Process B
```

Suppose A enters the critical section first.

```text
A → CS
B → waiting
```

B cannot enter while A is inside.

---

# 35. Timeline of Entry

The lecture's timeline shows:

```text
T1:
A enters critical region
```

Then:

```text
T2:
B attempts to enter
```

Since A is still inside:

```text
B → blocked
```

This demonstrates mutual exclusion.

---

# 36. A Leaves the Critical Section

At:

```text
T3
```

Process A leaves the critical region.

Now B is allowed to proceed.

```text
A exits
   ↓
B can enter
```

---

# 37. B Enters

After A leaves:

```text
B → enters CS
```

Eventually:

```text
T4:
B leaves CS
```

So the sequence is:

```text
A enters
   ↓
B waits
   ↓
A leaves
   ↓
B enters
   ↓
B leaves
```

---

# 38. Door Analogy

Think of the critical section as a room with a controlled door.

```text
             ┌──────────────┐
             │  Critical    │
             │   Section    │
             └──────┬───────┘
                    🚪
```

The synchronization mechanism controls this door.

Only one process should be allowed inside according to the synchronization rules.

---

# 39. Critical-Section Solution

We now know:

```text
Problem:
Race Condition

Goal:
Protect Critical Section

Requirements:
ME + Progress + BW
```

The next step is to design an actual algorithm that implements:

```text
Enter CS
Leave CS
```

---

# 40. Example

Suppose:

```text
Thread 1:
    Enter CS
    count++
    Leave CS
```

and:

```text
Thread 2:
    Enter CS
    count++
    Leave CS
```

Here:

```text
count++
```

is the shared operation.

The important question becomes:

> How do we implement `Enter CS` and `Leave CS`?

---

# 41. GATE Core Summary

## Race Condition

```text
Multiple processes
        +
Shared data
        +
Concurrent access
        +
Execution order affects result
        ↓
Race Condition
```

---

## Critical Section

```text
Code accessing shared data
        ↓
Must be controlled
        ↓
Critical Section
```

---

## Three Requirements

|Requirement|Meaning|
|---|---|
|**Mutual Exclusion**|At most one process in CS|
|**Progress**|If CS is free and processes want entry, some process should enter|
|**Bounded Waiting**|A waiting process cannot be postponed forever|

---

## Critical-Section Structure

```text
Entry Section
      ↓
Critical Section
      ↓
Exit Section
      ↓
Remainder Section
```

---

# 42. One-Line Memory Map

```text
Synchronization
      ↓
Race Condition
      ↓
Critical Section
      ↓
Critical-Section Problem
      ↓
ME + Progress + BW
      ↓
Entry Section
      ↓
Critical Section
      ↓
Exit Section
      ↓
Now design synchronization algorithms
```

---

# 43. Most Important GATE Traps

### Trap 1

**"Mutual exclusion means exactly one process is inside."**

Wrong.

```text
ME = At most one
```

So zero processes inside is also valid.

---

### Trap 2

**"If ME is satisfied, the solution is correct."**

Wrong.

You must also check:

```text
Progress
Bounded Waiting
```

---

### Trap 3

**"One line of C code is atomic."**

Wrong.

For example:

```c
x = x + 1;
```

may involve:

```text
LOAD
ADD
STORE
```

---

### Trap 4

**"Race condition means processes must execute simultaneously."**

Not exactly.

The important point is that the result depends on the **interleaving/order of execution**.

---

### Trap 5

**"Critical sections in different processes must contain identical code."**

Wrong.

They do not necessarily have to be the same code segment; what matters is the synchronization relationship involving shared resources.

---

# 44. Final Mental Model

Imagine a **single-room office**:

```text
People waiting
     ↓
   Lobby
     ↓
Single room
```

### Mutual Exclusion

Only one person inside.

### Progress

If room is empty and people are waiting, somebody gets in.

### Bounded Waiting

The same person cannot keep entering repeatedly while another waits forever.

That is essentially the whole **Critical-Section Problem**.