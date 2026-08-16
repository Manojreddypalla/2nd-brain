Yep — if you want **short Obsidian notes**, not the giant lecture rewrite, this is the version I'd keep. It covers the actual Lecture 6 scope and the questions/concepts shown in it.

# Lecture 6 — Switching, Physical Layer & Framing

## 1. Switching Networks

- Used for **long-distance communication** through intermediate switching nodes.
    
- Main types:
    

```text
Switching
├── Circuit Switching
└── Packet Switching
    ├── Virtual Circuit
    └── Datagram
```

---

## 2. Circuit Switching

**Idea:** Establish a path and reserve resources **before** sending data.

### Characteristics

- Setup required.
    
- Dedicated/reserved capacity.
    
- Data sent continuously.
    
- Bandwidth guaranteed during session.
    
- If required resources are unavailable → **connection refused**.
    

```text
Setup → Data Transfer → Teardown
```

**Example:** Traditional telephone network.

### Problem

Bad for **bursty traffic** because reserved bandwidth can remain unused.

---

# 3. Packet Switching

**Idea:** Break data into packets and share network links.

- No dedicated resource reservation.
    
- Packets are sent when ready.
    
- Header contains information used for forwarding.
    
- Better utilization of links.
    
- If outgoing link is busy → **queue packet**.
    
- If queue is full → **drop packet**.
    

### Types

```text
Packet Switching
├── Datagram
└── Virtual Circuit
```

---

# 4. Datagram Network

- **No setup phase.**
    
- Each packet is independent.
    
- Packet contains **destination address**.
    
- Each packet can take a different route.
    
- No resources are reserved.
    

```text
P1 → A → B → C → D
P2 → A → E → F → D
```

### Advantage

More tolerant of failures because packets can be **routed around failed nodes**.

---

# 5. Virtual Circuit (VC)

**Hybrid of circuit + packet switching.**

### Characteristics

- Setup/handshake before data.
    
- Preplanned route established.
    
- Each packet carries a **VC identifier**, not destination address.
    
- Routing decision is made during setup.
    
- Links can be shared by multiple VCs.
    
- Resources such as bandwidth/buffers **may** be allocated.
    
- Teardown/clear request removes VC.
    

### VC consists of

```text
VC = Path + VC number on each link
```

### Forwarding

```text
Incoming (Port, VC)
        ↓
 Forwarding Table
        ↓
Outgoing (Port, VC)
```

Example:

```text
(Port 1, VC 14) → (Port 3, VC 22)
```

---

# 6. VC Failure

If a link in the VC fails:

- Re-establish the virtual circuit, **or**
    
- Drop the VC and let a higher layer handle recovery.
    

### Key intuition

**Datagram:** route can change per packet.

**VC:** route is established first → failure can affect the whole VC.

---

# 7. Datagram vs VC

|Feature|Datagram|Virtual Circuit|
|---|---|---|
|Setup|❌|✅|
|Address|Destination address|VC identifier|
|Route|Per packet|Established beforehand|
|Routing|Every packet|During setup|
|Resource reservation|❌|May be allocated|
|Failure tolerance|Higher|Lower|

### GATE point ⭐

> If nodes are highly failure-prone → **Datagram is preferable**, because packets can be routed around failed nodes.

---

# 8. Why Internet Uses Packet Switching?

Internet applications are generally **bursty**.

Examples:

- Email uses bandwidth mainly during send/receive.
    
- Web browsing uses bandwidth mainly when data is requested.
    

Therefore:

> **Packet switching efficiently shares bandwidth for bursty applications.**

---

# 9. Physical Layer

### Main job

> Transfer **bits over a physical link using signals**.

```text
Bits → Signal → Physical Medium → Signal → Bits
```

### Responsibilities

- Representation of bits.
    
- Encoding `0` and `1` into signals.
    
- Bit length / data rate.
    
- Transmission over physical medium.
    

### Coding & Modulation

- **Coding:** represent bits using signals.
    
- **Modulation:** encode information onto a signal/carrier.
    
- **Modem = Modulator + Demodulator**.
    

### Media

- Wire
    
- Fiber optic
    
- Wireless
    

---

# 10. Data Link Layer

The link layer works above the physical layer.

```text
Application
Transport
Network
→ Link
→ Physical
```

Usually implemented in the **network adapter / NIC**.

```text
Network-layer packet
        ↓
       NIC
        ↓
      Frame
        ↓
 Physical medium
```

---

# 11. Functions of Data Link Layer

```text
Data Link Layer
├── Framing
├── Error Control
├── Flow Control
└── Link Access / MAC
```

### Framing

Break bit stream into frames.

### Error Control

Detect/correct errors.

Methods:

- Parity
    
- Checksum
    
- CRC
    

### Flow Control

Prevent sender from overwhelming receiver.

Methods:

- Stop-and-Wait
    
- Go-Back-N
    
- Selective Repeat
    

### Link Access / MAC

Controls access to a shared medium.

Examples:

- ALOHA
    
- CSMA/CD
    
- Polling
    

---

# 12. Framing

### Problem

Physical layer gives us only a continuous stream:

```text
101101001011010010...
```

Receiver needs to know:

```text
Frame 1 | Frame 2 | Frame 3
```

> **Framing = dividing the bit stream into identifiable frames.**

---

# 13. Fixed-Length Framing

Every frame has the same size.

```text
[FRAME][FRAME][FRAME]
   8B     8B     8B
```

### Advantage

- Simple.
    
- Receiver knows boundaries from frame size.
    

### Disadvantage

- May be inefficient for very small/large data.
    
- Restricts us to a particular frame size.
    

---

# 14. Variable-Length Framing

Frames can have different sizes.

Two general approaches:

```text
Variable Length
├── Length-based
│   └── Byte Count
└── Delimiter-based
    ├── Byte Stuffing
    └── Bit Stuffing
```

---

# 15. Byte Count

Put the **length of the frame** at the beginning.

```text
[Length][Frame Data]
```

Example:

```text
5 | Data...
5 | Data...
8 | Data...
```

Receiver reads the length → counts that many bytes → identifies frame boundary.

### Problem ⚠️

If length field gets corrupted:

```text
53 → 58
```

Receiver reads the wrong number of bytes → loses frame synchronization.

---

# 16. Byte Stuffing

Use a special **FLAG byte** to mark frame boundaries.

```text
FLAG | DATA | FLAG
```

### Problem

What if FLAG occurs inside actual data?

```text
FLAG | A | FLAG | B | FLAG
```

Receiver may mistake the middle FLAG for a boundary.

### Solution: ESC

```text
FLAG in data → ESC FLAG
ESC in data  → ESC ESC
```

### Example

```text
Original:
A FLAG B

After stuffing:
A ESC FLAG B
```

Receiver removes the ESC.

---

# 17. Bit Stuffing

Works at **bit level**.

### Flag

```text
01111110
```

### Sender rule

> After **5 consecutive 1s**, insert a `0`.

```text
11111 → 111110
```

### Receiver rule

> Remove the `0` after 5 consecutive `1`s.

```text
111110 → 11111
```

Purpose: prevent the flag pattern from accidentally appearing inside data.

---

# 18. Byte vs Bit Stuffing ⭐

||Byte Stuffing|Bit Stuffing|
|---|---|---|
|Works on|Bytes|Bits|
|Delimiter|FLAG byte|`01111110`|
|Solution|ESC|Insert `0`|
|Sender|FLAG → ESC FLAG|5 ones → add 0|
|Receiver|Remove ESC|Remove stuffed 0|

### Memory trick

> **Byte → ESC it**  
> **Bits → Stuff a 0 after five 1s**

---

# 19. Lecture Questions

### Q1. Why prefer Datagram when nodes fail frequently?

**Answer:** Packets can be routed around failed/problematic nodes; a VC failure can wipe out the entire circuit.

### Q2. Does VC require setup?

**TRUE.**

A VC requires a circuit-establishment phase before data transmission.

### Q3. Why don't VC packets need source/destination addresses?

Because the route is already established and forwarding is based on the **VC identifier**.

### Q4. Why does Internet use packet switching?

Because Internet traffic is **bursty**, so shared packet switching uses bandwidth more efficiently.

### Q5. Taipei Bus vs MRT

- **Bus:** Packet switching with VC-like fixed route, but route/resources aren't exclusively reserved.
    
- **MRT:** More like circuit switching because the route/schedule is predetermined.
    

---

# 20. Homework ⭐

Given:

```text
A    = 01000111
B    = 11100011
FLAG = 01111110
ESC  = 11100000
```

For frame:

```text
A B ESC FLAG
```

Find transmitted bits using:

1. **Byte count**
    
2. **Flag bytes + byte stuffing**
    
3. **Starting/ending flags + bit stuffing**
    

This is the explicit homework problem on page 93.

---

## 🔥 Ultra-Short Revision

```text
SWITCHING
Circuit → setup + dedicated resources
Packet → shared resources

PACKET
Datagram → no setup, destination address, independent paths
VC → setup, VC ID, fixed logical path

PHYSICAL
Bits → signals → medium

LINK
Packet → Frame
Functions:
Framing + Error Control + Flow Control + MAC

FRAMING
Fixed length
Variable length
 ├─ Byte Count
 ├─ Byte Stuffing
 └─ Bit Stuffing

BYTE STUFFING
FLAG → ESC FLAG
ESC  → ESC ESC

BIT STUFFING
After 5 consecutive 1s → insert 0
Flag = 01111110
```

This is the version I'd actually put into **Obsidian for revision**—short enough to revisit quickly, but it still contains the GATE-important distinctions and the lecture's questions.