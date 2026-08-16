Absolutely. I went through **Lecture 6 (97 pages)** and kept the notes aligned with the lecture rather than replacing it with generic CN material. The lecture moves through **Switching → Circuit Switching → Packet Switching → Virtual Circuits/Datagrams → Physical Layer → Link Layer → Framing → Byte/Bit Stuffing**.

# Lecture 6 — Switching, Physical Layer & Framing

---

# 1. Switching Networks

### Why switching?

Long-distance communication is typically done through a **network of switched nodes**.

Instead of having a direct physical connection between every pair of hosts, data travels through intermediate **switching nodes**.

```text
A ── Switch ── Switch ── B
```

The main switching approaches are:

```text
                    Switching
                       │
             ┌─────────┴─────────┐
             │                   │
      Circuit Switching    Packet Switching
                                  │
                         ┌────────┴────────┐
                         │                 │
                  Virtual Circuit      Datagram
```

This classification is shown early in the lecture.

---

# 2. Circuit Switching

## Core idea

**Reserve a complete communication path before sending data.**

Think of a traditional telephone call.

```text
A ── S1 ── S2 ── S3 ── B
       ← reserved path →
```

Once the connection is established, the resources along that path are reserved for that session.

### Important characteristics

- Connection/setup is required before data transmission.
    
- A dedicated path is established.
    
- Data is sent **continuously**.
    
- Bandwidth is guaranteed for the lifetime of the session.
    
- Resources remain reserved even when the sender has nothing to send.
    

The lecture compares this with the original telephone system, where a continuous circuit existed between sender and receiver.

---

## Circuit Switching Process

```text
1. Establish connection
        ↓
2. Reserve resources
        ↓
3. Send data continuously
        ↓
4. Release connection
```

### If setup fails?

The session is **refused**.

Example:

> Phone gives a busy signal because the required resources are unavailable.

---

## Main disadvantage

If the application is **bursty**, resources are wasted.

Example:

Suppose you have a 10 Mbps reserved connection.

```text
Time → →

Data: ████        ████             ███
      ↑           ↑                ↑
   actually    actually         actually
     used        used             used
```

During the gaps, the reserved bandwidth is unused.

Yet nobody else can use it.

---

# 3. Packet Switching

## Core idea

Instead of reserving the entire path:

> **Break data into packets and share the network links.**

```text
Message
   ↓
┌────┬────┬────┬────┐
│ P1 │ P2 │ P3 │ P4 │
└────┴────┴────┴────┘
```

Packets are transmitted **when they are ready**.

The links are shared between users.

---

## Why packet switching?

Circuit switching:

```text
Reserve → Send → Release
```

Packet switching:

```text
Send packet → share link → send next packet
```

Therefore packet switching uses bandwidth more efficiently, especially for **bursty traffic**.

---

# 4. What happens if a link is full?

Packet switching does **not** reserve the link exclusively.

If the outgoing link is busy:

```text
Packets
 ↓ ↓ ↓
[Router]
    │
    └──→ Full link
```

Packets are placed into a **queue**.

### If queue becomes full?

The packet may be **dropped**.

```text
Queue:
[P][P][P][P][P]  ← full

New packet → ❌ DROP
```

This is one of the fundamental differences from circuit switching.

---

# 5. Two Forms of Packet Switching

Packet switching has two approaches:

```text
Packet Switching
       │
 ┌─────┴─────┐
 │           │
Virtual     Datagram
Circuit
```

---

# 6. Datagram Approach

## Core idea

Each packet is treated independently.

The packet contains the **destination address**.

```text
Packet:
┌───────────────┬───────────────┐
│ Destination   │     Data      │
│    Address    │               │
└───────────────┴───────────────┘
```

Every router decides where to send the packet.

### No setup

There is **no connection establishment** before sending packets.

```text
Data
 ↓
Packetize
 ↓
Send immediately
```

### Different packets can take different paths

```text
P1: A → B → C → E

P2: A → B → D → E

P3: A → F → D → E
```

This makes datagram networks more tolerant of failures.

---

# 7. Virtual Circuit Approach

Virtual Circuit (VC) switching is a **hybrid of circuit switching and packet switching**.

### It combines:

**Circuit switching idea**  
→ setup phase

**Packet switching idea**  
→ packets are still sent over shared links.

---

## VC operation

### Step 1 — Setup

A route is established before packets are sent.

```text
Source ───────────→ Destination
          setup
```

The setup request travels through switches.

Each switch creates an entry in its forwarding table.

---

### Step 2 — Data transfer

After setup, packets follow the established virtual circuit.

But the path is **not necessarily physically dedicated**.

Different virtual circuits can share the same physical links.

---

### Step 3 — Teardown

A clear/release request removes the virtual circuit.

```text
Setup
  ↓
Data transfer
  ↓
Teardown
```

---

# 8. VC Identifier

A packet belonging to a VC carries a **VC number/identifier**, rather than carrying the complete destination address for routing.

```text
┌────────────┬─────────┐
│ VC Number  │  Data   │
└────────────┴─────────┘
```

Each switch uses its forwarding table to determine the outgoing port and possibly the new VC number.

---

# 9. VC Forwarding Table

A switch can have entries like:

|Incoming Port|Incoming VC|Outgoing Port|Outgoing VC|
|--:|--:|--:|--:|
|1|14|3|22|
|1|77|2|41|

Suppose a packet arrives:

```text
Incoming:
Port = 1
VC = 14
```

The switch looks it up:

```text
(1,14) → (3,22)
```

So the packet leaves:

```text
Port 3
VC 22
```

The lecture emphasizes that **every physical link can carry multiple virtual circuits**.

---

# 10. Circuit Switching vs Virtual Circuit

This distinction is extremely important.

|Circuit Switching|Virtual Circuit|
|---|---|
|Setup required|Setup required|
|Dedicated resources/path|Links can be shared|
|Continuous data model|Packet-based|
|Resource reservation|Usually no complete physical reservation|
|Failure can break circuit|VC may be re-established|

### Mental model

**Circuit switching**

> "This road belongs to me."

**Virtual circuit**

> "I have a known route, but the road is shared."

---

# 11. VC Failure

What happens if a link in a VC fails?

```text
A ── B ── ❌ ── C ── D
```

The virtual circuit may need to be **re-established**.

Possible recovery:

- Router near the failure can potentially repair/catch up the link.
    
- Original host/router can establish a new VC.
    
- Or the VC can be dropped and recovery can be handled by a higher layer.
    

---

# 12. Datagram vs Virtual Circuit — Important Comparison

|Feature|Datagram|Virtual Circuit|
|---|---|---|
|Setup|❌ No|✅ Yes|
|Packet route|Independent|Established route|
|Header|Destination address|VC identifier|
|Routing decision|For each packet|During setup|
|Failure handling|Can reroute packets|VC may need re-establishment|
|Resource reservation|No|May allocate resources|
|Shared links|Yes|Yes|

---

# 13. Why Datagram is Better When Nodes Fail Frequently?

Suppose:

```text
A ── B ── C ── D
          ❌
```

### VC

The established circuit passes through C.

So failure of C can destroy the entire VC.

### Datagram

Packets can potentially use another route:

```text
A ── B ── C ❌

A ── E ── F ── D
```

Therefore:

> **Datagrams are preferable because packets can be routed around problem areas, whereas a node failure can wipe out an entire virtual circuit.**

This is explicitly given as the answer in the lecture.

---

# 14. Important T/F

### Statement

> Virtual Circuit Switching requires a circuit establishment phase before any data can be sent.

**Answer: TRUE**

Because VC switching has a setup/handshaking phase before data transmission.

---

# 15. Why VC Packets Don't Need Source/Destination Addresses?

Once the VC has been established, the switches already know:

```text
Incoming port + VC ID
          ↓
Outgoing port + VC ID
```

Therefore there is no need to make a complete routing decision for every packet.

The lecture explicitly notes that routing is fixed along the established route.

---

# 16. Why Internet Uses Packet Switching

Internet applications are usually **bursty**.

Examples:

### Email

Bandwidth is used when:

```text
Send email
    ↓
data transmission
```

Then mostly nothing.

### Web browsing

Bandwidth is used when you request/load data.

Therefore reserving a dedicated circuit would waste resources.

Packet switching lets many users share the same links.

> **Packet switching is more efficient for bursty applications.**

---

# 17. Circuit vs Packet Switching — Big Picture

|Circuit Switching|Packet Switching|
|---|---|
|Dedicated resources|Shared resources|
|Setup required|No setup in datagram|
|Continuous data|Data divided into packets|
|Predictable service|Variable delay|
|Wastes bandwidth for bursty traffic|Efficient for bursty traffic|
|Busy → connection refused|Busy → packet queued/dropped|

### Easy analogy

**Circuit switching = booking an entire highway lane**

**Packet switching = everyone shares the highway**

---

# 18. Physical Layer

Now the lecture moves upward from switching to the **Physical Layer**.

The physical layer is concerned with:

> **How bits are represented and transmitted as signals over a physical medium.**

```text
Bits
 ↓
Signal
 ↓
Physical medium
 ↓
Signal
 ↓
Bits
```

---

# 19. Scope of Physical Layer

The physical layer concerns how **signals are used to transfer bits over a link**.

For example:

```text
10110
 ↓
electrical / optical / radio signal
 ↓
10110
```

---

# 20. Representation of Bits

Bits cannot simply travel through a physical cable as abstract `0` and `1`.

They must be encoded into a physical signal.

Examples:

```text
1 → +1V
0 → -1V
```

or optical/radio representations.

The exact representation depends on the medium and technology.

---

# 21. Bit Length / Data Rate

The physical layer also determines:

- How long a bit lasts.
    
- How many bits can be transmitted per second.
    

Example:

```text
Data rate = 1 Mbps

1 second → 1,000,000 bits
```

The lecture notes that different media can have different characteristics, such as copper, coaxial cable and fiber optics.

---

# 22. Coding and Modulation

Question:

> How do we send information across a link?

The lecture introduces:

### Coding

Represent bits using signals.

### Modulation

Modify a carrier signal to represent information.

A **modem** comes from:

> **MO**dulator + **DEM**odulator

---

# 23. Types of Physical Media

Three major types discussed:

```text
Physical Media
     │
 ┌───┼─────────┐
 │   │         │
Wire Fiber   Wireless
```

### Wire

Electrical signals.

### Fiber

Optical signals.

### Wireless

Electromagnetic/radio signals.

Media propagate the signals carrying bits.

---

# 24. Why Physical Layer is Different for Every Network

Different networks can use different physical media and technologies.

For example:

```text
Fiber network ≠ Wi-Fi network
```

The method used to encode/transmit information over fiber is different from wireless transmission.

So the physical layer is closely tied to the underlying medium.

---

# 25. Moving to the Link Layer

The lecture then moves upward:

```text
Application
Transport
Network
Link       ← NOW
Physical
```

---

# 26. Link Layer

The physical layer gives us a **stream of bits**.

But there's a problem:

```text
101101001010110100101...
```

How does the receiver know where one frame ends and another starts?

That's the job of **framing** at the link layer.

---

# 27. Network Adapter / NIC

The link layer is implemented in the **network adapter**, commonly called:

> **NIC — Network Interface Card**

The adapter receives a datagram from the network layer and creates a **frame**.

```text
Network layer
    │
 Datagram
    ↓
   NIC
    │
 Frame
    ↓
Physical medium
```

---

# 28. Packet vs Frame

This distinction is important.

### Network layer

Works with:

```text
Packet / Datagram
```

### Link layer

Works with:

```text
Frame
```

Conceptually:

```text
Network-layer packet
┌────────────────────┐
│       Packet       │
└────────────────────┘
          ↓
Link layer adds framing information
          ↓
┌───────┬────────────┬───────┐
│Header │   Packet   │Trailer│
└───────┴────────────┴───────┘
              Frame
```

---

# 29. Functions of Data Link Layer

The lecture gives four major functions:

1. **Framing**
    
2. **Error control**
    
3. **Flow control**
    
4. **Link access / MAC sublayer**
    

---

# 30. Framing

## Core problem

Physical layer gives:

```text
101101001011010010...
```

Receiver needs:

```text
Frame 1 | Frame 2 | Frame 3
```

So:

> **Framing = dividing a continuous bit stream into identifiable frames.**

---

# 31. Error Control

Errors can occur during transmission.

Example:

```text
Sender:   10110110
Receiver: 10100110
             ↑
           error
```

The link layer can detect and/or correct errors.

Methods mentioned in the lecture include:

- Parity checks
    
- Checksum
    
- CRC — Cyclic Redundancy Code
    

---

# 32. Flow Control

Flow control prevents a fast sender from overwhelming a slower receiver.

Methods listed:

- Stop-and-Wait
    
- Go-Back-N
    
- Selective Repeat
    

These are ARQ-related mechanisms.

---

# 33. Link Access / MAC

When multiple devices share a medium, they need rules for deciding **who can transmit**.

Examples from the lecture:

- ALOHA
    
- CSMA-CD
    
- Polling
    

This is handled by the **MAC sublayer**.

---

# 34. Framing — Why Is It Needed?

Imagine receiving:

```text
101101101001011011010...
```

Without frame boundaries:

> Where does the first frame end?

> Where does the next frame begin?

Therefore the receiver must identify:

```text
START | DATA | END
```

---

# 35. Methods of Identifying Frames

The lecture introduces different framing methods.

One simple method is **time gaps**.

```text
Frame 1      Frame 2
████████      ████████

       gap
       ↑
```

But timing gaps are not reliable because the network may eliminate/compress gaps during transmission.

Therefore we need better techniques.

---

# 36. Fixed-Length Framing

Every frame has the same size.

Example:

```text
[FRAME][FRAME][FRAME][FRAME]
  5B     5B     5B     5B
```

If the receiver knows the fixed frame size, it automatically knows the boundaries.

### Advantage

Simple.

### Disadvantage

If actual data sizes vary, fixed-size frames can be inefficient.

The lecture illustrates the tradeoff: choosing a very small frame creates overhead for long data, while very large frames can be inefficient for small data.

---

# 37. Variable-Length Framing

Frames can have different lengths.

```text
[START | DATA | END]
[START | DATA | END]
```

Now the receiver needs some mechanism to determine where the frame starts and ends.

The lecture presents:

- Byte count
    
- Byte stuffing
    
- Bit stuffing
    

---

# 38. Byte Count

## Idea

Put the length of the frame at the beginning.

```text
┌────────┬──────────────┐
│ Length │ Frame data   │
└────────┴──────────────┘
```

Example:

```text
5 | DATA
5 | DATA
8 | DATA
8 | DATA
```

The receiver reads the length and knows how many bytes belong to the frame.

---

# 39. Byte Count — Example

Suppose:

```text
53 | Frame contents
21 | Frame contents
```

The receiver interprets:

```text
53 → next 53 bytes belong to frame
21 → next 21 bytes belong to next frame
```

So the length field tells the receiver where the frame ends.

---

# 40. Problem with Byte Count

What if the length field itself becomes corrupted?

Original:

```text
53 | Frame contents | 21 | Frame contents
```

Suppose `53` becomes `58`.

Receiver thinks:

```text
58 bytes belong to Frame 1
```

Now the receiver consumes part of the next frame.

```text
Frame 1
└───────────────┐
                ↓
       accidentally consumes
       next frame data
```

This can destroy synchronization for subsequent frames.

The lecture explicitly demonstrates this failure case.

---

# 41. Byte Stuffing

To solve framing ambiguity, special **FLAG** bytes can mark frame boundaries.

```text
FLAG | DATA | FLAG
```

So:

```text
FLAG = START/END marker
```

The receiver knows that a FLAG indicates a frame boundary.

---

# 42. Problem with FLAG

What if the actual data contains the FLAG byte?

Example:

```text
FLAG | A | FLAG | B | FLAG
```

Receiver cannot know whether the middle FLAG is:

- actual data, or
    
- frame boundary.
    

We need a way to say:

> "This FLAG is data, not a delimiter."

---

# 43. Escape Character — ESC

Use an **ESC** character to distinguish a FLAG appearing inside data.

Rule:

```text
FLAG in data → ESC FLAG
```

And:

```text
ESC in data → ESC ESC
```

These are the exact byte-stuffing rules presented in the lecture.

---

# 44. Byte Stuffing Example

Original data:

```text
A FLAG B
```

After byte stuffing:

```text
A ESC FLAG B
```

Receiver sees:

```text
ESC FLAG
```

and understands:

> This FLAG is data.

Not a frame boundary.

---

# 45. What if ESC itself occurs in Data?

Suppose:

```text
A ESC B
```

We cannot leave it as-is because ESC has special meaning.

So:

```text
ESC → ESC ESC
```

Result:

```text
A ESC ESC B
```

Receiver removes one ESC and recovers:

```text
A ESC B
```

---

# 46. Byte Stuffing Rules — Remember This

```text
If DATA contains FLAG:
        FLAG → ESC FLAG

If DATA contains ESC:
        ESC → ESC ESC
```

Therefore any unescaped FLAG remaining in the stream is treated as a **frame boundary**.

---

# 47. Bit Stuffing

Byte stuffing works at the **byte level**.

Bit stuffing works at the **bit level**.

The lecture uses:

```text
01111110
```

as the FLAG pattern.

This is an 8-bit flag.

---

# 48. Bit Stuffing Rule

### At sender

Whenever there are **5 consecutive 1s** in the data:

> Insert a `0`.

```text
11111 → 111110
```

### At receiver

Whenever it sees:

```text
111110
```

inside the data:

> Remove the inserted `0`.

---

# 49. Why Bit Stuffing Works

Remember the FLAG:

```text
01111110
```

The data could accidentally contain the same sequence.

So we modify the data to ensure that the FLAG pattern cannot naturally occur.

Suppose data contains:

```text
01111110
```

The sender sees:

```text
11111
```

and inserts `0`:

```text
011111010
```

Now it no longer looks like the FLAG.

---

# 50. Bit Stuffing — Sender

Original:

```text
01111110
```

If this is **data**, after the first five `1`s:

```text
0111110 10
      ↑
    stuffed 0
```

The exact examples in the lecture demonstrate insertion of a `0` after every sequence of five consecutive `1`s.

---

# 51. Bit Stuffing — Receiver

Receiver sees:

```text
111110
```

It knows:

```text
11111 + stuffed 0
```

So it removes the `0`.

```text
111110
   ↓
11111
```

Thus:

```text
Sender:
data → stuffing → transmission

Receiver:
transmission → unstuffing → original data
```

---

# 52. Bit Stuffing Example

Suppose:

```text
Data:
011011111111001
```

Scan from left to right.

Whenever you encounter:

```text
11111
```

insert:

```text
0
```

So the transmitted stream becomes longer.

At the receiver, the inserted zeros are removed.

---

# 53. Byte Stuffing vs Bit Stuffing

||Byte Stuffing|Bit Stuffing|
|---|---|---|
|Unit|Byte|Bit|
|Special delimiter|FLAG byte|FLAG bit pattern|
|Escape mechanism|ESC|Insert `0`|
|Sender rule|FLAG → ESC FLAG|After 5 ones → insert 0|
|Receiver rule|Remove ESC|Remove stuffed 0|

### Mental shortcut

**Byte stuffing:**

> "Special byte? Escape it."

**Bit stuffing:**

> "Too many 1s? Insert 0."

---

# 54. Framing Summary

```text
Framing
   │
   ├── Fixed length
   │
   └── Variable length
          │
          ├── Byte Count
          │
          ├── Byte Stuffing
          │
          └── Bit Stuffing
```

---

# 55. Byte Count vs Stuffing

### Byte Count

```text
[length][data]
```

Problem:

> Corrupted length field can destroy synchronization.

### Byte Stuffing

```text
[FLAG][data][FLAG]
```

Problem:

> FLAG may appear inside data.

Solution:

```text
FLAG → ESC FLAG
ESC  → ESC ESC
```

### Bit Stuffing

```text
FLAG = 01111110
```

Problem:

> Flag pattern may appear in data.

Solution:

```text
5 consecutive 1s → insert 0
```

---

# 56. Homework Pattern from Lecture

The lecture gives a framing problem with:

```text
A     = 01000111
B     = 11100011
FLAG  = 01111110
ESC   = 11100000
```

You may be asked to produce the transmitted bit sequence using:

1. Byte count
    
2. Flag bytes + byte stuffing
    
3. Starting/ending flag bytes + bit stuffing
    

### How to approach these questions

Don't memorize the final sequence.

Use the transformation rules:

```text
Byte count:
[length][A][B]

Byte stuffing:
FLAG → ESC FLAG
ESC  → ESC ESC

Bit stuffing:
after every 5 consecutive 1s
insert 0
```

---

# 57. GATE Mental Map

The entire lecture can be compressed into this:

```text
SWITCHING
│
├── Circuit Switching
│     ├── Setup
│     ├── Dedicated resources
│     └── Continuous transmission
│
└── Packet Switching
      │
      ├── Datagram
      │     ├── No setup
      │     ├── Destination address
      │     └── Independent routing
      │
      └── Virtual Circuit
            ├── Setup
            ├── VC identifier
            ├── Established route
            └── Shared physical links
```

Then:

```text
PHYSICAL LAYER
│
├── Bits → Signals
├── Coding / Modulation
└── Media
      ├── Wire
      ├── Fiber
      └── Wireless
```

Then:

```text
DATA LINK LAYER
│
├── Framing
├── Error Control
├── Flow Control
└── Link Access / MAC
```

And framing:

```text
FRAMING
│
├── Fixed length
│
└── Variable length
      ├── Byte Count
      ├── Byte Stuffing
      └── Bit Stuffing
```

This overall progression is also summarized in the lecture's final overview slide.

---

# 58. ⭐ Must-Know Differences

### Circuit vs Packet

> **Circuit = reserve first**  
> **Packet = share resources**

### Datagram vs VC

> **Datagram = each packet is independent**  
> **VC = setup first, then packets follow established VC**

### Packet vs Frame

> **Packet = Network layer**  
> **Frame = Data Link layer**

### Physical vs Link

> **Physical = transmit bits/signals**  
> **Link = organize bits into frames + handle link-level functions**

### Byte vs Bit Stuffing

> **Byte stuffing = ESC special bytes**  
> **Bit stuffing = insert 0 after five 1s**

---

# 59. 🔥 GATE Traps

### Trap 1

**"Virtual circuit uses a dedicated physical path."**

❌ Not necessarily.

VCs can **share physical links**.

---

### Trap 2

**"VC doesn't require setup."**

❌ False.

VC requires a **setup/establishment phase**.

---

### Trap 3

**"Datagram packets always follow the same path."**

❌ False.

Packets are independently routed.

---

### Trap 4

**"Packet switching means packets never wait."**

❌ False.

Packets may be **queued** when a link is busy.

If the queue fills → packets may be **dropped**.

---

### Trap 5

**"Physical layer understands frames."**

❌ No.

Physical layer deals with the **bit stream/signals**.

Framing belongs to the **Data Link Layer**.

---

### Trap 6

**"Byte stuffing inserts a 0 after five 1s."**

❌ That's **bit stuffing**.

Byte stuffing uses **ESC**.

---

### Trap 7

**"Bit stuffing uses ESC."**

❌ No.

Bit stuffing inserts a **0 after five consecutive 1s**.

---

# 60. One-Page Revision Sheet

```text
LECTURE 6 — SWITCHING + PHYSICAL + LINK

SWITCHING
• Circuit switching → setup + dedicated resources
• Packet switching → packets + shared resources

PACKET SWITCHING
• Datagram → no setup, destination address, independent routing
• VC → setup, VC identifier, established route

CIRCUIT
• Continuous data
• Guaranteed bandwidth
• Busy → connection refused

PACKET
• Bursty traffic friendly
• Shared links
• Busy link → queue
• Full queue → drop packet

VC
• Setup required
• VC ID in packet
• Forwarding table
• Shared physical links
• Failure may require VC re-establishment

DATAGRAM
• No setup
• Destination address
• Packets can take different paths
• More tolerant of node failures

PHYSICAL LAYER
• Bits → signals
• Coding + modulation
• Wire / fiber / wireless
• Data rate / bit duration

LINK LAYER
• Implemented in NIC / network adapter
• Packet → Frame
• Framing
• Error control
• Flow control
• MAC/link access

FRAMING
• Fixed length
• Variable length

VARIABLE LENGTH
• Byte count
• Byte stuffing
• Bit stuffing

BYTE COUNT
[length][data]
Problem → corrupted length destroys synchronization

BYTE STUFFING
FLAG in data → ESC FLAG
ESC in data  → ESC ESC

BIT STUFFING
FLAG = 01111110
After 5 consecutive 1s → insert 0
Receiver removes stuffed 0
```

The lecture ends by transitioning from framing to **Link Layer: Error Detection and Correction**, which is the next topic.