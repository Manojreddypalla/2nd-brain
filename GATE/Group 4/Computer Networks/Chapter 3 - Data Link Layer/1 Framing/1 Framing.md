# 3.1 Framing

> [!info] Definition
> **Framing** is the process of dividing a continuous stream of bits into smaller identifiable units called **Frames** so that the receiver can determine where each frame begins and ends.

---

# Why is Framing Needed?

Imagine the sender transmits

```
101001010101101010101001010101101001...
```

The receiver receives

```
101001010101101010101001010101101001...
```

Question:

- Where does Frame 1 end?
- Where does Frame 2 begin?

Without boundaries, the receiver cannot distinguish one message from another.

Hence, the Data Link Layer groups data into **Frames**.

---

# Real-Life Analogy

Suppose a courier has to deliver a 500-page book.

Instead of sending 500 loose pages, the courier divides them into separate parcels.

Each parcel has

- Sender
- Receiver
- Parcel Number
- Contents

Similarly,

```
Network Layer
        │
        ▼
Large Packet
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

A **Frame** is the Data Link Layer Protocol Data Unit (PDU).

General Structure

```
+--------------------------------------+
| Header | Payload(Data) | Trailer |
+--------------------------------------+
```

Example (Ethernet)

```
+-------------------------------------------------------------+
| Destination | Source | Type | Data | CRC |
+-------------------------------------------------------------+
```

---

# Components of a Frame

## Header

Contains control information such as

- Source Address
- Destination Address
- Length
- Sequence Information
- Control Bits

---

## Payload

Contains the actual data received from the Network Layer.

---

## Trailer

Usually contains

- CRC
- Error Detection Bits
- End of Frame Information

---

# Objectives of Framing

Framing allows the receiver to

- Detect frame boundaries
- Synchronize communication
- Perform error detection
- Deliver complete data to the Network Layer
- Retransmit only damaged frames

---

# Functions of Framing

## 1. Frame Delimitation

Marks the beginning and end of every frame.

---

## 2. Synchronization

Keeps sender and receiver aligned.

---

## 3. Error Detection

Each frame can be checked independently using CRC or other techniques.

---

## 4. Reliable Delivery

Only corrupted frames need retransmission.

---

## 5. Efficient Communication

Large messages become manageable pieces.

---

# Methods of Framing

The Data Link Layer uses multiple techniques to identify frame boundaries.

## 1. Character Count

Frame size is stored at the beginning.

```
Length = 20 Bytes

[20][DATA........]
```

---

## 2. Byte Stuffing (Character Stuffing)

Special bytes indicate

- Start
- End

Escape characters are inserted whenever those special bytes appear inside data.

---

## 3. Bit Stuffing

Special bit pattern

```
01111110
```

marks frame boundaries.

Whenever five consecutive 1s occur inside data,

```
11111
```

the sender inserts

```
0
```

The receiver removes the inserted zero.

---

# Advantages

- Easy identification of frame boundaries
- Reliable communication
- Better synchronization
- Easier error detection
- Efficient retransmission
- Supports flow control

---

# Disadvantages

- Additional header and trailer increase overhead.
- Stuffing techniques slightly increase frame size.
- Character Count is vulnerable if the count field becomes corrupted.

---

# Applications

Framing is used in

- Ethernet
- HDLC
- PPP
- Wi-Fi (IEEE 802.11)
- Serial Communication Protocols

---

# Important Points

> [!important]
>
> Framing is a **Data Link Layer** responsibility.

---

> [!important]
>
> Data Link Layer PDU = **Frame**

---

> [!important]
>
> Network Layer PDU = Packet

---

> [!important]
>
> Transport Layer PDU = Segment (TCP) / Datagram (UDP)

---

# Memory Trick

Imagine reading this

```
hellohowareyouiamfine
```

Impossible.

Now add punctuation.

```
Hello.
How are you?
I am fine.
```

Frames are the punctuation marks of networking.

---

# Summary

- Framing divides continuous data into Frames.
- Receiver identifies beginning and end of each Frame.
- Improves synchronization.
- Enables error detection.
- Makes retransmission efficient.

---

# Revision Box

## Definition

Process of dividing a bit stream into Frames.

## Why?

Receiver must know frame boundaries.

## Frame Structure

```
Header | Data | Trailer
```

## Functions

- Delimitation
- Synchronization
- Error Detection
- Reliable Delivery

## Methods

- Character Count
- Byte Stuffing
- Bit Stuffing

## PDU

Frame

---

# Related Topics

- Character Count
- Byte Stuffing
- Bit Stuffing
- CRC
- Ethernet Frame
- HDLC