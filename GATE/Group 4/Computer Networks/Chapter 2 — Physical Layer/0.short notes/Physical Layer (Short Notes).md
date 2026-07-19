# 📌 Computer Networks – Chapter 2: Physical Layer (Short Notes)

---

# 1. Transmission Media

## Guided Media
- Twisted Pair → Cheap, Telephone, Ethernet
- Coaxial Cable → Cable TV
- Optical Fiber → High Speed, Long Distance, Immune to EMI

## Unguided Media
- Radio → Omni-directional
- Microwave → Line of Sight
- Infrared → Short Range
- Satellite → Large Coverage, High Delay

---

# 2. Signal Basics

## Data
Information to be transmitted.

## Signal
Electrical/Optical representation of data.

### Analog Signal
- Continuous
- Infinite values

### Digital Signal
- Discrete
- Binary (0,1)

---

## Signal Parameters

Amplitude → Signal Strength

Frequency → Cycles/sec (Hz)

Phase → Position of wave

Wavelength (λ)

λ = v/f

Bandwidth

Frequency range of channel

Bit Rate

Bits transmitted per second (bps)

Baud Rate

Signal changes per second

Relationship

Bit Rate = Baud × log₂(L)

L = Number of Signal Levels

---

# 3. Channel Capacity

## Nyquist Theorem

Noise-Free Channel

Bit Rate = 2B log₂(L)

B = Bandwidth

L = Signal Levels

---

## Shannon Capacity

Noisy Channel

C = B log₂(1 + S/N)

S/N → Linear

If SNR in dB

SNR = 10^(dB/10)

---

## Difference

Nyquist → Noise-Free

Shannon → Noisy

---

# 4. Line Coding

Purpose

- Digital → Digital Conversion
- Synchronization
- Reduce DC Component

---

## Encoding Rules

NRZ-L

Level = Bit

NRZ-I

Transition = 1

Manchester

Middle Transition = Bit

Differential Manchester

Beginning Transition = Bit

AMI

1's Alternate

Pseudoternary

0's Alternate

---

## Block Coding

4B/5B

4 bits → 5 bits

Efficiency = 80%

8B/10B

8 bits → 10 bits

Efficiency = 80%

---

# 5. Transmission Modes

## Asynchronous

- Character Transmission
- Start Bit
- Stop Bit
- No Shared Clock
- Low Speed

Applications

UART

RS-232

Keyboard

---

## Synchronous

- Frame Transmission
- Shared Clock
- No Start/Stop Bits
- High Speed
- Better Efficiency

Applications

Ethernet

PPP

HDLC

---

## RS-232

Computer ↔ Modem

Logic 1 → Negative Voltage

Logic 0 → Positive Voltage

---

# 6. Multiplexing

## FDM

Resource → Frequency

Communication → Analog

Guard Band → Required

Applications

Radio

TV

---

## TDM

Resource → Time

Communication → Digital

Synchronization Required

---

### Synchronous TDM

Fixed Slots

Idle Slot Wasted

---

### Statistical TDM

Dynamic Slots

Better Utilization

---

## WDM

Resource → Wavelength

Medium → Optical Fiber

Very High Capacity

---

# 7. Switching

## Circuit Switching

Dedicated Path

Connection-Oriented

Telephone Network

---

## Message Switching

Entire Message

Store & Forward

High Delay

---

## Packet Switching

Message → Packets

Store & Forward

Internet

---

### Virtual Circuit

Connection-Oriented

Fixed Path

Ordered Delivery

ATM

Frame Relay

---

### Datagram

Connectionless

Dynamic Path

Unordered Delivery

IP

---

# ⭐ Must Remember

Nyquist → Noise-Free

Shannon → Noisy

Manchester → Middle Transition

Differential Manchester → Beginning Transition

AMI → Ones Alternate

Pseudoternary → Zeros Alternate

FDM → Frequency

TDM → Time

WDM → Wavelength

Circuit → Dedicated Path

Message → Entire Message

Packet → Small Packets

Virtual Circuit → Fixed Path

Datagram → Different Paths

Internet → Datagram Switching

Telephone → Circuit Switching

Optical Fiber → WDM

Guard Band → FDM

Shared Clock → Synchronous

Start/Stop Bits → Asynchronous

RS-232

1 → Negative Voltage

0 → Positive Voltage

---

# 🔥 PYQ Hot Topics

✓ Nyquist Formula

✓ Shannon Formula

✓ Bit Rate vs Baud Rate

✓ Line Coding Comparison

✓ Manchester vs Differential Manchester

✓ AMI vs Pseudoternary

✓ FDM vs TDM

✓ Synchronous vs Statistical TDM

✓ Circuit vs Packet Switching

✓ Virtual Circuit vs Datagram

✓ RS-232 Voltage Levels
