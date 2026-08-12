# OS — Synchronization & Critical Section

## Synchronization

- Needed when **multiple processes/threads access shared memory/data concurrently**.
    
- Goal: avoid incorrect/inconsistent results.
    

## Race Condition

- Occurs when multiple processes access/manipulate the **same data concurrently**.
    
- The final result **depends on the order of execution**.
    

### Example

```c
x = x + 1;
```

Internally it may be:

```text
LOAD x
ADD 1
STORE x
```

Two threads can both read the same old value, causing a **lost update**.

---

## Critical Section

- A code segment that **accesses shared variables/resources**.
    
- It should execute as an **atomic operation**.
    
- At most **one process** should execute its critical section at a time.
    

### Structure

```text
do {
    Entry Section
    Critical Section
    Exit Section
    Remainder Section
} while (true);
```

- **Entry section** → request permission to enter CS
    
- **Critical section** → access shared data
    
- **Exit section** → release access
    
- **Remainder section** → remaining code
    

---

# Requirements of Critical-Section Solution

## 1. Mutual Exclusion (ME)

- **At most one** process can be in the critical section at a time.
    

```text
ME ≤ 1
```

## 2. Progress

- If no process is in the CS and some processes want to enter, **at least one should be allowed to enter**.
    
- The system should not unnecessarily keep everyone waiting.
    

## 3. Bounded Waiting (BW)

- A waiting process should not be postponed **forever**.
    
- A process that repeatedly enters CS should not keep getting priority over waiting processes.
    

---

## Quick GATE Check

For any synchronization algorithm, check:

```text
ME   → Can 2 processes enter CS together?
Progress → Can everyone get stuck unnecessarily?
BW   → Can one process wait forever?
```

A correct critical-section solution must satisfy:

```text
ME + Progress + Bounded Waiting
```