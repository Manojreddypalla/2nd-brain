# GATE Computer Networks — Chapter 1 Complete Revision Notes

# **OSI Model & TCP/IP Architecture (Master Summary)**

This chapter looks simple, but **GATE repeatedly asks conceptual traps** from it. There are almost **no formulas**, but there are many confusing statements.

---

# 1. Big Picture

Whenever two computers communicate,

```
Application
     ↓
Presentation
     ↓
Session
     ↓
Transport
     ↓
Network
     ↓
Data Link
     ↓
Physical
```

Each layer

- performs one specific job
    
- adds its own header
    
- passes data downward.
    

Receiver performs the reverse process.

---

# 2. OSI Model

OSI = **Open Systems Interconnection**

Developed by **ISO**

Purpose

> Standardize communication between different vendors.

Remember

> OSI is a **Reference Model**
> 
> It is NOT an actual protocol.

GATE Trap

> TCP/IP is actually used.
> 
> OSI mainly explains networking.

---

# 3. Seven Layers

---

## Layer 7 — Application

Provides services directly to users.

Examples

- HTTP
    
- FTP
    
- SMTP
    
- DNS
    
- SSH
    

PDU

```
Data
```

Responsibilities

- Email
    
- File transfer
    
- Web browsing
    

Trap

Application Layer ≠ User Interface

It provides **network services**, not GUI.

---

## Layer 6 — Presentation

Responsible for

- Translation
    
- Encryption
    
- Compression
    

Examples

ASCII ↔ Unicode

JPEG Compression

SSL/TLS encryption (conceptually)

Think

```
Different Languages

↓

Presentation converts

↓

Common Language
```

---

## Layer 5 — Session

Responsible for

- Session establishment
    
- Session maintenance
    
- Session termination
    

Also

- Synchronization
    
- Checkpoints
    

Example

Video call

If connection drops,

resume from checkpoint.

---

## Layer 4 — Transport

Most important GATE layer.

Responsibilities

- End-to-end delivery
    
- Reliability
    
- Error recovery
    
- Flow control
    
- Segmentation
    
- Multiplexing
    

Protocols

TCP

UDP

Address

Port Number

PDU

Segment

---

## Layer 3 — Network

Responsibilities

Routing

Logical Addressing

Path Selection

Fragmentation

Protocol

IP

Address

IP Address

Device

Router

PDU

Packet

---

## Layer 2 — Data Link

Responsibilities

Node-to-node delivery

Framing

MAC Addressing

Error Detection

Flow Control

Medium Access

Protocols

Ethernet

PPP

HDLC

Address

MAC Address

Device

Switch

PDU

Frame

---

## Layer 1 — Physical

Responsibilities

Transmission of bits

Voltage

Frequency

Cable

Wireless Signals

Bit Synchronization

PDU

Bits

Device

Hub

Repeater

---

# 4. PDU Table (VERY IMPORTANT)

|Layer|PDU|
|---|---|
|Application|Data|
|Presentation|Data|
|Session|Data|
|Transport|Segment|
|Network|Packet|
|Data Link|Frame|
|Physical|Bits|

Common PYQ.

---

# 5. Device Mapping

|Device|Layer|
|---|---|
|Hub|Physical|
|Repeater|Physical|
|Bridge|Data Link|
|Switch|Data Link|
|Router|Network|
|Gateway|Upper Layers|

Gateway

Can work across multiple layers.

---

# 6. Addressing at Every Layer

One of GATE's favorites.

|Layer|Address|
|---|---|
|Transport|Port Number|
|Network|IP Address|
|Data Link|MAC Address|
|Physical|No Address|

Remember

Application has names

Presentation has no addressing

Session has no addressing

---

# 7. Encapsulation

Sender

```
Application Data

↓

Transport adds Header

↓

Network adds Header

↓

Data Link adds Header + Trailer

↓

Physical converts to Bits
```

Representation

```
[TCP H]

↓

[IP H][TCP H]

↓

[MAC H][IP H][TCP H][Trailer]
```

Physical

```
101010101010...
```

---

# 8. Decapsulation

Reverse.

```
Bits

↓

Frame

↓

Packet

↓

Segment

↓

Application Data
```

Each layer removes only **its own header**.

---

# 9. Peer-to-Peer Communication

Logical communication.

```
Application ↔ Application

Transport ↔ Transport

Network ↔ Network

```

Reality

Data only moves

```
↓

↓

↓

through lower layers
```

Peer communication is

Logical

NOT Physical

GATE Trap.

---

# 10. Headers

Each layer adds

```
Header
```

except

Data Link

adds

Header

AND

Trailer

Physical

adds nothing.

---

# 11. SAP (Service Access Point)

One of the least understood topics.

Definition

Interface where one layer requests services from the lower layer.

Think

Restaurant

```
Customer

↓

Waiter

↓

Kitchen
```

Customer never enters kitchen.

Waiter = SAP

Similarly

Transport uses Network via SAP.

Examples

Port Numbers

are Transport SAPs.

---

# 12. Service vs Protocol

Very common PYQ.

Service

"What a layer provides."

Protocol

"How two peer layers communicate."

Example

```
Transport

provides

Reliable Delivery

(Service)

using

TCP

(Protocol)
```

---

# 13. OSI vs TCP/IP

|OSI|TCP/IP|
|---|---|
|7 Layers|4 Layers|
|Reference Model|Protocol Suite|
|ISO|DARPA|
|Theoretical|Practical|
|Strict Layering|Flexible Layering|

TCP/IP Layers

```
Application

Transport

Internet

Network Access
```

---

# 14. Layer Mapping

OSI

```
Application

Presentation

Session
```

↓

TCP/IP

```
Application
```

OSI

```
Transport
```

↓

TCP/IP

```
Transport
```

OSI

```
Network
```

↓

TCP/IP

```
Internet
```

OSI

```
Data Link

Physical
```

↓

TCP/IP

```
Network Access
```

---

# 15. OSI Advantages

Modularity

Standardization

Interoperability

Easy troubleshooting

Vendor independence

---

# 16. Layer Responsibilities (One-line Revision)

Physical

Move bits.

Data Link

Move frames.

Network

Find route.

Transport

Reliable delivery.

Session

Maintain conversation.

Presentation

Translate data.

Application

Provide user services.

---

# 17. Flow of Communication

```
Sender

Application

↓

Presentation

↓

Session

↓

Transport

↓

Network

↓

Data Link

↓

Physical

==================

Physical

↓

Data Link

↓

Network

↓

Transport

↓

Session

↓

Presentation

↓

Application

Receiver
```

---

# 18. GATE Traps

### Trap 1

OSI is NOT implemented.

TCP/IP is implemented.

---

### Trap 2

Logical communication

≠

Physical communication.

---

### Trap 3

Switch

works at Layer 2

NOT Layer 3.

---

### Trap 4

Router

uses

IP Address

NOT MAC Address.

---

### Trap 5

TCP

belongs to

Transport Layer

NOT Network.

---

### Trap 6

HTTP

Application Layer

NOT Transport.

---

### Trap 7

Encryption

Presentation Layer.

---

### Trap 8

Port Number

Transport Layer.

---

### Trap 9

MAC Address

changes hop-by-hop.

IP Address

remains same end-to-end (ignoring NAT).

---

### Trap 10

Data Link

adds Trailer.

Others don't.

---

### Trap 11

Physical Layer

has no addressing.

---

### Trap 12

Gateway

can operate across multiple layers.

---

### Trap 13

Session Layer

does NOT provide reliability.

Transport does.

---

### Trap 14

Network Layer

does NOT recover lost packets.

Transport handles end-to-end recovery.

---

### Trap 15

Application Layer

is not the application software itself.

It provides network services to applications.

---

# 19. PYQ Pattern Recognition

Expect questions like:

- Match protocols to layers (HTTP, TCP, IP, Ethernet).
    
- Identify the correct PDU at a given layer.
    
- Match devices (Hub, Switch, Router, Gateway) to OSI layers.
    
- Determine which layer is responsible for encryption, routing, flow control, or framing.
    
- Compare OSI and TCP/IP (number of layers, origin, practical vs reference model).
    
- Identify which headers are added during encapsulation and in what order.
    
- Distinguish logical peer communication from the actual physical path.
    
- Identify the address used at each layer (Port, IP, MAC).
    
- Differentiate **service** from **protocol**.
    
- Conceptual questions on SAP and inter-layer interaction.
    

---

# 20. Ultra-Fast Revision Sheet (Last 5 Minutes Before Exam)

### Layer Mnemonic

> **All People Seem To Need Data Processing**

- Application
    
- Presentation
    
- Session
    
- Transport
    
- Network
    
- Data Link
    
- Physical
    

### PDU

- Data
    
- Segment
    
- Packet
    
- Frame
    
- Bits
    

### Addresses

- Port → Transport
    
- IP → Network
    
- MAC → Data Link
    

### Devices

- Hub → Physical
    
- Repeater → Physical
    
- Bridge → Data Link
    
- Switch → Data Link
    
- Router → Network
    
- Gateway → Multiple/Upper Layers
    

### Encapsulation Order

```
Application Data
→ TCP Header
→ IP Header
→ MAC Header + Trailer
→ Bits
```

### TCP/IP Mapping

- OSI Application + Presentation + Session → TCP/IP Application
    
- Transport → Transport
    
- Network → Internet
    
- Data Link + Physical → Network Access
    

### Remember These One-Liners

- **OSI is a reference model; TCP/IP is a protocol suite.**
    
- **Each layer serves the layer above and uses the layer below.**
    
- **Peer communication is logical; actual transmission is vertical through the stack.**
    
- **Only the Data Link layer adds a trailer.**
    
- **MAC addresses change at every hop; IP addresses normally remain end-to-end.**
    
- **Reliability is an end-to-end Transport layer function; routing is a Network layer function.**
    

If you can answer questions on **layer responsibilities, PDUs, devices, addressing, encapsulation/decapsulation, OSI vs TCP/IP, services vs protocols, peer communication, and SAP**, you've essentially covered the high-yield portion of this GATE chapter.