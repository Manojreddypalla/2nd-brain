
# Asynchronous vs Synchronous Transmission (GATE Revision)

| Feature | Asynchronous Transmission | Synchronous Transmission |
|----------|---------------------------|--------------------------|
| Clock | No common clock | Common clock required |
| Transmission Unit | Character (Byte) | Frame / Block |
| Synchronization | Using Start & Stop bits | Using shared clock |
| Start Bit | Required | Not Required |
| Stop Bit | Required | Not Required |
| Parity Bit | Optional | Usually not used per character |
| Data Flow | Character by character | Continuous stream of bits |
| Idle Gaps | Allowed | Not allowed within a frame |
| Speed | Lower | Higher |
| Efficiency | Lower | Higher |
| Overhead | High (Start/Stop bits) | Low |
| Complexity | Simple | More complex |
| Hardware Cost | Low | High |
| Error Detection | Parity (optional) | CRC / Checksum (frame level) |
| Best For | Low-speed, intermittent data | High-speed, continuous data |
| Examples | UART, RS-232, Keyboard, Mouse | Ethernet, HDLC, SPI, I²C |

---

## GATE Points ⭐

- **Asynchronous → No Clock → Character → Start & Stop Bits**
- **Synchronous → Common Clock → Frame → No Start & Stop Bits**
- **Asynchronous has higher overhead.**
- **Synchronous has higher efficiency and bandwidth utilization.**
- **Asynchronous allows idle gaps; Synchronous transmits continuously.**

---

## Memory Trick 🧠

### Asynchronous

**A = Alone**
- Each character travels **alone**.
- Needs **Start & Stop** bits.

### Synchronous

**S = Same Clock**
- Devices share the **Same Clock**.
- Data travels in **Frames**.

---

## One-Line Revision

- **Asynchronous:** Character-wise transmission without a common clock using Start and Stop bits.
- **Synchronous:** Frame-wise continuous transmission with a common clock and no Start/Stop bits.
