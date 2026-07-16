Here's a **clean, GATE-oriented short note** for your Obsidian.

---

# 📄 OSI Model – Layer 4: Transport Layer

## 🎯 Purpose

Provides **End-to-End (Host-to-Host) Communication** between applications.

Think of it as the **Shipping Manager** of the OSI model.

---

## 💡 Core Idea

Prepares data for transmission by:

- Segmenting data
    
- Ensuring reliable delivery
    
- Delivering data to the correct application
    

---

## ⚙ Functions

- End-to-End Communication
    
- Segmentation
    
- Reassembly
    
- Reliability (TCP)
    
- Sequencing
    
- Flow Control
    
- Error Control
    
- Multiplexing & Demultiplexing
    

---

## 🌍 Real-World Example

Downloading a large file:

- Split into segments
    
- Sent over the network
    
- Missing segments retransmitted (TCP)
    
- Reassembled at the receiver
    

---

## 🌐 Protocols

- **TCP (Transmission Control Protocol)** – Reliable, connection-oriented.
    
- **UDP (User Datagram Protocol)** – Fast, connectionless.
    
- **SCTP (Stream Control Transmission Protocol)** _(less common)_
    
- **DCCP (Datagram Congestion Control Protocol)** _(less common)_
    

---

## 📦 PDU

**Segment** _(TCP)_

**Datagram** _(UDP)_

---

## 📍 Address Used

**Port Number**

Examples:

- HTTP → 80
    
- HTTPS → 443
    
- SSH → 22
    
- FTP → 21
    
- DNS → 53
    

---

## 🚫 Does NOT Handle

- Routing
    
- Path Selection
    
- MAC Addressing
    
- Framing
    
- Physical Transmission
    

---

## 🎯 GATE Focus

- TCP vs UDP
    
- End-to-End Communication
    
- Port Numbers
    
- Segmentation & Reassembly
    
- Flow Control
    
- Error Control
    
- Multiplexing & Demultiplexing
    

---

## ⚠ Common Mistakes

- ❌ Transport Layer performs routing.
    
- ❌ TCP and IP are the same.
    
- ❌ UDP is always unreliable and useless.
    

---

## 📝 One-Line Revision

> **Layer 4 (Transport Layer) provides end-to-end communication by segmenting data, ensuring reliable delivery (TCP), controlling flow and errors, and delivering data to the correct application using port numbers.**