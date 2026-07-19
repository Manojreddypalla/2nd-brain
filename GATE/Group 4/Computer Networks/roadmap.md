# GATE Computer Networks Roadmap (Final)

---

# Module 0 — Foundations (Prerequisite)

**Goal:** Build the intuition for why networks exist before studying the layers.

## Topics

### Introduction
- Why Networking?
- Components of a Network

### Types of Networks
- LAN
- MAN
- WAN

### Network Topologies
- Bus
- Star
- Ring
- Mesh
- Tree

### Basic Networking Concepts
- Protocol
- Service
- Interface
- Layering

### Data Flow
- Encapsulation
- Decapsulation
- PDU
  - Data
  - Segment
  - Packet
  - Frame
  - Bits

### Network Models (Overview)
- OSI Overview
- TCP/IP Overview

---

**Practice**

- Introductory PYQs (if available)

---

# Module 1 — OSI Layer

**Goal:** Understand the responsibility of each layer.

## Topics

### OSI Model
- OSI Architecture
- Functions of Each Layer

### TCP/IP Model
- TCP/IP Architecture
- Functions

### Comparison
- OSI vs TCP/IP

### Communication
- Peer-to-Peer Communication
- Service Access Point (SAP)

### Addressing
- Physical Address
- Logical Address
- Port Address
- Specific Address

### Encapsulation
- Encapsulation
- Decapsulation (Detailed)

---

**Practice**

- OSI Layer PYQs

---

# Chapter 2 — Physical Layer

---

## 2.1 Transmission Media

### Guided Media
- Twisted Pair Cable (UTP, STP)
- Coaxial Cable
- Optical Fiber

### Unguided Media
- Radio Waves
- Microwaves
- Infrared
- Satellite Communication

---

## 2.2 Signals

### Basic Concepts
- Data vs Signal
- Analog Data vs Digital Data
- Analog Signal vs Digital Signal

### Signal Characteristics
- Amplitude
- Frequency
- Phase
- Wavelength

### Performance Metrics
- Data Rate (Bit Rate)
- Baud Rate
- Bandwidth
- Bit Error Rate (BER)

---

## 2.3 Channel Capacity

- Nyquist Bit Rate Theorem
- Shannon Capacity Theorem
- Nyquist vs Shannon

---

## 2.4 Line Coding

### Introduction
- Why Line Coding?
- Characteristics of Good Line Coding

### Unipolar Encoding
- Unipolar NRZ
- Unipolar RZ

### Polar Encoding
- NRZ-L
- NRZ-I
- Polar RZ
- Manchester Encoding
- Differential Manchester

### Bipolar Encoding
- AMI
- Pseudoternary

### Block Coding
- 4B/5B
- 8B/10B

### Comparison of Encoding Schemes
- Bandwidth Requirement
- Synchronization
- DC Component
- Error Detection Capability
- Advantages & Disadvantages

---

## 2.5 Transmission Modes

### Asynchronous Transmission
- Start Bit
- Stop Bit
- Character Framing

### Synchronous Transmission
- Synchronization
- Frame Transmission

### Comparison
- Asynchronous vs Synchronous

### RS-232 (Basic)
- Voltage Levels
- Start/Stop Bits
- Applications

---

## 2.6 Multiplexing

### Frequency Division Multiplexing (FDM)

### Time Division Multiplexing (TDM)
- Synchronous TDM
- Statistical TDM

### Wavelength Division Multiplexing (WDM) *(Basic)*

---

## 2.7 Switching

### Introduction to Switching

### Circuit Switching

### Message Switching

### Packet Switching

#### Packet Switching Types
- Virtual Circuit Switching
- Datagram Switching

---

## Practice

- GateOverflow Topic-wise PYQs
- Previous 15+ Years GATE PYQs
- ISRO PYQs (Selected)
- Formula Revision

---

# Module 3 — Data Link Layer

## Topics

### Framing
- Character Count
- Byte Stuffing
- Bit Stuffing

---

### Error Detection
- Parity
- Checksum
- CRC

---

### Error Correction
- Hamming Code

---

### Flow Control
- Stop-and-Wait
- Sliding Window

---

### ARQ Protocols
- Stop-and-Wait ARQ
- Go-Back-N ARQ
- Selective Repeat ARQ

---

### Medium Access Control
- Pure ALOHA
- Slotted ALOHA
- CSMA
- CSMA/CD
- CSMA/CA

---

### Ethernet
- Ethernet Frame
- MAC Address
- Ethernet Standards

---

### Bridging
- Transparent Bridge
- Learning Bridge
- Spanning Tree Protocol (Basic)

---

**Practice**

- Data Link Layer PYQs

---

# Module 4 — Network Layer

## Topics

### IP Addressing
- IPv4
- IPv6 *(Basic)*
- Classful Addressing
- CIDR
- Subnetting
- Supernetting

---

### IP Support Protocols
- ARP
- RARP *(Basic)*
- ICMP
- DHCP
- NAT

---

### IP Datagram
- IPv4 Header
- Fragmentation
- Reassembly

---

### Routing

#### Routing Algorithms
- Flooding
- Shortest Path Routing
- Distance Vector Routing
- Link State Routing

#### Routing Protocols
- RIP
- OSPF *(Basic)*

---

**Practice**

- Network Layer PYQs

---

# Module 5 — Transport Layer

## Topics

### UDP
- Features
- Header
- Applications

---

### TCP
- Features
- Header
- Reliable Transmission

---

### Connection Management
- Three-Way Handshake
- Connection Termination

---

### Flow Control

- Sliding Window

---

### Congestion Control
- Slow Start
- Congestion Avoidance
- Fast Retransmit
- Fast Recovery *(Basic)*

---

### Sockets
- Port Numbers
- Socket Concept

---

**Practice**

- Transport Layer PYQs

---

# Module 6 — Application Layer

## Topics

- DNS
- HTTP
- HTTPS *(Basic Understanding)*
- FTP
- SMTP
- POP3
- IMAP
- Email Architecture

---

**Practice**

- Application Layer PYQs

---

# Module 7 — Network Security (Basics)

## Topics

### Cryptography Basics
- Plaintext
- Ciphertext
- Keys

### Encryption
- Symmetric Encryption
- Asymmetric Encryption

### Authentication
- Hash Functions
- Digital Signatures

### Secure Communication
- SSL/TLS
- Firewalls
- VPN
- IPsec *(Overview)*

---

**Practice**

- Network Security PYQs

---

# Module 8 — Revision

## Revision Plan

- Topic-wise Revision
- Formula Sheet
- Comparison Tables
- Previous Year Questions (15+ Years)
- Mock Tests
- Weak Topic Revision

---

# Study Loop

For every module:

1. 📺 Watch Gate Smashers (Concept Building)
2. 📝 Create Obsidian Notes
3. 📄 Create Short Revision Notes
4. 💻 Solve Topic-wise GateOverflow PYQs
5. 🔍 Analyze Mistakes
6. ➜ Move to Next Module