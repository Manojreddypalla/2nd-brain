# OS — Synchronization & Critical Section

## Pages 45–95

---

# 1. Implementing Mutual Exclusion

The goal is to implement:

```text
Enter Critical Section
        ↓
Execute CS
        ↓
Leave Critical Section
```

Possible ways to implement synchronization:

1. Write the synchronization code ourselves.
    
2. Ask the OS for help using system calls.
    
3. Use hardware support.
    

The lecture first tries to build a solution using software/shared variables.

---

# 2. First Attempt — `interested`

Use a shared variable:

```c
int interested = 0;
```

Idea:

```text
interested = 0 → nobody is interested
interested = 1 → someone is interested
```

A process can do:

```c
while (interested);

interested = 1;

Critical Section
```

The idea is:

> Wait until nobody is interested, then mark yourself as interested and enter CS.

---

# 3. Why This First Attempt Fails

Suppose:

```text
interested = 0
```

Both processes see:

```text
while(interested)
```

and both observe:

```text
interested == 0
```

Therefore both can move forward.

Possible execution:

```text
P1 checks interested → 0
P2 checks interested → 0

P1 sets interested = 1
P2 sets interested = 1

P1 enters CS
P2 enters CS
```

Therefore:

```text
Mutual Exclusion ✗
```

The problem is the **check and update are separate operations**.

Both processes can pass the check before either one changes the variable.

---

# 4. Attempt 1 — Evaluation

For this approach:

```text
Mutual Exclusion → ✗
Progress           → ✓
Bounded Waiting    → ✗
```

The important lesson:

> Simply having a shared "interested" variable does not automatically make the entry operation atomic.

---

# 5. Systematic Checking

For every synchronization algorithm, check:

```text
1. Mutual Exclusion
2. Progress
3. Bounded Waiting
```

This same three-step test is repeatedly used throughout the lecture.

---

# 6. Three Requirements — Exact Situations

## Mutual Exclusion

### Case 1

One process is already in CS and another wants to enter.

The second process must be blocked.

### Case 2

Two or more processes are in the entry section.

At most one should enter the CS.

---

# 7. Progress

Progress is considered in two cases.

### Case 1

No process is in the CS and process `P1` arrives.

Then:

```text
P1 should be able to enter
```

### Case 2

Two or more processes are in the entry section.

At least one should be able to enter the CS.

The purpose is to avoid indefinite blocking when there is no reason to block.

---

# 8. Bounded Waiting

Suppose:

```text
P1 → inside CS
P2 → waiting
```

P1 exits.

The solution must ensure that P1 cannot repeatedly re-enter while P2 continues waiting forever.

So there must be a bound on how many times other processes can enter before P2 gets its chance.

This is why bounded waiting is also associated with **fairness**.

---

# 9. Second Attempt — Strict Alternation

Now use a shared variable:

```c
int turn = 1;
```

Idea:

```text
turn = 0 → Process 0 gets the turn
turn = 1 → Process 1 gets the turn
```

### Process 0

```c
while (turn != 0);

Critical Section

turn = 1;
```

### Process 1

```c
while (turn != 1);

Critical Section

turn = 0;
```

---

# 10. Why Strict Alternation Works for ME

Suppose:

```text
turn = 0
```

Then only Process 0 can pass:

```c
while (turn != 0);
```

Process 1 is forced to wait because:

```c
turn != 1
```

is true.

After Process 0 finishes:

```c
turn = 1;
```

Now Process 1 can enter.

Therefore:

```text
Mutual Exclusion ✓
```

Only the process whose turn it is can enter.

---

# 11. Strict Alternation and Progress

The problem is that the process outside the CS can still control the turn.

Example:

```text
turn = 1
```

Suppose Process 0 wants to enter.

But Process 1:

```text
does not want to enter
```

Process 0 still waits because:

```c
while (turn != 0);
```

The turn belongs to Process 1.

So:

```text
Process 1 doesn't need CS
Process 0 needs CS
Process 0 still waits
```

Therefore:

```text
Progress ✗
```

---

# 12. Strict Alternation

This approach is called:

## Strict Alternation

The processes are forced to enter the CS in an alternating order:

```text
P0 → P1 → P0 → P1 → ...
```

It satisfies:

```text
Mutual Exclusion ✓
Bounded Waiting ✓
Progress ✗
```

The lecture specifically identifies the progress failure.

---

# 13. Why Progress Fails

The key mistake is:

> **The algorithm gives priority based on whose turn it is, rather than based on who actually wants to enter.**

Example:

```text
turn = 1

P0 wants CS
P1 does not want CS
```

Still:

```text
P0 waits
```

That violates progress.

---

# 14. Important Misconception

### Progress does NOT simply mean:

> "There is no deadlock."

The lecture explicitly points out that:

```text
No deadlock
```

and

```text
Progress
```

are not exactly the same idea.

Progress is specifically about whether processes that want to enter can actually make progress when the CS is available.

---

# 15. Third Attempt — `want[]`

Now use an array:

```c
int want[2] = {false, false};
```

Meaning:

```text
want[0] = true
→ P0 wants to enter

want[1] = true
→ P1 wants to enter
```

### Process 0

```c
want[0] = true;

while (want[1]);

Critical Section

want[0] = false;
```

### Process 1

```c
want[1] = true;

while (want[0]);

Critical Section

want[1] = false;
```

---

# 16. Why `want[]` Satisfies Mutual Exclusion

Suppose:

```text
want[0] = true
want[1] = false
```

Then Process 0 enters.

If both want to enter:

```text
want[0] = true
want[1] = true
```

then:

```text
P0 waits for want[1] to become false
P1 waits for want[0] to become false
```

This prevents both from entering the CS together.

Therefore:

```text
Mutual Exclusion ✓
```

---

# 17. Problem with `want[]`

Suppose both processes simultaneously do:

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

Therefore:

```text
P0 waits
P1 waits
```

Nobody can enter.

```text
DEADLOCK
```

So:

```text
Mutual Exclusion ✓
Progress ✗
```

---

# 18. Important Deadlock Pattern

Whenever you see:

```text
P0 waits for P1
P1 waits for P0
```

think:

```text
Circular Waiting
      ↓
Deadlock
```

This is exactly what happens with the simple `want[]` solution when both processes raise their flags simultaneously.

---

# 19. Common Mistake

A common mistake is to look only at:

```text
"Can both enter CS?"
```

and conclude the algorithm is correct.

But synchronization requires checking all three:

```text
ME
Progress
BW
```

For the `want[]` attempt:

```text
ME ✓
Progress ✗
BW ✓
```

because it can deadlock.

---

# 20. Why the `want[]` Approach Is Not Enough

We now have two problems from previous attempts:

### Strict Alternation

```text
ME ✓
Progress ✗
BW ✓
```

Problem:

```text
Someone who doesn't want CS
can unnecessarily block someone who does.
```

### `want[]`

```text
ME ✓
Progress ✗
BW ✓
```

Problem:

```text
Both can declare interest
and then wait forever for each other.
```

So we need something more intelligent.

---

# 21. The Need for Combining Ideas

We want:

```text
want[]  → expresses interest
turn    → resolves competition
```

The eventual idea is to combine:

```text
"I want to enter"
+
"If both want it, who gets priority?"
```

This leads toward **Peterson's Solution**.

---

# 22. Formal Definitions — Galvin

The lecture now introduces textbook definitions for:

```text
Mutual Exclusion
Progress
Bounded Waiting
```

These definitions are from Galvin's Operating System material.

---

# 23. Mutual Exclusion — Formal

If process `Pi` is executing in its critical section, then:

> No other process can be executing in its critical section at the same time.

In short:

```text
At most one process
inside CS
```

---

# 24. Progress — Formal

If no process is currently executing in its critical section and some processes want to enter, then:

- only processes that are **not executing in their remainder sections** can participate in deciding who enters next.
    
- the decision cannot be postponed indefinitely.
    

### Key idea

A process that doesn't even want the critical section should not be able to unnecessarily block the processes that do want it.

---

# 25. Bounded Waiting — Formal

There must be a bound on the number of times other processes are allowed to enter their critical sections after a process has requested entry and before that request is granted.

In simpler terms:

```text
You request CS
      ↓
Other processes may enter
      ↓
But only a bounded number of times
      ↓
Then you get your chance
```

---

# 26. Strict Alternation — Revisited Formally

Strict alternation guarantees:

```text
ME ✓
BW ✓
```

But fails:

```text
Progress ✗
```

because the `turn` variable can force a process to wait even when the other process is not interested.

---

# 27. GATE Question Pattern — ME vs Progress

The lecture includes a GATE question where two processes use:

```c
while (S1 == S2);

Critical Section

S1 = S2;
```

and:

```c
while (S1 != S2);

Critical Section

S2 = !S1;
```

The important lesson from the solution is:

```text
Mutual Exclusion ✓
Progress ✗
```

because one process that isn't interested can still prevent the other from entering.

### GATE Pattern

When analyzing a synchronization algorithm:

> Ask what happens when **only one process wants to enter**.

That is often where progress fails.

---

# 28. GATE Question — `wants1`, `wants2`

Another GATE question uses:

```c
wants1
wants2
```

Each process sets its own variable to `true` and waits for the other's variable to become `false`.

The result:

```text
ME ✓
Progress ✗
BW ✓
```

because both can set their variables to `true` and wait forever.

---

# 29. Strict Alternation GATE Question

Another GATE question uses:

```c
turn
```

with:

```text
Process 0 waits while turn == 1
Process 1 waits while turn == 0
```

and then changes the turn after leaving CS.

The result:

```text
ME ✓
BW ✓
Progress ✗
```

This is **strict alternation**.

---

# 30. Why Strict Alternation Satisfies BW

There are only two processes.

After:

```text
P0 enters
```

the next opportunity is reserved for:

```text
P1
```

Similarly:

```text
P1 enters
```

then:

```text
P0
```

So one process cannot repeatedly enter while the other waits forever.

Therefore:

```text
Bounded Waiting ✓
```

---

# 31. Why Strict Alternation Violates Progress

Suppose:

```text
turn = 0
```

P0 finishes and changes:

```text
turn = 1
```

Now P0 wants to enter again.

But P1 is not interested.

P0 still has to wait until P1 changes the turn.

Therefore a process that doesn't want the CS can stop another interested process.

Hence:

```text
Progress ✗
```

The lecture calls this strict alternation.

---

# 32. Progress — Layman Meaning

The lecture gives a very useful plain-language interpretation.

The purpose of progress is to ensure that:

```text
Either:
    a process is already in the CS and doing work

OR

    at least one process wants to enter
    and can enter to do work
```

In both cases:

```text
Actual work is being done.
```

Therefore the system as a whole keeps progressing.

---

# 33. Progress — Simple Mental Model

Think:

```text
CS occupied?
    YES → work is happening

    NO
    ↓
Does someone want CS?
    YES → someone should be allowed to proceed
    NO  → nobody needs to enter
```

The system should not get stuck doing nothing while work is waiting to happen.

---

# 34. Bounded Waiting — Layman Meaning

The lecture gives the simple interpretation:

> A process should not be bypassed by another process.

In simple terms:

```text
P wants to enter
     ↓
P' cannot repeatedly jump ahead forever
```

---

# 35. Important BW Clarification

Bounded waiting does **not** necessarily say:

> "The process must definitely enter immediately."

It says:

> The process should not be **bypassed indefinitely**.

So:

```text
Waiting ≠ immediate entry
```

Instead:

```text
Waiting → bounded number of bypasses
```

The PDF explicitly emphasizes this distinction.

---

# 36. Progress vs Bounded Waiting

### Progress asks:

> "Can someone make progress when the CS is available?"

### Bounded Waiting asks:

> "Can one waiting process be bypassed forever?"

So:

```text
Progress
→ system should not get unnecessarily stuck

Bounded Waiting
→ a particular waiting process should not be unfairly postponed
```

---

# 37. Important Comparison

|Property|Main Question|
|---|---|
|**Mutual Exclusion**|Can two processes enter CS together?|
|**Progress**|If CS is free and someone wants it, can someone proceed?|
|**Bounded Waiting**|Can one process be bypassed forever?|

---

# 38. First Attempts Summary

## Attempt 1 — Simple `interested`

```text
interested = 0/1
```

Problem:

```text
Two processes can pass the check
before either updates the variable.
```

Result:

```text
ME ✗
```

---

## Attempt 2 — `turn`

```text
turn = 0/1
```

Called:

```text
Strict Alternation
```

Result:

```text
ME ✓
Progress ✗
BW ✓
```

---

## Attempt 3 — `want[]`

```text
want[0]
want[1]
```

Result:

```text
ME ✓
Progress ✗
BW ✓
```

Problem:

```text
Both can wait for each other
→ Deadlock
```

---

# 39. What the Algorithms Are Missing

### Simple `interested`

Missing:

```text
Atomic coordination
```

### Strict Alternation

Missing:

```text
Independence from the other process's interest
```

### `want[]`

Missing:

```text
A tie-breaking mechanism
```

This motivates combining:

```text
Interest
+
Turn
```

which is the idea behind the next solution.

---

# 40. Exam Approach

Whenever GATE gives a synchronization algorithm:

## Step 1 — Check ME

Try to make both processes enter CS.

If possible:

```text
ME ✗
```

## Step 2 — Check Progress

Make:

```text
Only P1 wants to enter
```

If P1 can still be blocked unnecessarily:

```text
Progress ✗
```

## Step 3 — Check BW

Make:

```text
P1 waits
P2 repeatedly enters
```

If P2 can bypass P1 forever:

```text
BW ✗
```

This is the systematic procedure emphasized in the lecture.

---

# 41. Useful Counterexamples

## To disprove ME

Construct:

```text
P1 → enters CS
P2 → enters CS
```

simultaneously.

---

## To disprove Progress

Construct:

```text
P1 wants CS
P2 does NOT want CS
```

and show that P2 can still prevent P1.

---

## To disprove BW

Construct:

```text
P1 waiting
P2 repeatedly enters
P2 repeatedly exits
P2 enters again
...
```

while P1 never gets its chance.

---

# 42. Deadlock vs Starvation/Fairness

### Deadlock

Processes wait for each other forever.

Example:

```text
P0 waits for P1
P1 waits for P0
```

### Bounded-Waiting Failure

A process keeps getting bypassed while another continues entering.

Think:

```text
P1 waiting
P2 keeps getting priority
```

The lecture uses bounded waiting as the fairness condition.

---

# 43. Strict Alternation vs `want[]`

|Feature|Strict Alternation|`want[]`|
|---|---|---|
|Mutual Exclusion|✓|✓|
|Progress|✗|✗|
|Bounded Waiting|✓|✓|
|Main Problem|Uninterested process can block|Both can wait forever|

---

# 44. Key Insight Before Peterson's Solution

We need **both**:

```text
want[i]
```

to tell the system:

> "I want to enter."

and:

```text
turn
```

to decide:

> "If both want to enter, who gets priority?"

So the required idea is:

```text
Interest
+
Tie breaking
```

This is the conceptual bridge to Peterson's solution.

---

# 45. Final Revision Map — Pages 45–95

```text
Implement Mutual Exclusion
        ↓
Simple interested variable
        ↓
Fails ME
        ↓
Strict Alternation using turn
        ↓
ME ✓
Progress ✗
BW ✓
        ↓
want[] approach
        ↓
ME ✓
Progress ✗
BW ✓
        ↓
Both can wait
        ↓
Deadlock
        ↓
Need better solution
        ↓
Formal Galvin definitions
        ↓
ME / Progress / BW
        ↓
Use these as GATE checking rules
        ↓
Interest + Turn
        ↓
Peterson's Solution
```

---

# 46. GATE Must-Remember

### Mutual Exclusion

```text
At most one process in CS
```

### Progress

```text
If CS is free and some process wants to enter,
the decision cannot be postponed indefinitely.
```

### Bounded Waiting

```text
A waiting process cannot be bypassed indefinitely.
```

### Strict Alternation

```text
ME ✓
Progress ✗
BW ✓
```

### `want[]` solution

```text
ME ✓
Progress ✗
BW ✓
```

### Common reasoning pattern

```text
Only one process wants → test Progress
Both want → test ME / Deadlock
One waits while other keeps entering → test BW
```

---

# 47. Questions Appearing in These Pages

The lecture includes GATE questions from:

- GATE CSE 2010
    
- GATE CSE 2007
    
- GATE CSE 1988
    
- GATE CSE 2015 Set 3
    
- GATE CSE 2016 Set 2
    

These are used to apply the three checks:

```text
ME
Progress
BW
```

We should solve them separately after the theory.