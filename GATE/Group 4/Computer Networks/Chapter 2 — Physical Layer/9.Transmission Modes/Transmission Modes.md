# Chapter 2.5 – Transmission Modes

## Overview

Transmission Mode refers to **how data is transmitted between two devices** over a communication channel.

There are two main transmission modes:

1. **Asynchronous Transmission**
2. **Synchronous Transmission**

---

# 2.5.1 Asynchronous Transmission

## Definition

Asynchronous transmission is a communication method in which **data is transmitted one character (byte) at a time**.

Each character is transmitted **independently**, so the receiver knows where each character starts and ends using **Start** and **Stop** bits.

> **Key Idea:** Character-by-character transmission.

---

## Character Framing

Each transmitted character is enclosed within a frame.

```
+-------+----------+--------+-------+
| Start |  Data    | Parity | Stop  |
+-------+----------+--------+-------+
   1 bit   5–8 bits 0/1 bit 1–2 bits
```

### Components

### 1. Start Bit

- Marks the **beginning** of a character.
- Usually **Logic 0 (LOW)**.
- Alerts the receiver that data is arriving.

---

### 2. Data Bits

Actual information.

Usually:

- 5 bits
- 6 bits
- 7 bits
- 8 bits (most common)

---

### 3. Parity Bit (Optional)

Used for **simple error detection**.

Types:

- Even Parity
- Odd Parity

(Not used in every transmission.)

---

### 4. Stop Bit

Marks the **end** of a character.

Usually:

- Logic 1 (HIGH)

May be:

- 1 Stop Bit
- 1.5 Stop Bits
- 2 Stop Bits

---

## Example

Suppose the ASCII character **A**

```
Data = 01000001
```

Transmission:

```
Start | 01000001 | Stop
```

Receiver detects:

- Start bit
- Reads 8 bits
- Stops after Stop bit

---

## Characteristics

### Advantages

- Simple implementation
- No shared clock required
- Low hardware cost
- Easy to use

---

### Disadvantages

- Extra Start/Stop bits increase overhead
- Lower efficiency
- Lower speed
- Not suitable for large data transfer

---

## Applications

- Keyboard
- Mouse
- UART Communication
- Serial Port
- RS-232

---

# 2.5.2 Synchronous Transmission

## Definition

Synchronous transmission sends **multiple characters together as a frame** instead of sending one character at a time.

There are **no Start and Stop bits for every character**.

> **Key Idea:** Frame-by-frame transmission.

---

## Synchronization

Sender and receiver remain synchronized using a **common clock** or synchronization mechanism.

Since timing is already known,

there is **no need for Start/Stop bits** for each character.

---

## Frame Transmission

Instead of

```
Character
Character
Character
```

Data is sent as

```
+---------+----------------+---------+
| Header  |      Data      | Trailer |
+---------+----------------+---------+
```

A frame may contain hundreds or thousands of bytes.

---

## Components of Frame

### Header

Contains

- Source Address
- Destination Address
- Control Information

---

### Data

Actual user information.

---

### Trailer

Contains

- CRC
- Checksum
- Error Detection Information

---

## Characteristics

### Advantages

- High transmission speed
- Better efficiency
- Less overhead
- Suitable for large data transfer

---

### Disadvantages

- More complex
- Clock synchronization required
- Higher implementation cost

---

## Applications

- Ethernet
- HDLC
- PPP
- High-speed LANs
- WANs

---

# 2.5.3 Comparison

| Feature | Asynchronous | Synchronous |
|----------|--------------|-------------|
| Transmission Unit | Character | Frame |
| Clock | Not Shared | Shared |
| Start Bit | Required | Not Required |
| Stop Bit | Required | Not Required |
| Synchronization | Every Character | Entire Frame |
| Speed | Low | High |
| Efficiency | Low | High |
| Overhead | High | Low |
| Complexity | Low | High |
| Cost | Low | Higher |
| Best For | Small Data | Large Data |

---

# 2.5.4 RS-232 (Basic)

## Definition

RS-232 (**Recommended Standard 232**) is a **serial communication standard** used for communication between a **DTE (Data Terminal Equipment)** and **DCE (Data Communication Equipment)**.

Examples:

- Computer ↔ Modem
- PC ↔ Microcontroller
- PC ↔ Router Console Port

---

## DTE and DCE

### DTE

Device that generates or receives data.

Examples:

- Computer
- Laptop
- Terminal

---

### DCE

Device that provides communication.

Examples:

- Modem
- Communication Equipment

---

## Serial Communication

RS-232 sends data

**One bit at a time**

instead of

Multiple bits simultaneously.

---

## Voltage Levels

Unlike TTL logic,

RS-232 uses **positive and negative voltages**.

| Logic | Voltage |
|--------|----------|
| Logic 1 (Mark) | -3V to -15V |
| Logic 0 (Space) | +3V to +15V |

### Important

Negative Voltage

↓

Logic 1

Positive Voltage

↓

Logic 0

⚠️ This is opposite to TTL logic.

---

## Start and Stop Bits

RS-232 generally uses **Asynchronous Transmission**.

Therefore every character contains

```
Start

↓

Data

↓

(Optional Parity)

↓

Stop
```

---

## Advantages

- Simple
- Cheap
- Reliable
- Easy implementation

---

## Limitations

- Low Speed
- Short Distance
- Noise Sensitive
- Obsolete for modern high-speed communication

---

## Applications

- Serial Port (COM Port)
- Embedded Systems
- Arduino
- UART Communication
- Industrial Equipment
- Router/Switch Console Ports

---

# GATE Notes

## Remember

### Asynchronous

- Character-by-character
- Start Bit
- Stop Bit
- No Shared Clock

---

### Synchronous

- Frame Transmission
- Shared Clock
- No Start/Stop Bits
- Higher Efficiency

---

### RS-232

- Serial Communication Standard
- Computer ↔ Modem
- Logic 1 = Negative Voltage
- Logic 0 = Positive Voltage

---

# GATE Corner

## High Yield Concepts

### Asynchronous

- Character Transmission
- Start Bit = Beginning
- Stop Bit = End
- High Overhead
- Low Speed

---

### Synchronous

- Frame Transmission
- Shared Clock
- No Start/Stop Bits
- Low Overhead
- High Speed

---

### RS-232

- Serial Communication
- DTE ↔ DCE
- Logic 1 → Negative Voltage
- Logic 0 → Positive Voltage

---

# PYQ Focus ⭐⭐⭐⭐

Frequently Asked:

- Asynchronous vs Synchronous
- Start and Stop Bits
- Character Framing
- Frame Transmission
- RS-232 Voltage Levels
- DTE vs DCE
- Serial Communication
- 