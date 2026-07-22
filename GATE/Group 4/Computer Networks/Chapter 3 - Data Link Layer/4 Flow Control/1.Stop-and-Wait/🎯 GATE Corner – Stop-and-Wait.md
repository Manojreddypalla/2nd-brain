# GATE Corner – Stop-and-Wait

## Definition

Stop-and-Wait is a flow control technique where the sender transmits one frame and waits for an ACK before sending the next frame.

---

## Key Features

- One frame in transit at a time.
- ACK required for every frame.
- Simple and reliable.
- Low channel utilization.

---

## Advantages

✅ Simple implementation

✅ Reliable communication

✅ Easy error handling

---

## Disadvantages

❌ High waiting time

❌ Low throughput

❌ Poor efficiency on long-delay links

---

## Common GATE Questions

### Q1. How many frames can be outstanding in Stop-and-Wait?

✅ One

---

### Q2. What does the sender do after sending a frame?

✅ Waits for an ACK

---

### Q3. Why is Stop-and-Wait inefficient?

✅ The sender remains idle while waiting for the acknowledgement.

---

## Common GATE Traps

❌ Multiple frames can be sent without waiting.

✔ Only one frame can be outstanding.

---

❌ Stop-and-Wait is suitable for high-speed, high-delay networks.

✔ It performs poorly on such networks because much of the time is spent waiting.

---

## Memory Trick

🧠 **SWA = Send → Wait → ACK → Send Again**

Remember:

> **One Frame, One ACK, Then Next Frame.**
> 