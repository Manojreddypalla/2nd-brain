That's actually the best thing to do now. Since you've just finished all 7 layers, solving questions immediately will lock everything into long-term memory.

From now on, we'll use this format for every PYQ:

---

# 📌 GATE PYQ

### Question

> _(Original GATE question)_

---

## 🧠 How to Think

Before looking at the options, ask:

1. **What is the keyword?**
    
2. **Which OSI layer is responsible for that keyword?**
    
3. **What does that layer actually do?**
    

---

## 🔍 Analysis

Explain each option one by one.

### Option A

- Why it is correct/incorrect.
    
- Which layer it belongs to.
    
- Why other layers cannot perform that function.
    

### Option B

...

---

## ✅ Correct Answer

**Option X**

---

## 💡 Why?

Short explanation.

---

## 🎯 GATE Trick

> If you see **"Routing"**, think **Layer 3**.

or

> If you see **"MAC Address"**, think **Layer 2**.

---

## 📝 Revision Note

- Layer:
    
- Keyword:
    
- Concept Tested:
    
- Common Mistake:
    

---

# Example 1

## 📌 GATE PYQ

### Question

**In the following pairs of OSI protocol layer/sub-layer and its functionality, the INCORRECT pair is**

A. Network Layer — Routing

B. Data Link Layer — Bit Synchronization

C. Transport Layer — End-to-End Process Communication

D. MAC Sublayer — Channel Sharing

---

## 🧠 How to Think

Find the **keyword** in every option.

- Routing
    
- Bit Synchronization
    
- End-to-End Communication
    
- Channel Sharing
    

Now match them with OSI responsibilities.

---

## 🔍 Analysis

### A. Network Layer — Routing

Layer 3 performs:

- Routing
    
- Path Selection
    
- Packet Forwarding
    

✅ Correct.

---

### B. Data Link Layer — Bit Synchronization

Bit synchronization means identifying where one bit ends and the next begins during transmission.

This is handled by the **Physical Layer**, not the Data Link Layer.

The Data Link Layer starts working **after** the bits have been received correctly.

❌ Incorrect Pair.

---

### C. Transport Layer — End-to-End Process Communication

Transport Layer provides:

- End-to-End Communication
    
- Process-to-Process Communication
    
- Reliability
    

✅ Correct.

---

### D. MAC Sublayer — Channel Sharing

The MAC sublayer decides which device gets access to the shared communication medium.

Examples:

- CSMA/CD
    
- CSMA/CA
    

✅ Correct.

---

## ✅ Correct Answer

**Option B**

---

## 🎯 GATE Trick

If you see:

- **Bit Synchronization** → Physical Layer
    
- **Framing** → Data Link Layer
    
- **Routing** → Network Layer
    
- **Port Number** → Transport Layer
    

---

## 📝 Revision Note

- **Layer:** Physical Layer
    
- **Concept:** Bit Synchronization
    
- **Common Mistake:** Confusing Framing with Bit Synchronization.
    

---

# Example 2

## 📌 GATE PYQ

### Question

An IP machine **Q** communicates with **H** through routers **R1 → R2 → R3**.

The communication uses:

- HTTP
    
- Session-layer encryption (DES)
    

An attacker is sniffing only at **R2**.

Which information can the attacker learn?

- I1: URL
    
- I2: TCP Port Numbers
    
- I3: IP Addresses
    
- I4: MAC Addresses of Q and H
    

---

## 🧠 How to Think

Ask:

> **What information is visible to a router?**

Remember the packet structure.

```text
Application Data (Encrypted)
        ↓
TCP Header
        ↓
IP Header
        ↓
MAC Header (Current Hop Only)
```

---

## 🔍 Analysis

### I1 — URL

Application Layer data.

Encrypted using DES.

❌ Cannot read.

---

### I2 — TCP Port Numbers

Stored in TCP Header.

Visible.

✅ Can read.

---

### I3 — IP Addresses

Stored in IP Header.

Routers require these for routing.

✅ Can read.

---

### I4 — MAC Addresses

MAC addresses change at every hop.

At R2,

Router only sees:

```
R1 ↔ R2
or
R2 ↔ R3
```

It never sees:

```
Q ↔ H
```

❌ Cannot read.

---

## ✅ Correct Answer

**Only I2 and I3**

---

## 🎯 GATE Trick

Remember:

```
Application Data → Can be encrypted

TCP Header → Visible

IP Header → Visible

MAC Header → Local Link Only
```

---

## 📝 Revision Note

- **Layer:** 2, 3, 4, 7
    
- **Concept:** Encapsulation
    
- **Common Mistake:** Thinking MAC Address travels end-to-end.
    

---

I think this format is perfect for your **Obsidian GATE vault**. Every solved PYQ becomes a mini concept note, not just an answer. After 200–300 PYQs, you'll have a complete revision notebook where each question reinforces the underlying concept instead of just the correct option.