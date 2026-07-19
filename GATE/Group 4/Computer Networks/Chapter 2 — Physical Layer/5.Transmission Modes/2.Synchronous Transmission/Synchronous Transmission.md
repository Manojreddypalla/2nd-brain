
# Synchronous Transmission (GATE Revision)

## Definition

- Data is transmitted **as a continuous stream of bits**.
- Sender and receiver are **synchronized using a common clock**.
- Data is sent in **frames (blocks)** instead of individual characters.
- **No Start or Stop bits** are used for each character.

---

## Frame Structure

```
| Header | Data | Trailer |
```

Typical Frame:

```
| Sync/Header | Payload (Multiple Characters) | Error Detection (CRC) |
```

---

## Synchronization

- Sender and receiver use a **common clock**.
- Clock synchronization is established **before transmission**.
- Receiver knows exactly when to sample each bit.
- No re-synchronization is needed for every character.

---

## Frame Transmission

- Multiple characters are grouped into **one frame**.
- Entire frame is transmitted continuously.
- No gaps between characters inside a frame.
- Frame boundaries are identified using **special synchronization bits/bytes** or protocol headers.

---

## Characteristics

- Uses a shared clock.
- Frame-oriented transmission.
- High transmission speed.
- High efficiency.
- Low overhead.
- More complex hardware.

---

## Advantages

- Higher data rate.
- Better bandwidth utilization.
- No Start/Stop bit overhead.
- Suitable for continuous data transmission.

---

## Disadvantages

- Requires clock synchronization.
- More complex implementation.
- Not efficient for transmitting very small amounts of data.

---

## Applications

- Ethernet
- HDLC
- SDLC
- SPI
- I2C
- High-speed networks

---

## Asynchronous vs Synchronous

| Feature | Asynchronous | Synchronous |
|---------|--------------|-------------|
| Clock | No common clock | Common clock |
| Unit | Character | Frame |
| Start Bit | Required | Not Required |
| Stop Bit | Required | Not Required |
| Efficiency | Low | High |
| Speed | Low | High |
| Overhead | High | Low |
| Complexity | Low | High |

---

## GATE Points ⭐

- **Common clock is required.**
- **Frame-based transmission.**
- **No Start/Stop bits for each character.**
- **Higher efficiency and speed.**
- **Lower transmission overhead.**

---

## Memory Trick 🧠

**Synchronous = Same Clock**

**Same Clock → Frames → No Start/Stop → High Speed**

---

## One-Line Revision

**Synchronous transmission sends data continuously in frames using a common clock, resulting in higher speed, lower overhead, and better bandwidth utilization.**
