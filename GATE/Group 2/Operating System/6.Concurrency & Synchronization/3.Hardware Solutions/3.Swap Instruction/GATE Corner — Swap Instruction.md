# 🎯 GATE Corner — Swap Instruction

> [!tip] What GATE Expects
> GATE rarely asks Swap directly, but it often asks you to **compare it with Test-and-Set and Compare-and-Swap** or identify its properties.

---

# ⭐ Frequently Tested Concepts

- Swap is a **hardware synchronization instruction**.
- Swap is **atomic**.
- Swap exchanges **two values**.
- Used to implement **Mutual Exclusion**.
- Used in **Spin Locks**.
- Busy Waiting still exists.
- Fairness is not guaranteed.
- Starvation is possible.

---

# ⭐ GATE Trap #1

> Swap returns the old value.

❌ False

Swap simply exchanges two values.

---

# ⭐ GATE Trap #2

> Swap compares values before updating.

❌ False

Only **Compare-and-Swap** performs a comparison.

---

# ⭐ GATE Trap #3

> Swap eliminates Busy Waiting.

❌ False

Busy Waiting (Spin Waiting) is still present.

---

# ⭐ GATE Trap #4

> Swap is a software synchronization algorithm.

❌ False

Swap is implemented by **hardware**.

---

# ⭐ GATE Trap #5

> Swap exchanges two variables atomically.

✅ True

That is the definition of Swap.

---

# Important MCQ Facts

| Property | Swap |
|----------|------|
| Hardware Instruction | ✅ |
| Atomic | ✅ |
| Exchanges Values | ✅ |
| Returns Old Value | ❌ |
| Conditional Update | ❌ |
| Busy Waiting | ✅ |
| Spin Lock | ✅ |
| Fairness | ❌ |
| Starvation | Possible |

---

# Test-and-Set vs CAS vs Swap

| Feature | Test-and-Set | CAS | Swap |
|---------|--------------|-----|------|
| Returns Old Value | ✅ | ✅ | ❌ |
| Conditional Update | ❌ | ✅ | ❌ |
| Exchanges Values | ❌ | ❌ | ✅ |
| Always Writes | ✅ | ❌ | Exchange |

---

# One-Liners for Revision

> Swap = **Exchange two values atomically.**

> Swap is a hardware synchronization primitive.

> Swap does **not** compare values.

> Swap does **not** return the old value.

> Busy Waiting still exists.

---

# Previous-Year GATE Pattern

Questions are generally about:

- Hardware vs Software synchronization
- Atomic operations
- Comparison with Test-and-Set and CAS
- Busy Waiting
- Mutual Exclusion

---

# 30-Second Revision

```text
Need Mutual Exclusion
        │
        ▼
Swap
        │
        ▼
Exchange
lock ↔ key
        │
        ▼
key = 0
        │
Enter CS
        │
key = 1
        │
Busy Wait
```

---

# Interview Question

**Q:** How is Swap different from Compare-and-Swap?

**Answer:**

- **Swap** always exchanges two values atomically.
- **Compare-and-Swap** updates memory **only if** the current value matches the expected value.

---

# Memory Trick 🧠

Remember:

> **Swap = Just Exchange**

> **CAS = Compare then Exchange**

> **Test-and-Set = Set no matter what**