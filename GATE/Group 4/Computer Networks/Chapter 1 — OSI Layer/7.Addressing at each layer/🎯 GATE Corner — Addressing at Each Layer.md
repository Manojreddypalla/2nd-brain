# 🎯 GATE Corner — Addressing at Each Layer

## Important Facts ⭐⭐⭐⭐⭐

- ✅ **Application Layer → Name** (Domain Name, URL, Email)
    
- ✅ **Transport Layer → Port Number**
    
- ✅ **Network Layer → IP Address**
    
- ✅ **Data Link Layer → MAC Address**
    
- ✅ **Physical Layer → No Address**
    

---

## Layer vs Address vs Device

|Layer|Address|Used By|
|---|---|---|
|Application|Domain Name / URL|DNS|
|Transport|Port Number|TCP/UDP|
|Network|IP Address|Routers|
|Data Link|MAC Address|Switches/NIC|
|Physical|None|Transmission Medium|

---

## GATE Traps ⚠️

❌ **MAC Address identifies a host on the Internet.**

✔ **False** → **IP Address** identifies a host across networks.

---

❌ **Port Number identifies a computer.**

✔ **False** → It identifies a **process/application**.

---

❌ **Routers use MAC addresses to route packets.**

✔ **False** → Routers use **IP addresses**. MAC addresses are used only for **local (LAN) delivery**.

---

❌ **Switches forward frames using IP addresses.**

✔ **False** → Switches use **MAC addresses**.

---

❌ **Physical layer has an address.**

✔ **False** → It only transmits bits/signals.

---

❌ **Domain names are used for routing.**

✔ **False** → Domain names are first resolved into **IP addresses** using DNS.

---

## Frequently Asked GATE Concepts ⭐⭐⭐

- **Host Identification** → IP Address
    
- **Process Identification** → Port Number
    
- **Device Identification (LAN)** → MAC Address
    
- **Human-readable Identification** → Domain Name / URL
    

---

## PYQ-Style MCQs

### Q1.

Which address is used by a router for packet forwarding?

A. MAC Address  
B. Port Number  
C. IP Address  
D. Domain Name

✅ **Answer:** **C**

---

### Q2.

Which layer is responsible for process-to-process communication?

A. Network  
B. Data Link  
C. Transport  
D. Physical

✅ **Answer:** **C** (Uses **Port Numbers**)

---

### Q3.

Which address is used by switches to forward frames?

A. IP Address  
B. MAC Address  
C. Port Number  
D. URL

✅ **Answer:** **B**

---

### Q4.

Which layer does **not** use any addressing?

A. Data Link  
B. Physical  
C. Network  
D. Transport

✅ **Answer:** **B**

---

## Memory Trick 🧠

```text
Name  → Service
Port  → Process
IP    → Host
MAC   → Device (LAN)
None  → Bits
```

---

## One-Line Revision 📝

> **Name identifies a service, Port identifies a process, IP identifies a host, MAC identifies a device on a LAN, and the Physical layer has no addressing.**