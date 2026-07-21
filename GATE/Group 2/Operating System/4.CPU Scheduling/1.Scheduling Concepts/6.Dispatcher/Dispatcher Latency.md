# GATE Corner – Dispatcher Latency

## Definition

**Dispatcher Latency** is the time required by the Dispatcher to switch the CPU from one process to another.

---

## Includes

✅ Context Switching

✅ Mode Switching (Kernel → User)

✅ Transfer of CPU Control

---

## Does NOT Include

❌ Process execution time

❌ CPU Burst

❌ I/O Burst

❌ Scheduling decision time

---

## Key Points

- Dispatcher Latency is **pure overhead**.
- No useful user process executes during this time.
- Every context switch introduces dispatcher latency.
- Lower dispatcher latency improves system performance and response time.

---

## Previous GATE Questions

### Q1. Dispatcher Latency is the time taken to:

✅ Stop one process and start another.

---

### Q2. Dispatcher Latency includes?

✅ Context Switching

✅ Mode Switching

✅ Transfer of CPU Control

---

### Q3. Dispatcher Latency is considered:

✅ Overhead

---

### Q4. Lower Dispatcher Latency results in:

✅ Better CPU Utilization

✅ Better Response Time

---

## Common GATE Traps

❌ Dispatcher Latency = Context Switching only.

✔ It includes **Context Switching + Mode Switching + Transfer of CPU Control**.

---

❌ Useful work is performed during Dispatcher Latency.

✔ No user process performs useful work during this period.

---

## Memory Trick

🧠 **Dispatcher Latency = CMT Delay**

- **C** → Context Switching
- **M** → Mode Switching
- **T** → Transfer of CPU Control

> **CMT = Dispatcher Latency**