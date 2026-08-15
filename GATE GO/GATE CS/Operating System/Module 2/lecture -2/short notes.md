# OS — Lecture 2: Synchronization & Critical Section

## 1. Implementing Mutual Exclusion

### Goal

A process follows:

```text
Entry Section
      ↓
Critical Section
      ↓
Exit Section
```

The objective is to ensure that **only one process executes its Critical Section at a time**.

### Ways to implement synchronization

1. Write synchronization code ourselves → **Software solution**
    
2. Ask the OS for help → **System calls**
    
3. Use hardware support → **Hardware synchronization**
    

The lecture first attempts to build synchronization using **shared variables**.

---

# 2. Critical Section

A **Critical Section (CS)** is the part of a process that accesses shared data/resources and must be executed safely.

### Main requirement

> At most one process should execute its Critical Section at a time.

The general structure is:

```text
do {
    Entry Section

    Critical Section

    Exit Section

    Remainder Section
} while(true);
```

Critical sections in different threads/processes **do not necessarily have to be the same code segment**.

---

# 3. Three Requirements of a Synchronization Solution

Every synchronization algorithm must be checked for:

### 1. Mutual Exclusion

> At most one process can be inside the Critical Section at a time.

### 2. Progress

> If the CS is free and some process wants to enter, the decision about who enters next should not be postponed indefinitely.

### 3. Bounded Waiting

> A process waiting to enter must not be bypassed indefinitely.

These are the three standard checks used throughout the lecture.

---

# 4. Mutual Exclusion — How to Test

### Case 1

One process is already in CS and another tries to enter.

→ The second process must be blocked.

### Case 2

Two or more processes are in the Entry Section.

→ At most one should enter the CS.

---

# 5. Progress

Progress is checked mainly in two situations.

### Case 1

No process is in CS and one process wants to enter.

→ That process should be able to proceed.

### Case 2

Two or more processes are in Entry Section.

→ At least one should be able to enter.

### Key idea

A process that **doesn't even want the CS** should not unnecessarily prevent another interested process from entering.

---

# 6. Bounded Waiting

Suppose:

```text
P1 → inside CS
P2 → waiting
```

After P1 exits, P1 should not repeatedly re-enter while P2 waits forever.

Therefore, there must be a bound on how many times other processes can enter before P2 gets its chance.

### Meaning

```text
Waiting is allowed
        ↓
Infinite bypassing is not allowed
```

Bounded waiting is therefore associated with **fairness**.

---

# 7. Attempt 1 — `interested`

Use one shared variable:

```c
int interested = 0;
```

Meaning:

```text
interested = 0 → nobody is interested
interested = 1 → someone is interested
```

Basic idea:

```c
while (interested);

interested = 1;

CS
```

Meaning:

> Wait until nobody is interested, mark yourself as interested, then enter CS.

---

# 8. Why `interested` Fails

Initially:

```text
interested = 0
```

Possible execution:

```text
P1 checks interested → 0
P2 checks interested → 0

P1 sets interested = 1
P2 sets interested = 1

P1 enters CS
P2 enters CS
```

Both processes passed the check before either one effectively blocked the other.

### Core problem

**Check and update are separate operations.**

```text
CHECK
  ↓
[another process can execute]
  ↓
UPDATE
```

So both can observe `0`.

### Result

```text
Mutual Exclusion ❌
Progress         ✅
Bounded Waiting  ❌
```

### Lesson

> A shared variable alone does not make the check-and-update operation atomic.

---

# 9. Attempt 2 — `turn`

Use:

```c
int turn = 1;
```

Meaning:

```text
turn = 0 → P0 gets the turn
turn = 1 → P1 gets the turn
```

### Process 0

```c
while (turn != 0);

CS

turn = 1;
```

### Process 1

```c
while (turn != 1);

CS

turn = 0;
```

The processes alternate:

```text
P0 → P1 → P0 → P1 → ...
```

This is called **Strict Alternation**.

---

# 10. Why Strict Alternation Satisfies Mutual Exclusion

Suppose:

```text
turn = 0
```

Only P0 can pass:

```c
while (turn != 0);
```

P1 must wait.

After P0 finishes:

```c
turn = 1;
```

Now P1 can enter.

Therefore:

```text
Mutual Exclusion ✅
```

Only the process whose turn it is can enter.

---

# 11. Why Strict Alternation Fails Progress

Suppose:

```text
turn = 1
```

Now:

```text
P0 wants CS
P1 does NOT want CS
```

P0 still executes:

```c
while (turn != 0);
```

Since `turn = 1`, P0 waits.

But P1 isn't even interested in entering.

Therefore:

```text
P1 → does not need CS
P0 → needs CS
P0 → still waits
```

### Result

```text
Mutual Exclusion ✅
Progress         ❌
Bounded Waiting  ✅
```

### Key idea

> Strict Alternation gives priority based on **whose turn it is**, not on **who actually wants to enter**.

---

# 12. Strict Alternation — Important

### Why Bounded Waiting is satisfied

Because the processes alternate:

```text
P0 → P1 → P0 → P1 → ...
```

One process cannot continuously re-enter while the other waits forever.

Therefore:

```text
Bounded Waiting ✅
```

---

# 13. Important Misconception — Progress ≠ No Deadlock

The lecture explicitly distinguishes:

```text
No Deadlock
```

from

```text
Progress
```

Progress specifically asks whether a process that wants to enter can actually make progress when the CS is available.

So:

> **No deadlock does not automatically mean Progress is satisfied.**

---

# 14. Attempt 3 — `want[]`

Now use a separate flag for each process:

```c
int want[2] = {false, false};
```

Meaning:

```text
want[0] = true → P0 wants to enter
want[1] = true → P1 wants to enter
```

### Process 0

```c
want[0] = true;

while (want[1]);

CS

want[0] = false;
```

### Process 1

```c
want[1] = true;

while (want[0]);

CS

want[1] = false;
```

The idea is:

> Each process announces its interest before trying to enter.

---

# 15. Why `want[]` Satisfies Mutual Exclusion

Suppose:

```text
want[0] = true
want[1] = false
```

Then P0 can enter.

If both want to enter:

```text
want[0] = true
want[1] = true
```

then:

```text
P0 waits for want[1] → false
P1 waits for want[0] → false
```

Therefore both cannot enter simultaneously.

```text
Mutual Exclusion ✅
```

---

# 16. Problem with `want[]` — Deadlock

Suppose both processes become interested simultaneously:

```text
want[0] = true
want[1] = true
```

Then:

```text
P0 → while(want[1])
P1 → while(want[0])
```

Both conditions are true.

So:

```text
P0 waits for P1
P1 waits for P0
```

Neither can proceed.

```text
P0 ─────wait────→ P1
↑                 │
└──────wait───────┘
```

This is **circular waiting → deadlock**.

### Result

```text
Mutual Exclusion ✅
Progress         ❌
Bounded Waiting  ✅
```

---

# 17. Common Deadlock Pattern

Whenever you see:

```text
P0 waits for P1
P1 waits for P0
```

Think:

```text
Circular Waiting
      ↓
   Deadlock
```

This is exactly what happens when both `want[]` flags become true.

---

# 18. Comparison of the First Attempts

|Solution|Mutual Exclusion|Progress|Bounded Waiting|Main Problem|
|---|---|---|---|---|
|`interested`|❌|✅|❌|Check + update not atomic|
|`turn` / Strict Alternation|✅|❌|✅|Uninterested process can block|
|`want[]`|✅|❌|✅|Both can wait → deadlock|

---

# 19. What Each Approach Is Missing

### `interested`

Missing:

```text
Atomic coordination
```

Two processes can observe the same state before either updates it.

### Strict Alternation

Missing:

```text
Independence from the other process's interest
```

An uninterested process can unnecessarily control the turn.

### `want[]`

Missing:

```text
Tie-breaking mechanism
```

Both can express interest and then wait for each other.

---

# 20. Combining the Ideas

We need both:

```text
want[] → "I want to enter."
```

and:

```text
turn → "If both want to enter, who gets priority?"
```

So the required idea is:

```text
Interest
   +
Tie Breaking
```

This leads to **Peterson's Solution**.

---

# 21. Formal Definition — Mutual Exclusion

If process `Pi` is executing in its CS:

> No other process can be executing in its CS at the same time.

In short:

```text
At most one process
inside CS
```

---

# 22. Formal Definition — Progress

If no process is currently in CS and some processes want to enter:

- only processes that are actually participating in the decision should influence who enters;
    
- the decision cannot be postponed indefinitely.
    

### Core idea

> A process that does not want the CS should not unnecessarily block processes that do want it.

---

# 23. Formal Definition — Bounded Waiting

There must be a bound on the number of times other processes can enter their CS after a process requests entry and before that request is granted.

Simple view:

```text
Request CS
   ↓
Other processes may enter
   ↓
But only a bounded number of times
   ↓
You get your chance
```

---

# 24. Progress vs Bounded Waiting

### Progress asks:

> Can someone make progress when the CS is available?

### Bounded Waiting asks:

> Can one particular waiting process be bypassed forever?

So:

```text
Progress
→ system should not be unnecessarily stuck

Bounded Waiting
→ a waiting process should not be postponed forever
```

---

# 25. GATE Analysis Pattern

Whenever GATE gives a synchronization algorithm, check in this order:

### Step 1 — Mutual Exclusion

Try to make **both processes enter CS**.

If possible:

```text
ME ❌
```

### Step 2 — Progress

Make:

```text
Only one process wants to enter
```

If it can still be blocked unnecessarily:

```text
Progress ❌
```

### Step 3 — Bounded Waiting

Make:

```text
P1 waiting
P2 repeatedly enters
```

If P2 can bypass P1 forever:

```text
BW ❌
```

---

# 26. Useful Counterexamples

### To disprove Mutual Exclusion

Construct:

```text
P1 → CS
P2 → CS
```

at the same time.

### To disprove Progress

Construct:

```text
P1 → wants CS
P2 → does NOT want CS
```

and show P2 can still block P1.

### To disprove Bounded Waiting

Construct:

```text
P1 → waiting
P2 → enters
P2 → exits
P2 → enters again
P2 → exits
...
```

while P1 never gets its chance.

---

# 27. Deadlock vs Bounded-Waiting Failure

### Deadlock

Processes wait for each other forever.

```text
P0 waits for P1
P1 waits for P0
```

### Bounded-Waiting Failure

One process keeps getting bypassed while another keeps entering.

```text
P1 waiting
P2 keeps getting priority
```

The lecture uses bounded waiting as the **fairness** condition.

---

# 28. Strict Alternation vs `want[]`

|Feature|Strict Alternation|`want[]`|
|---|---|---|
|Mutual Exclusion|✅|✅|
|Progress|❌|❌|
|Bounded Waiting|✅|✅|
|Main Problem|Uninterested process can block|Both can wait forever|

---

# 29. Key Insight Before Peterson's Solution

We need:

```text
want[i]
```

to express:

> "I want to enter."

And:

```text
turn
```

to decide:

> "If both want to enter, who gets priority?"

Therefore:

```text
Interest + Tie Breaking
        ↓
Peterson's Solution
```

---

# 30. Final Revision Map

```text
Implement Mutual Exclusion
        ↓
Attempt 1 — interested
        ↓
Fails Mutual Exclusion
        ↓
Attempt 2 — turn
        ↓
Strict Alternation
        ↓
ME ✅
Progress ❌
BW ✅
        ↓
Attempt 3 — want[]
        ↓
ME ✅
Progress ❌
BW ✅
        ↓
Both can wait
        ↓
Deadlock
        ↓
Need better solution
        ↓
ME / Progress / BW
        ↓
Interest + Turn
        ↓
Peterson's Solution
```

---

# GATE Must Remember

### Mutual Exclusion

```text
At most ONE process in CS
```

### Progress

```text
CS is free + someone wants it
→ someone should be able to proceed
```

### Bounded Waiting

```text
A waiting process cannot be bypassed forever
```

### Strict Alternation

```text
ME ✅
Progress ❌
BW ✅
```

### `want[]`

```text
ME ✅
Progress ❌
BW ✅
```

### Fast pattern recognition

```text
Only one process wants
→ Check Progress

Both want
→ Check ME / Deadlock

One waits while another repeatedly enters
→ Check Bounded Waiting
```