## 🎯 GATE Corner — Service Access Point (SAP)

### Important Exam Facts ⭐⭐⭐⭐⭐

- ✅ SAP = **Interface (Access Point)** between **adjacent layers**.
    
- ✅ SAP exists **within the same host**, **not between two hosts**.
    
- ✅ Higher layer **uses** the services of the lower layer.
    
- ✅ Lower layer **provides** the services.
    
- ✅ During **encapsulation**, data moves **downward** through SAP.
    
- ✅ During **decapsulation**, processed data moves **upward** through the **same SAP**.
    
- ✅ SAP is **not** a protocol.
    
- ✅ SAP is **not** the service itself.
    
- ✅ SAP enables **layer independence (abstraction)**.
    

---

## GATE Traps ⚠️

❌ SAP is used for communication between two computers.

✔ **False** — That's the job of **protocols** (e.g., TCP, IP).

---

❌ SAP and Protocol are the same.

✔ **False**

- **SAP** → Vertical interface (same host)
    
- **Protocol** → Horizontal communication (different hosts)
    

---

❌ SAP is only used during data transmission.

✔ **False**

It is used during **both**:

- Encapsulation (downward)
    
- Decapsulation (upward)
    

---

❌ Lower layer knows how the upper layer works.

✔ **False**

The lower layer only provides a **service**; it doesn't know the upper layer's internal implementation.

---

## PYQ/MCQ Practice

### Q1

A Service Access Point (SAP) is:

A. A communication protocol

B. A routing algorithm

C. An interface between adjacent layers

D. A transmission medium

✅ **Answer: C**

---

### Q2

SAP exists between:

A. Two hosts

B. Adjacent layers of the same host

C. Two routers

D. Sender and receiver

✅ **Answer: B**

---

### Q3

Which statement is correct?

A. SAP defines communication rules between hosts.

B. SAP provides routing functionality.

C. SAP is the interface through which higher layers access lower-layer services.

D. SAP exists only in the OSI model.

✅ **Answer: C**

---

### Q4

During decapsulation:

A. The application layer requests data from every lower layer.

B. Each lower layer automatically processes the received data and passes the payload upward.

C. SAP is not used.

D. Headers are added.

✅ **Answer: B**

---

## One-Liner for Revision 📝

> **SAP = Vertical Interface (Same Host) | Protocol = Horizontal Communication (Different Hosts).** This distinction is one of the most common GATE concepts.