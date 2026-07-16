# GATE CSE 2025 SET-1 — Question 1

## Question

**Identify the ONE CORRECT matching between the OSI layers and their corresponding functionalities.**

|OSI Layer|Functionality|
|---|---|
|(a) Network Layer|(I) Packet Routing|
|(b) Transport Layer|(II) Framing and Error Handling|
|(c) Data Link Layer|(III) Host-to-Host Communication|

### Options

A. (a)-(I), (b)-(II), (c)-(III)

✅ **B. (a)-(I), (b)-(III), (c)-(II)**

C. (a)-(II), (b)-(I), (c)-(III)

D. (a)-(III), (b)-(II), (c)-(I)

---

# Answer

**Correct Option: B**

- **Network Layer → Packet Routing**
- **Transport Layer → Host-to-Host Communication**
- **Data Link Layer → Framing and Error Handling**

---

# Explanation

## 1. Network Layer → Packet Routing ✔

The **Network Layer (Layer 3)** is responsible for moving packets between different networks.

Its primary functions are:

- Logical Addressing (IP Address)
- Routing
- Path Selection
- Packet Forwarding

Think of it as **Google Maps** deciding the best route from the source to the destination.

**Keywords:** Routing, IP Address, Packet

---

## 2. Transport Layer → Host-to-Host Communication ✔

The **Transport Layer (Layer 4)** provides reliable **end-to-end (host-to-host)** communication.

Responsibilities include:

- Segmentation
- Reliability (TCP)
- Error Recovery
- Flow Control
- Sequencing

Think of it as a courier company ensuring every package reaches the receiver in the correct order.

**Keywords:** TCP, UDP, End-to-End Communication, Segment

---

## 3. Data Link Layer → Framing and Error Handling ✔

The **Data Link Layer (Layer 2)** is responsible for communication within the same network.

Its functions include:

- Framing
- MAC Addressing
- Error Detection (CRC)
- Flow Control
- Local Delivery

Think of it as packaging data into **frames** before transmission over the physical medium.

**Keywords:** Frame, MAC Address, CRC, Error Detection

---

# Why Other Options Are Incorrect

### Option A

Transport Layer is matched with **Framing**, which belongs to the **Data Link Layer**.

---

### Option C

Network Layer is matched with **Framing**, which is incorrect.

Routing is always the responsibility of the **Network Layer**.

---

### Option D

Network Layer is matched with **Host-to-Host Communication**, which is the responsibility of the **Transport Layer**.

---

# Theory Revision

|OSI Layer|Primary Function|
|---|---|
|Physical|Transmits Bits|
|Data Link|Framing, MAC Addressing, Error Detection|
|Network|Routing, Logical Addressing (IP)|
|Transport|End-to-End Communication, Reliability|
|Session|Session Establishment and Management|
|Presentation|Encryption, Compression, Translation|
|Application|Network Services to User Applications|

---

# GATE Takeaway

Remember the signature function of each layer:

- **Physical** → Bits
- **Data Link** → Frame, MAC, CRC
- **Network** → Routing, IP
- **Transport** → End-to-End Communication
- **Session** → Session Management
- **Presentation** → Encryption & Compression
- **Application** → User Services

> **GATE Tip:** Matching questions often test the **primary responsibility** of each OSI layer. If you remember each layer's signature function, you can solve these questions without memorizing every protocol.