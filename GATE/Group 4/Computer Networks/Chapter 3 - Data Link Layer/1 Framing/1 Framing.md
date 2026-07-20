# 3.1 Framing

> [!info] Definition
> **Framing** is the process of dividing a continuous stream of bits into smaller identifiable units called **Frames**, enabling the receiver to determine where each frame begins and ends.

---

# Why is Framing Needed?

Suppose the sender transmits

```
101001010101101010101001010101101001...
```

The receiver also receives

```
101001010101101010101001010101101001...
```

Now the receiver faces two questions:

- Where does **Frame 1** end?
- Where does **Frame 2** begin?

Without boundaries, the receiver sees only one long stream of bits.

Hence, the **Data Link Layer** divides the bit stream into **Frames**.

---

# Real-Life Analogy

Imagine sending a **500-page book** through a courier.

Instead of sending 500 loose pages, the courier divides them into multiple parcels.

Each parcel contains:

- Sender Address
- Receiver Address
- Parcel Number
- Contents

Similarly,

```
Network Layer Packet

        │
        ▼

+---------+
| Frame 1 |
+---------+
| Frame 2 |
+---------+
| Frame 3 |
+---------+
```

Each frame is transmitted independently.

---

# What is a Frame?

A **Frame** is the **Protocol Data Unit (PDU)** of the **Data Link Layer**.

General Frame Format

```
+------------------------------------------------+
| Header | Payload (Data) | Trailer |
+------------------------------------------------+
```

---

# Components of a Frame

## 1. Header

The header contains control information required before processing the data.

Typical fields include

- Source Address
- Destination Address
- Frame Length
- Frame Type
- Sequence Number
- Control Information

---

## 2. Payload

The payload contains the actual data received from the **Network Layer**.

This is the information that must reach the destination.

---

## 3. Trailer

The trailer contains information used to verify the correctness of the received frame.

Typical fields include

- CRC (Cyclic Redundancy Check)
- Frame Check Sequence (FCS)
- Error Detection Information

---

# Frame Delimitation

> [!important]
> **Frame Delimitation** is the process of identifying the **beginning and end of a frame**.

Without frame delimitation, the receiver cannot distinguish one frame from another.

Different framing techniques use different methods.

| Technique | Frame Delimitation Method |
|------------|--------------------------|
| Character Count | Count Field |
| Byte Stuffing | FLAG Character |
| Bit Stuffing | FLAG Bit Pattern (`01111110`) |

---

# Framing vs Frame Delimitation

| Framing | Frame Delimitation |
|----------|-------------------|
| Divides data into frames | Identifies frame boundaries |
| Overall process | Boundary detection technique |

---

# Objectives of Framing

Framing enables the receiver to

- Identify frame boundaries
- Synchronize sender and receiver
- Detect transmission errors
- Deliver complete frames to the Network Layer
- Retransmit only damaged frames

---

# Functions of Framing

### 1. Frame Delimitation

Marks the beginning and end of every frame.

### 2. Synchronization

Maintains synchronization between sender and receiver.

### 3. Error Detection

Each frame can be independently checked using CRC or other techniques.

### 4. Reliable Communication

Only corrupted frames need retransmission.

### 5. Efficient Communication

Large messages are divided into manageable units.

---

# Methods of Framing

## 1. Character Count

The first field stores the total frame length.

```
+----------------+
| Count | Data |
+----------------+
```

---

## 2. Byte Stuffing (Character Stuffing)

Special **FLAG** characters indicate the beginning and end of a frame.

If a FLAG appears inside the data, an **ESC** character is inserted before it.

---

## 3. Bit Stuffing

Special FLAG pattern

```
01111110
```

marks frame boundaries.

Whenever **five consecutive 1s** occur inside the data,

```
11111
```

the sender inserts

```
0
```

The receiver removes the stuffed bit before delivering the data.

---

# Advantages

- Clearly identifies frame boundaries.
- Enables synchronization.
- Supports error detection.
- Improves reliability.
- Allows retransmission of only damaged frames.
- Supports flow control.

---

# Disadvantages

- Header and trailer introduce overhead.
- Stuffing techniques increase frame size.
- Character Count loses synchronization if the count field is corrupted.

---

# Applications

Framing is used in

- Ethernet
- HDLC
- PPP
- IEEE 802.11 (Wi-Fi)
- Serial Communication Protocols

---

# Important GATE Points

> [!important]
> Data Link Layer PDU = **Frame**

> [!important]
> Network Layer PDU = **Packet**

> [!important]
> Transport Layer PDU = **Segment (TCP)** / **Datagram (UDP)**

> [!important]
> Framing is a **Data Link Layer** responsibility.

---

# Memory Trick

Imagine reading

```
hellohowareyouiamfine
```

Very difficult.

Now add punctuation.

```
Hello.
How are you?
I am fine.
```

Frames are the **punctuation marks** of networking.

---

# Summary

- Framing divides a continuous bit stream into Frames.
- A Frame is the PDU of the Data Link Layer.
- Every frame generally consists of Header, Payload, and Trailer.
- Frame Delimitation identifies frame boundaries.
- Common framing techniques:
  - Character Count
  - Byte Stuffing
  - Bit Stuffing

---

# Quick Revision

| Concept | Remember |
|----------|----------|
| PDU | Frame |
| Frame Structure | Header + Payload + Trailer |
| Frame Delimitation | Detect Start & End |
| Character Count | Count Field |
| Byte Stuffing | ESC + FLAG |
| Bit Stuffing | Insert 0 after five consecutive 1s |

---

# Related Topics

- Character Count
- Byte Stuffing
- Bit Stuffing
- CRC
- HDLC
- Ethernet Frame
- Flow Control