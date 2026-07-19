# 2.7 Switching

## Introduction to Switching

### Definition

Switching is the process of selecting a communication path through which data travels from a **source** to a **destination**.

Without switching, every device would require a direct connection to every other device, which is impractical.

### Why Switching?

- Efficient communication
- Better resource utilization
- Connects multiple devices
- Reduces the number of physical links
- Enables large-scale networks

---

# Types of Switching

```text
Switching
│
├── Circuit Switching
├── Message Switching
└── Packet Switching
      │
      ├── Virtual Circuit
      └── Datagram
```

---

# Circuit Switching

## Definition

Circuit Switching establishes a **dedicated communication path** between the sender and receiver before data transmission begins.

The path remains reserved until communication ends.

### Phases

1. Connection Setup
2. Data Transfer
3. Connection Release

### Characteristics

- Dedicated path
- Fixed bandwidth
- In-order delivery
- Connection-oriented
- No other user can use the reserved resources

### Advantages

- Guaranteed bandwidth
- No packet loss due to congestion
- Suitable for continuous communication

### Disadvantages

- Setup delay
- Bandwidth wastage when idle
- Poor resource utilization

### Applications

- Traditional Telephone Network (PSTN)

---

# Message Switching

## Definition

Message Switching transmits the **entire message** as one unit.

Each intermediate node stores the complete message before forwarding it to the next node.

This is called the **Store-and-Forward** technique.

### Characteristics

- No dedicated path
- Entire message stored
- High delay
- Large storage required

### Advantages

- Efficient bandwidth utilization
- No connection setup
- Dynamic routing possible

### Disadvantages

- High transmission delay
- Large buffer requirement
- Unsuitable for real-time applications

### Applications

- Telegraph systems (historically)
- Email-like store-and-forward systems

---

# Packet Switching

## Definition

Packet Switching divides a large message into **small packets** before transmission.

Each packet contains:

- Data
- Source Address
- Destination Address
- Sequence Number
- Error Control Information

Packets travel independently through the network.

### Characteristics

- Store-and-Forward
- Efficient bandwidth utilization
- Reliable communication
- Used in modern computer networks

### Advantages

- Better bandwidth utilization
- Fault tolerant
- Multiple users can share the network

### Disadvantages

- Variable delay
- Packet loss possible
- Packet reordering required

### Applications

- Internet
- LAN
- WAN

---

# Packet Switching Types

Packet Switching is classified into:

1. Virtual Circuit Switching
2. Datagram Switching

---

# Virtual Circuit Switching

## Definition

A logical path is established before transmission.

All packets follow the **same path** from source to destination.

### Characteristics

- Connection-oriented
- Fixed route
- Packets arrive in order
- Setup required

### Advantages

- Ordered delivery
- Easier packet reassembly
- Predictable performance

### Disadvantages

- Connection setup required
- Failure of the path interrupts communication

### Examples

- Frame Relay
- ATM

---

# Datagram Switching

## Definition

No dedicated path is established.

Every packet is treated independently and may travel through different routes.

### Characteristics

- Connectionless
- Dynamic routing
- No setup phase
- Packets may arrive out of order

### Advantages

- No setup delay
- Highly flexible
- Robust against network failures

### Disadvantages

- Variable delay
- Out-of-order delivery
- Reassembly required

### Example

- Internet Protocol (IP)

---

# Comparison

| Feature | Circuit | Message | Packet |
|---------|---------|----------|--------|
| Path | Dedicated | No | No |
| Setup Required | Yes | No | VC: Yes / Datagram: No |
| Transmission Unit | Continuous Data | Entire Message | Packets |
| Delay | Low (after setup) | High | Moderate |
| Resource Utilization | Poor | Better | Best |
| Suitable For | Voice | Non Real-Time | Computer Networks |

---

# Virtual Circuit vs Datagram

| Feature | Virtual Circuit | Datagram |
|---------|-----------------|-----------|
| Connection | Connection-Oriented | Connectionless |
| Setup | Required | Not Required |
| Path | Fixed | Dynamic |
| Packet Order | Maintained | Not Guaranteed |
| Delay | Lower | Variable |
| Example | ATM | IP |

---

# Key Points

- Switching selects the path between source and destination.
- Circuit Switching reserves a dedicated path.
- Message Switching sends the entire message using Store-and-Forward.
- Packet Switching divides data into packets.
- Virtual Circuit uses a fixed logical path.
- Datagram allows each packet to choose its own route.
- Modern Internet uses Datagram Packet Switching.
