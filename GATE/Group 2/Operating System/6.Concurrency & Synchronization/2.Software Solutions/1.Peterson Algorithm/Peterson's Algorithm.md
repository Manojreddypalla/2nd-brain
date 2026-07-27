# Peterson's Algorithm

---

# Definition

**Peterson's Algorithm** is a **software-based synchronization algorithm** that provides a solution to the **Critical Section Problem** for **exactly two processes**.

It guarantees:

- ✅ Mutual Exclusion
- ✅ Progress
- ✅ Bounded Waiting

without using any special hardware instructions.

---

# Why Do We Need Peterson's Algorithm?

Suppose two processes want to execute:

```c
count++;
```

If both execute simultaneously,

```
Read count
↓

Modify count

↓

Write count
```

their operations may overlap, causing a **Race Condition**.

Peterson's Algorithm ensures that **only one process enters the Critical Section at a time**.

---

# Prerequisites

Before Peterson's Algorithm, you should know:

- Concurrency
- Race Condition
- Critical Section
- Mutual Exclusion
- Progress
- Bounded Waiting

Peterson's Algorithm is the **first complete software solution** satisfying all three Critical Section requirements.

---

# Assumptions

Peterson's Algorithm works only if:

- Exactly **2 Processes**
- Shared Memory
- Atomic Read/Write operations
- Sequentially consistent memory model

Processes are named:

```
P0

P1
```

---

# Shared Variables

Peterson's Algorithm uses two shared variables.

## 1. flag[]

```c
bool flag[2];
```

Purpose:

Indicates whether a process wants to enter the Critical Section.

Example

```c
flag[0] = true;
```

means

> Process P0 wants to enter the Critical Section.

Initially

```c
flag[0]=false;
flag[1]=false;
```

---

## 2. turn

```c
int turn;
```

Purpose:

Resolves conflicts when **both processes want to enter simultaneously**.

Possible values

```
0

or

1
```

Example

```
turn = 0
```

means

> Give priority to Process P0.

---

# Intuition

Think of two people trying to enter a room.

Each person:

1. Raises their hand.

```
"I want to enter."
```

↓

This is

```c
flag[i]=true;
```

Then says

```
"If we both want to enter,

I'll let you go first."
```

↓

This is

```c
turn = j;
```

If both say this simultaneously,

the **last process to update `turn` decides who gets priority**.

---

# Algorithm

For Process Pi

```c
flag[i] = true;      // I want to enter
turn = j;            // Give priority to other process

while(flag[j] && turn == j)
{
    // Busy Waiting
}

// Critical Section

flag[i] = false;

// Remainder Section
```

Where

```
j = 1 - i
```

---

# Dry Run 1 (Only P0 Wants to Enter)

Initial State

```
flag0 = false
flag1 = false
```

---

### Step 1

P0

```c
flag0 = true;
```

```
flag0 = true
flag1 = false
```

---

### Step 2

```c
turn = 1;
```

```
flag0 = true
flag1 = false
turn = 1
```

---

### Step 3

P0 checks

```c
while(flag1 && turn == 1)
```

Substitute values

```
false && true
```

↓

```
false
```

Condition is false.

✅ P0 enters the Critical Section.

---

### Step 4

P0 exits

```c
flag0 = false;
```

Done.

---

# Dry Run 2 (Both Want to Enter)

Initial State

```
flag0 = false
flag1 = false
```

---

### Step 1

P0

```c
flag0 = true;
```

```
flag0 = true
flag1 = false
```

---

### Step 2

P0

```c
turn = 1;
```

```
flag0 = true
flag1 = false
turn = 1
```

---

### Step 3

CPU switches to P1.

P1

```c
flag1 = true;
```

```
flag0 = true
flag1 = true
turn = 1
```

---

### Step 4

P1

```c
turn = 0;
```

```
flag0 = true
flag1 = true
turn = 0
```

Notice:

The **last write** changes the value of `turn`.

---

### Step 5

CPU switches back to P0.

P0 checks

```c
while(flag1 && turn == 1)
```

Substitute values

```
true && false
```

↓

```
false
```

✅ P0 enters the Critical Section.

---

### Step 6

P1 checks

```c
while(flag0 && turn == 0)
```

Substitute

```
true && true
```

↓

```
true
```

❌ P1 waits.

---

### Step 7

P0 exits

```c
flag0 = false;
```

State

```
flag0 = false
flag1 = true
turn = 0
```

---

### Step 8

P1 checks again

```c
while(flag0 && turn == 0)
```

Substitute

```
false && true
```

↓

```
false
```

Now P1 enters.

---

# Timeline

```
Time →

P0                           P1
────────────────────────────────────────────

flag0=true

turn=1

                             flag1=true

                             turn=0

while(true && false)

↓

ENTER CS

                             while(true && true)

                             WAIT

flag0=false

                             while(false && true)

                             ↓

                             ENTER CS
```

---

# Why Does It Work?

## Mutual Exclusion

If both processes want to enter simultaneously,

only one satisfies the while condition.

Therefore,

only one enters the Critical Section.

---

## Progress

If only one process wants to enter,

it immediately enters because the other process's flag is false.

No unnecessary waiting occurs.

---

## Bounded Waiting

Once one process exits,

it clears its flag.

The waiting process eventually enters.

Thus,

no process waits forever.

---

# Busy Waiting

The waiting process repeatedly checks

```c
while(flag[j] && turn==j)
```

without sleeping.

This is called

- Busy Waiting
- Spin Waiting

It wastes CPU cycles while waiting.

---

# Advantages

- Simple
- Pure software solution
- No hardware instructions required
- Guarantees all three Critical Section properties
- Excellent for understanding synchronization

---

# Limitations

- Works only for **2 processes**
- Uses Busy Waiting
- Inefficient on modern systems
- Not used in real operating systems
- Modern OS uses Mutexes, Semaphores, Spinlocks, Monitors, etc.

---

# Peterson vs Mutex

| Peterson | Mutex |
|-----------|-------|
| Software algorithm | OS synchronization primitive |
| Only 2 processes | Multiple threads/processes |
| Busy Waiting | Usually blocks waiting threads |
| Educational | Practical |
| No OS support | OS supported |

---

# GATE Points ⭐

- Software solution
- Exactly **2 processes**
- Uses shared variables:
  - `flag[]`
  - `turn`
- Uses Busy Waiting
- Guarantees:
  - Mutual Exclusion
  - Progress
  - Bounded Waiting
- No special hardware required

---

# Common GATE Traps ⚠️

❌ Works for multiple processes

→ False

---

❌ Eliminates Busy Waiting

→ False

---

❌ Uses hardware instructions

→ False

---

❌ Only guarantees Mutual Exclusion

→ False

It guarantees all three:

- Mutual Exclusion
- Progress
- Bounded Waiting

---

# Memory Trick 🧠

```
flag[]

↓

"I WANT TO ENTER"

turn

↓

"YOU GO FIRST"

while()

↓

"I WILL WAIT"

↓

Critical Section
```

---

# Quick Revision ⭐

- Exactly **2 Processes**
- `flag[]` → Interest
- `turn` → Tie breaker / Priority
- `while()` → Busy Waiting
- Guarantees:
  - Mutual Exclusion
  - Progress
  - Bounded Waiting
- Pure software solution
- Mainly asked in GATE conceptual questions