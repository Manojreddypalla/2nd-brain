> [!tip] What GATE Expects
> GATE mainly tests whether you understand **conditional atomic updates**, **how CAS differs from Test-and-Set**, **busy waiting**, and **lock-free programming**.

---

# ⭐ Frequently Tested Concepts

- CAS is a **hardware synchronization instruction**.
- CAS is **atomic**.
- CAS compares memory with an **expected value**.
- Memory is updated **only if the comparison succeeds**.
- CAS returns the **old value**.
- CAS is used in **lock-free algorithms**.
- Busy Waiting is still possible.
- Fairness is **not** guaranteed.
- Starvation is possible.

---

# ⭐ GATE Trap #1

> CAS always updates memory.

❌ False

CAS updates memory **only when**

```text
Current Value == Expected Value
```

Otherwise,

```text
No Update
```

---

# ⭐ GATE Trap #2

> CAS is just another name for Test-and-Set.

❌ False

### Test-and-Set

```text
Always

↓

Write 1
```

### Compare-and-Swap

```text
Compare

↓

Match?

↓

YES → Write

NO → Do Nothing
```

CAS performs **conditional updates**.

---

# ⭐ GATE Trap #3

> CAS removes Busy Waiting.

❌ False

Typical implementation

```cpp
while(CAS(lock,0,1)!=0);
```

The process continuously retries.

This is

- Busy Waiting
- Spin Waiting
- Spin Lock

---

# ⭐ GATE Trap #4

> CAS guarantees fairness.

❌ False

Some process may repeatedly fail the comparison.

Therefore,

- Fairness ❌
- Starvation Possible ✅

---

# ⭐ GATE Trap #5

> CAS is a software synchronization algorithm.

❌ False

CAS is implemented by the **CPU hardware**.

---

# ⭐ GATE Trap #6

> CAS prevents race conditions.

✅ True

The compare-and-update operation happens **atomically**.

No process can interrupt it midway.

---

# Important MCQ Facts

| Property | Compare-and-Swap |
|----------|------------------|
| Hardware Instruction | ✅ |
| Atomic | ✅ |
| Returns Old Value | ✅ |
| Conditional Update | ✅ |
| Updates Only if Expected Matches | ✅ |
| Busy Waiting | ✅ |
| Spin Lock | ✅ |
| Fairness | ❌ |
| Starvation | Possible |
| Lock-Free Algorithms | ✅ |

---

# Test-and-Set vs Compare-and-Swap

| Feature | Test-and-Set | Compare-and-Swap |
|---------|--------------|------------------|
| Atomic | ✅ | ✅ |
| Always Writes | ✅ | ❌ |
| Conditional Write | ❌ | ✅ |
| Lock-Free Algorithms | ❌ | ✅ |

---

# One-Liners for Revision

> CAS = **Compare → Match? → Update**

> If comparison fails → Memory remains unchanged.

> CAS performs **conditional atomic updates**.

> Test-and-Set always writes.

> CAS writes **only when necessary**.

> CAS is the foundation of modern lock-free programming.

---

# Previous-Year GATE Pattern

GATE commonly asks about:

- ✅ Difference between Test-and-Set and CAS
- ✅ Atomic operations
- ✅ Conditional updates
- ✅ Busy Waiting
- ✅ Lock-Free Algorithms
- ✅ Trace-based questions
- ✅ MSQs on synchronization properties

---

# 30-Second Revision

```text
Need Atomic Update
        │
        ▼
Compare-and-Swap
        │
        ▼
Compare Memory
        │
        ▼
Equal?
   │
   ├── Yes → Update
   │
   └── No → Do Nothing
        │
        ▼
Returns Old Value
        │
        ▼
Busy Waiting
        │
        ▼
Critical Section
```

---

# Interview Question

**Q:** Why is Compare-and-Swap preferred over Test-and-Set?

**Answer:**

Test-and-Set **always writes** to memory, even if the lock is already held, creating unnecessary memory traffic.

Compare-and-Swap first checks whether the current value matches the expected value. It updates memory **only if necessary**, reducing unnecessary writes and making it suitable for **lock-free concurrent algorithms**.

---

# Memory Trick 🧠

**CAS = Compare → And → Swap**

```text
Compare

↓

Same?

↓

YES

↓

Swap

↓

NO

↓

Don't Touch Memory
```

Remember:

> **Test-and-Set:** "Lock it anyway."

> **Compare-and-Swap:** "Lock it only if it's still free."