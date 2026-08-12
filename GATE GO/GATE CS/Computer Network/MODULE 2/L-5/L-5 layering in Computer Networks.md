 Absolutely. I went through the **entire 66-page Lecture 5 PDF**, including the handwritten annotations and diagrams. The PDF is image-based, so I’m using the actual page visuals rather than relying only on extracted text.

Below are **full study notes**, following the lecture's order and scope. I’ll keep the explanations deep enough for GATE understanding without adding unrelated material.

# CN — Lecture 5: Subnetting, Protocols & Layering

---

# 1. Revision: Subnetting

## 1.1 Why do we need subnetting?

Suppose an organization has one large network:

```text
University Network
       |
 ┌─────┼─────┐
Engineering  Medical  Library
```

All these networks may be **independently managed**.

If we simply give the entire organization one network, all hosts appear to belong to the same network.

### Problem

We want to divide the large network into smaller logical networks.

### Solution

**Subnetting**

Subnetting adds another level to the IP addressing structure.

```text
Original:

Network Prefix | Host Number


After subnetting:

Network Prefix | Subnet Number | Host Number
```

The combination:

```text
Network Prefix + Subnet Number
```

is called the **extended network prefix**.

This allows us to distinguish different subnets within the same larger network.

---

# 2. What is a subnet?

A **subnet** is a logical subdivision of a larger IP network.

For example, suppose:

```text
University Network
Network: 128.143.0.0/16
```

We can divide it into:

```text
Engineering → 128.143.71.0/24
Medical     → 128.143.56.0/24
Library     → 128.143.121.0/24
```

Each subnet has its own network address.

---

# 3. Hosts communicate directly only within the same subnet

One important idea from the lecture:

> If H1 and H2 belong to the same network/subnet, they can communicate directly.

Example:

```text
H1 → 130.3.5.1
H2 → 130.3.5.10
```

If both belong to:

```text
130.3.5.0/24
```

then they are in the same subnet.

Therefore:

```text
H1 ───────── H2
      direct
```

But if they belong to different subnets, communication requires an intermediate networking device/router.

Also remember:

> **Every subnet has its own interface.**

---

# 4. Subnet Mask

A subnet mask tells us:

```text
Which bits → network/subnet portion
Which bits → host portion
```

Example:

```text
/24
```

means:

```text
11111111.11111111.11111111.00000000
```

Therefore:

```text
255.255.255.0
```

---

# 5. Prefix Length → Subnet Mask

For:

```text
/22
```

we have:

```text
22 ones + remaining zeros
```

Binary:

```text
11111111.11111111.11111100.00000000
```

Convert each octet:

```text
11111111 = 255
11111111 = 255
11111100 = 252
00000000 = 0
```

Therefore:

```text
/22 = 255.255.252.0
```

### Important GATE pattern

```text
/8  → 255.0.0.0
/16 → 255.255.0.0
/24 → 255.255.255.0
```

And for intermediate prefixes, convert the partially filled octet.

Example:

```text
/22
```

means:

```text
8 + 8 + 6 = 22
```

so:

```text
255.255.252.0
```

---

# 6. No Subnetting vs Subnetting

## Without subnetting

Hosts can think that all other hosts belong to the same network.

Conceptually:

```text
             128.143.70.0/16
                    |
       ┌────────────┼────────────┐
       H1           H2           H3
```

There is no additional subnet-level separation.

---

## With subnetting

The extended network prefix differentiates the subnets.

Example:

```text
128.143.137.0/24
128.143.71.0/24
```

Hosts having the same extended network prefix belong to the same subnet.

---

# 7. Protocols and Layering

After subnetting, the lecture moves into:

```text
Protocols and Layering
```

The central idea is:

> Communication between two devices requires many different functions.

---

# 8. Why do we need Layering?

Imagine sending a physical letter.

The sender has multiple tasks:

```text
Write the letter
      ↓
Put it in envelope
      ↓
Send to post office
      ↓
Transportation
      ↓
Destination post office
      ↓
Receiver gets letter
      ↓
Open envelope
      ↓
Read letter
```

Instead of one giant system doing everything, different stages handle different responsibilities.

This is the basic intuition behind **layering**.

---

# 9. Layering Analogy — Shipping a Toy

The lecture gives a shipping analogy.

Suppose:

```text
Sender → wants to send toy → California → Receiver
```

There are multiple possible operations:

### Step 1

Hire:

```text
Truck + truck driver
```

to take the toy to a railway station.

### Step 2

Use a:

```text
Train
```

to move it toward California.

### Step 3

Hire another truck/driver to take it from the station to the destination.

So:

```text
Sender
  ↓
Truck
  ↓
Railway
  ↓
Train
  ↓
Railway
  ↓
Truck
  ↓
Receiver
```

---

# 10. Important Layering Insight

Suppose you simply drop the toy at:

```text
FedEx office
```

You don't personally perform the remaining operations.

But that **does NOT mean those operations don't happen**.

Someone else handles them.

So:

> A layer can provide a service while internally depending on many lower-level operations.

This is a very important way to understand abstraction.

---

# 11. Service Between Layers

Suppose:

```text
Person 1
   ↓
Layer 1
   ↓
Layer 2
```

One layer provides a service to another layer.

For example:

```text
Layer 1:
"I'll perform these two tasks."

Layer 2:
"Good. You handle those; I'll handle the remaining tasks."
```

Thus, layers divide a complicated communication problem into manageable pieces.

---

# 12. Why not implement everything in one program?

Suppose you have a huge project.

You could implement everything in:

```text
one gigantic file
```

or:

```text
multiple files/modules
```

The second approach is easier to:

- design
    
- understand
    
- modify
    
- debug
    
- reuse
    

Similarly, computer networks divide communication functionality into **layers**.

---

# 13. Protocol

## Definition

A **protocol** is:

> An agreed-upon convention for communication.

Both endpoints must understand the protocol.

A protocol must be:

- formally defined
    
- unambiguous
    

The lecture then focuses on important TCP/IP protocols.

---

# 14. Why do we need protocols?

Imagine two computers communicating.

They need to agree on things such as:

- how data is represented
    
- how addresses are included
    
- how errors are detected
    
- how data is divided
    
- how the receiver interprets the information
    

For example, the lecture notes that data must be converted into appropriate signals when transmitting over a medium.

Other functions include:

```text
Addressing
Error checking
Process identification
Data organization
```

---

# 15. Layering and the OSI Model

The lecture compares:

```text
OSI Model
```

with:

```text
TCP/IP Protocol Suite
```

### OSI has 7 layers

From top to bottom:

```text
7. Application
8. Presentation
9. Session
10. Transport
11. Network
12. Data Link
13. Physical
```

### TCP/IP stack shown in the lecture

```text
Application
Transport
Network
Data Link
Physical
```

So the upper OSI layers:

```text
Application
Presentation
Session
```

are represented together by the TCP/IP **Application layer**.

---

# 16. Layer Communication

Consider:

```text
Sender                              Receiver

Application                        Application
Transport                          Transport
Network                            Network
Data Link                          Data Link
Physical                           Physical
        ← transmission medium →
```

The application data starts at the sender's Application layer.

It travels downward through layers.

At the destination, it travels upward.

Conceptually:

```text
Sender
Application
    ↓
Transport
    ↓
Network
    ↓
Data Link
    ↓
Physical
    ↓
   wire
    ↓
Physical
    ↑
Data Link
    ↑
Network
    ↑
Transport
    ↑
Application
Receiver
```

---

# 17. Peer-to-Peer Communication

A very important conceptual point:

The same layer on two different machines is considered a **peer layer**.

For example:

```text
Sender Application
        ↕
Receiver Application
```

They logically communicate with each other.

Similarly:

```text
Sender Transport
        ↕
Receiver Transport
```

The actual data physically travels through lower layers and the network.

So:

> Peer layers communicate logically, while the actual transmission goes through the lower layers.

---

# 18. Encapsulation

Each layer needs some information to perform its job.

Therefore:

```text
Upper-layer data
       ↓
Layer adds control information
       ↓
Passed to lower layer
```

The control information is usually added as a **header**.

The lecture describes this as:

> Each layer adds control information to the data before passing it to the lower layer.

At the receiver, the corresponding peer layer uses this control information.

---

# 19. Header

A **header** contains control information required by a protocol/layer.

Conceptually:

```text
Header | Data
```

When data moves downward:

```text
Application Data
      ↓
Transport Header + Data
      ↓
Network Header + Transport Header + Data
      ↓
Data Link Header + Network Header + Transport Header + Data
```

This process is called **encapsulation**.

At the receiver, headers are processed/removed as data moves upward.

---

# 20. Protocol Data Units (PDU)

The lecture gives names to the communication units at different layers.

|Layer|Unit|
|---|---|
|Physical|**Bit**|
|Data Link|**Frame**|
|Network|**Datagram**|
|Transport|**Segment**|

So remember:

```text
Physical   → Bit
Data Link  → Frame
Network    → Datagram
Transport  → Segment
```

### GATE memory trick

```text
Bit → Frame → Datagram → Segment
```

Moving upward:

```text
Segment
  ↓
Datagram
  ↓
Frame
  ↓
Bits
```

---

# 21. Physical Layer

The Physical Layer deals with the actual transmission of the **bit stream over the physical medium**.

It determines how bits are represented as physical signals.

Possible signals include:

- electrical
    
- optical
    
- other physical forms
    

Example:

```text
0 → -1V
1 → +1V
```

The exact encoding depends on the physical technology.

---

# 22. Physical Layer — Bit Representation

The physical layer determines:

```text
How 0 and 1 are converted into signals.
```

For example:

```text
Bits:

1 0 1 1 0

       ↓

Physical signal:

__|‾‾|__|‾‾
```

The lecture diagram shows bits being converted into a signal and transmitted through the transmission medium.

---

# 23. Physical Layer — Bit Length and Data Rate

The Physical Layer also deals with:

### Bit length

How long a bit lasts.

### Data rate

How many bits are transmitted per second.

```text
Data rate = bits / second
```

Different physical media have different characteristics.

Examples mentioned:

- copper wire
    
- coaxial cable
    
- fiber optics
    

---

# 24. Data Link Layer

The Data Link Layer provides communication between devices connected over the **same link/network**.

Its major functions in the lecture are:

1. Framing
    
2. Physical addressing
    
3. Error control
    
4. Access control
    

---

# 25. Data Link Layer — Framing

The network layer gives a stream of data to the Data Link Layer.

The DLL divides this stream into manageable units called:

> **Frames**

Conceptually:

```text
Network Layer data
        ↓
Data Link Layer
        ↓
┌─────────┬─────────┬─────────┐
│ Frame 1 │ Frame 2 │ Frame 3 │
└─────────┴─────────┴─────────┘
```

So:

```text
Data Link PDU = Frame
```

---

# 26. Physical Addressing

The Data Link Layer adds physical addressing information.

This identifies the appropriate receiver on the local network.

The address associated with the network interface is the:

> **MAC address**

---

# 27. MAC Address

Every networking device/interface has a MAC address.

The lecture describes it as:

```text
48-bit address
```

associated with the:

```text
Network Interface Card (NIC)
```

Example:

```text
CE:FA:39:52:69:7F
```

This contains:

```text
6 hexadecimal bytes
```

because:

```text
6 × 8 = 48 bits
```

---

# 28. MAC Address vs IP Address

This is extremely important.

## MAC address

Associated with the network interface.

It is used for **local/link-level communication**.

## IP address

Used for **logical addressing and routing**.

The lecture highlights an important reason for having both.

### MAC doesn't follow a network hierarchy/prefix

You cannot naturally say:

```text
MAC prefix → subnet
```

in the same way as IP addressing.

Therefore, MAC addresses are not suitable for routing across large networks.

---

# 29. Why do we need IP if we already have MAC?

This is one of the most important questions in the lecture.

Suppose:

```text
MAC address = fixed interface identifier
```

But hosts can move between networks.

For example:

```text
Laptop
   ↓
Home Wi-Fi
   ↓
College Wi-Fi
```

The laptop's MAC address can remain associated with the interface.

But its **IP address can change** depending on the network it connects to.

So:

```text
MAC → identifies interface
IP  → identifies logical/network location
```

---

# 30. IP Addressing and Routing

The lecture explicitly gives:

> IP addressing is needed for routing/navigation.

Why?

Because routers need hierarchical information to determine where packets should go.

Conceptually:

```text
IP address
    ↓
Network prefix
    ↓
Which network?
    ↓
Which direction?
```

MAC doesn't provide this kind of hierarchical routing information.

---

# 31. Same Subnet and MAC

Within a subnet, devices can use link-layer addressing.

Conceptually:

```text
        Subnet
   ┌───────────────┐
   │               │
  H1              H2
   │               │
   └───────────────┘
```

The MAC address is relevant to the local link.

But when communication crosses routers, IP addressing becomes essential.

---

# 32. Data Link Layer — Error Control

The Data Link Layer can add reliability mechanisms to the physical layer.

The physical layer itself essentially gives us:

```text
raw bits
```

But physical transmission can introduce errors.

The DLL can add information that helps:

```text
detect errors
and/or
recover from lost/damaged frames
```

The lecture describes this as adding a **trailer** with information necessary for detecting/recovering from damaged or lost frames.

---

# 33. Data Link Layer — Access Control

Suppose multiple devices share the same link.

```text
H1 ─┐
H2 ─┼── shared link
H3 ─┘
```

Who gets to transmit?

The Data Link Layer can determine:

> Which device has control over the shared link at a particular time.

This is **access control**.

---

# 34. Point-to-Point Link

The lecture gives a special case:

```text
H1 ───────── H2
```

Only two devices are connected.

This is a:

> **Point-to-point link**

In this situation, there may not be a need for complicated shared-medium access control because there aren't many competing devices.

The lecture uses **PPP** as an example.

```text
H1 ───── PPP ───── H2
```

---

# 35. Data Link Layer Summary

Think:

```text
DATA LINK
│
├── Framing
├── Physical addressing → MAC
├── Error control
└── Access control
```

### Main question answered:

> How do two directly connected devices reliably communicate over a single link?

---

# 36. Data Link Layer as an Abstraction

The lecture gives a very useful conceptual statement:

> The Data Link Layer transforms the Physical Layer's raw stream of bits into a reliable link between two devices on the same network.

So:

```text
Physical Layer
     ↓
raw / unreliable-looking bit stream
     ↓
Data Link Layer
     ↓
reliable link between adjacent devices
```

The DLL makes the physical layer appear **error-free to the upper layer**.

---

# 37. Frame Structure

A Data Link frame can conceptually look like:

```text
┌──────┬─────────────┬──────┐
│ H1   │    Data     │ T2   │
└──────┴─────────────┴──────┘
```

Where:

```text
H1 = header
T2 = trailer
```

The header/trailer provide control information.

The data portion comes from the upper layer.

---

# 38. Network Layer

The Network Layer solves a larger problem.

The Data Link Layer handles:

```text
device → device
```

over a local link.

The Network Layer handles:

```text
source → destination
```

across potentially **multiple networks/links**.

---

# 39. Network Layer — Logical Addressing

Physical addressing handles local delivery.

But if a packet travels across multiple networks, we need a different addressing system.

This is:

> **Logical addressing**

In TCP/IP, this is primarily based on:

> **IP addresses**

The network layer uses logical addressing to distinguish source and destination networks.

---

# 40. Network Layer — Routing

The Network Layer provides the mechanism to route packets toward their final destination.

Imagine:

```text
A ── B ── C
 \       /
   D ── E
```

A packet from A to E could potentially follow multiple paths.

Routing determines:

> Which path should be used?

The lecture associates routing with algorithms such as:

```text
DVR
LSR
```

where:

```text
DVR = Distance Vector Routing
LSR = Link State Routing
```

---

# 41. Routing vs Forwarding

This distinction is **very important for GATE**.

## Routing

> Deciding/finding the route.

It determines:

```text
Which path should traffic follow?
```

Example:

```text
A → B → C → D
```

Routing decides that this is the appropriate path.

---

## Forwarding

> Actually sending the packet according to the route.

Once a packet reaches a router:

```text
Packet arrives
      ↓
Router checks forwarding table
      ↓
Selects outgoing interface
      ↓
Forwards packet
```

---

# 42. Routing vs Forwarding — Core Difference

Think:

```text
ROUTING
"Which road should I use?"

FORWARDING
"Now send the packet onto that road."
```

### Routing

- Finds routes.
    
- Can operate even when no packet currently needs to be sent.
    
- Maintains/prepares routing information.
    

### Forwarding

- Uses an already determined route.
    
- Happens when a packet arrives.
    
- Sends packet toward destination.
    
- Happens in real time.
    

---

# 43. Network Layer — Fragmentation and Reassembly

Different Data Link technologies may have different maximum packet sizes.

Therefore:

```text
Network Layer packet
        ↓
too large for underlying link
        ↓
fragment into smaller pieces
```

The Network Layer can split the packet into pieces.

Those pieces can then be transmitted.

At the destination, they can be:

```text
reassembled
```

into the original packet.

---

# 44. Network Layer Summary

Think:

```text
NETWORK LAYER
│
├── Logical addressing → IP
├── Routing
├── Forwarding
└── Fragmentation / Reassembly
```

Main question:

> **How does a packet travel from one computer to another across multiple networks?**

---

# 45. Network Layer vs Data Link Layer

This distinction should be crystal clear.

### Data Link Layer

```text
Device → Device
```

Usually over:

```text
one link / same local network
```

### Network Layer

```text
Source → Destination
```

Across:

```text
multiple links / multiple networks
```

Visualize:

```text
Host A
  │
  │ Data Link
  ↓
Router 1
  │
  │ Data Link
  ↓
Router 2
  │
  │ Data Link
  ↓
Host B
```

The Network Layer provides the **end-to-end path across the network**, while each individual Data Link Layer handles the local link.

---

# 46. Transport Layer

The Transport Layer moves communication one level higher.

Network Layer:

```text
Host → Host
```

Transport Layer:

```text
Process → Process
```

This distinction is extremely important.

---

# 47. Why Process-to-Process Delivery?

One computer can run many processes simultaneously.

Example:

```text
Computer A
│
├── Browser
├── YouTube
├── Email
└── SSH
```

Suppose a packet reaches the correct computer.

How does the computer know:

> Which application/process should receive it?

The answer is:

**Port number.**

---

# 48. Port Addressing

The Transport Layer uses:

> **Port addresses**

A port identifies the destination process/service on a host.

Conceptually:

```text
IP address
    ↓
Which computer?
    ↓
Port number
    ↓
Which process?
```

So:

```text
IP → host identification
Port → process identification
```

---

# 49. Network Layer vs Transport Layer

This is one of the most important GATE distinctions:

```text
Network Layer:
Source Host → Destination Host

Transport Layer:
Source Process → Destination Process
```

Example:

```text
PC A
  ↓
IP address identifies PC B
  ↓
Port identifies browser/server process
```

The lecture summarizes this as:

> Network layer gets each packet to the correct computer, while the transport layer gets the entire message to the correct process on that computer.

---

# 50. Transport Layer — Segmentation

Suppose the application generates a large message.

The Transport Layer divides it into smaller units:

> **Segments**

```text
Large message
      ↓
┌────────┐
│Segment1│
├────────┤
│Segment2│
├────────┤
│Segment3│
└────────┘
```

Each segment can carry a sequence number.

---

# 51. Sequence Numbers

Sequence numbers allow the receiver to:

- identify the order of segments
    
- reassemble the original message
    
- identify missing segments
    

Example:

```text
Segment 1
Segment 2
Segment 3
```

Suppose they arrive:

```text
1
3
```

The receiver can detect that:

```text
Segment 2
```

is missing.

---

# 52. Transport Layer — Reassembly

At the destination:

```text
Segment 1
Segment 2
Segment 3
       ↓
Sequence information
       ↓
Original message
```

Thus:

```text
Segmentation → sender
Reassembly → receiver
```

---

# 53. Transport Layer — Flow and Error Control

The Transport Layer can perform:

- flow control
    
- error control
    

Importantly, the lecture emphasizes that these occur:

> **End-to-end**

rather than only across a single link.

This is a key difference from Data Link Layer control.

---

# 54. Data Link Error Control vs Transport Error Control

This distinction is worth memorizing conceptually.

### Data Link Layer

```text
Error control
between neighboring devices
```

Example:

```text
Host → Router
```

### Transport Layer

```text
Error/flow control
end-to-end
```

Example:

```text
Process on Host A
        ↓
    Internet
        ↓
Process on Host B
```

---

# 55. Transport Service Requirements

Different applications have different communication requirements.

The lecture shows examples such as:

|Application|Data loss|Bandwidth|Time sensitivity|
|---|---|---|---|
|File transfer|No loss|Elastic|No|
|E-mail|No loss|Elastic|No|
|Web documents|No loss|Elastic|No|
|Real-time audio/video|Loss-tolerant|Specified|Yes|
|Interactive games|Loss-tolerant|Low|Yes|
|Financial apps|No loss|Elastic|—|

---

# 56. Why Application Requirements Matter

Different applications require different transport characteristics.

### File transfer

We cannot casually lose data.

```text
File:
A B C D E

Missing C
→ file corrupted
```

So it requires reliable delivery.

### Real-time video

A small amount of loss may be acceptable.

Why?

Because:

```text
old frame arriving too late
```

may be less useful than simply skipping it.

Therefore:

```text
Reliability requirement
depends on application.
```

---

# 57. Examples Mentioned in the Lecture

The lecture connects transport requirements to applications such as:

- Email
    
- Web browsing
    
- File transfer
    
- DNS
    
- Real-time media
    
- Interactive games
    

The interaction between layers is important because different applications impose different requirements.

---

# 58. Why Layering Is Useful

The lecture compares:

## Without layering

Every application must understand every network technology.

Imagine:

```text
HTTP ─┐
FTP  ─┼── Coaxial
NFS  ─┼── Fiber
Telnet┘   Radio
```

Every application would need separate handling for every transmission technology.

This becomes complicated very quickly.

---

# 59. With Layering

An intermediate layer hides the underlying technology.

```text
Application
   │
   ↓
Intermediate Layer
   │
   ├── Coaxial
   ├── Fiber
   └── Radio
```

The application doesn't need to know the details of each physical technology.

This provides **abstraction**.

---

# 60. Main Benefit of Layering

Layering allows upper layers to use a standardized service without caring about the implementation details below.

Example:

```text
Application
     ↓
"Give me communication."
     ↓
Transport
     ↓
Network
     ↓
Data Link
     ↓
Physical
```

The application doesn't need to know:

```text
Is it Wi-Fi?
Ethernet?
Fiber?
Coaxial?
Radio?
```

The lower layers deal with that.

---

# 61. End-to-End vs Hop-to-Hop

This is a very useful mental model from the lecture.

Suppose:

```text
Host A
   ↓
Router 1
   ↓
Router 2
   ↓
Host B
```

### Network Layer

Provides:

```text
Host A → Host B
```

across the Internet.

### Data Link Layer

Operates separately on each link:

```text
A → R1
R1 → R2
R2 → B
```

So Data Link functionality is repeated on every individual link.

### Transport Layer

Works:

```text
Process A → Process B
```

end-to-end.

The lecture's diagram explicitly distinguishes:

```text
Network layer → Host-to-host delivery

Transport layer → Process-to-process delivery
```

---

# 62. Full Layered Communication Picture

This is probably the **most useful diagram to keep in your notes**:

```text
                  SENDER
                     │
              Application
                     │
               Transport
                     │
                Network
                     │
               Data Link
                     │
                Physical
                     │
                   Bits
                     │
              ─── Network ───
                     │
                   Bits
                     │
                Physical
                     │
               Data Link
                     │
                Network
                     │
               Transport
                     │
              Application
                     │
                  RECEIVER
```

But internally, the responsibilities differ:

```text
Application
    ↓
Application-specific communication

Transport
    ↓
Process → Process

Network
    ↓
Host → Host

Data Link
    ↓
Adjacent device → Adjacent device

Physical
    ↓
Bits → Signals
```

---

# 63. The Complete Layer Comparison

|Layer|Main responsibility|Address / Identifier|PDU|
|---|---|---|---|
|**Application**|Application-level communication|Application-specific|Data|
|**Transport**|Process-to-process delivery|Port number|Segment|
|**Network**|Host-to-host delivery across networks|IP address|Datagram|
|**Data Link**|Local/link delivery|MAC address|Frame|
|**Physical**|Bit transmission|—|Bit|

This table is the **core of this lecture**.

---

# 64. Addressing — Very Important

There are multiple addressing concepts because each layer solves a different problem.

```text
MAC Address
    ↓
Which interface/device on local link?

IP Address
    ↓
Which host/network across internetwork?

Port Number
    ↓
Which process/application on that host?
```

Therefore:

```text
MAC → local delivery
IP  → host-to-host routing
Port → process-to-process delivery
```

---

# 65. The Three-Level Delivery Mental Model

Suppose you open a website.

Think:

```text
1. MAC
   ↓
Get the frame to the next local device.

2. IP
   ↓
Get the packet to the correct destination host.

3. Port
   ↓
Get the data to the correct process.
```

This is much easier to remember than memorizing isolated definitions.

---

# 66. Encapsulation and Decapsulation

## Sender

```text
Application Data
      ↓
Transport
[Transport Header | Data]
      ↓
Network
[Network Header | Transport Header | Data]
      ↓
Data Link
[DL Header | Network Header | Transport Header | Data | Trailer]
      ↓
Physical
Bits
```

This is:

> **Encapsulation**

---

## Receiver

The reverse occurs:

```text
Bits
  ↓
Physical
  ↓
Frame
  ↓
Data Link removes/processes DLL information
  ↓
Datagram
  ↓
Network processes network information
  ↓
Segment
  ↓
Transport processes transport information
  ↓
Application Data
```

This is:

> **Decapsulation**

---

# 67. Layering — The Big Picture

The entire purpose of layering can be summarized as:

```text
Complex communication problem
            ↓
Divide into layers
            ↓
Each layer performs specific functions
            ↓
Each layer provides service to upper layer
            ↓
Each layer uses lower-layer service
            ↓
Communication becomes modular
```

---

# 68. Most Important GATE Distinctions

## 1. MAC vs IP

```text
MAC → local/link-level identification
IP  → logical addressing + routing
```

---

## 2. IP vs Port

```text
IP   → identifies destination host
Port → identifies destination process
```

---

## 3. Data Link vs Network

```text
Data Link → device-to-device / one link
Network   → source-to-destination across networks
```

---

## 4. Network vs Transport

```text
Network   → host-to-host
Transport → process-to-process
```

---

## 5. Routing vs Forwarding

```text
Routing
→ finding/deciding the path

Forwarding
→ sending packet using that path
```

---

## 6. Physical vs Data Link

```text
Physical
→ raw bit transmission

Data Link
→ turns raw bits into frames
→ local reliable link
```

---

## 7. Data Link Error Control vs Transport Error Control

```text
Data Link
→ local / link-level

Transport
→ end-to-end
```

---

# 69. PDU Names — Must Know

```text
Application → Data
Transport   → Segment
Network     → Datagram
Data Link   → Frame
Physical    → Bit
```

### Memorize this chain:

```text
DATA
 ↓
SEGMENT
 ↓
DATAGRAM
 ↓
FRAME
 ↓
BITS
```

---

# 70. Addressing Chain — Must Know

```text
Application
     ↓
Port Number
     ↓
IP Address
     ↓
MAC Address
     ↓
Physical signal
```

Conceptually:

```text
Port → Process
IP   → Host
MAC  → Local interface
Signal → Actual transmission
```

---

# 71. Subnetting — Must Know

### Original addressing

```text
Network Prefix | Host
```

### Subnetted addressing

```text
Network Prefix | Subnet | Host
```

Therefore:

```text
Extended Network Prefix
=
Network Prefix + Subnet Number
```

And:

```text
/22 = 255.255.252.0
```

---

# 72. One Final Mental Model

Imagine sending a message from:

```text
Your Browser
```

to:

```text
A Web Server
```

The journey is:

```text
APPLICATION
"Here is the data I want to send."

        ↓

TRANSPORT
"Which process should receive it?"
→ Port number
→ Segment
→ sequence information

        ↓

NETWORK
"Which host/network should receive it?"
→ IP address
→ Datagram
→ Routing

        ↓

DATA LINK
"Which nearby device should receive this frame?"
→ MAC address
→ Frame
→ Error/access control

        ↓

PHYSICAL
"How do I represent these bits physically?"
→ electrical/optical/radio signals
→ Bits
```

At the destination, the process happens in reverse.

---

# 73. GATE Quick Revision Sheet

```text
SUBNETTING
────────────────────────────────
Network Prefix + Subnet + Host
Extended Network Prefix
/22 = 255.255.252.0
Each subnet has its own interface
Same subnet → direct communication


PROTOCOL
────────────────────────────────
Agreed-upon convention for communication
Both endpoints must understand it
Must be formally defined and unambiguous


OSI
────────────────────────────────
Application
Presentation
Session
Transport
Network
Data Link
Physical


TCP/IP
────────────────────────────────
Application
Transport
Network
Data Link
Physical


PDU
────────────────────────────────
Application → Data
Transport   → Segment
Network     → Datagram
Data Link   → Frame
Physical    → Bit


PHYSICAL
────────────────────────────────
Bit transmission
Bit representation
Signals
Bit length
Data rate


DATA LINK
────────────────────────────────
Framing
MAC / physical addressing
Error control
Access control
Local link
Frame


NETWORK
────────────────────────────────
Logical addressing
IP
Routing
Forwarding
Fragmentation/reassembly
Host-to-host
Datagram


TRANSPORT
────────────────────────────────
Port addressing
Segmentation/reassembly
Sequence numbers
Flow control
Error control
Process-to-process
Segment


KEY DISTINCTIONS
────────────────────────────────
MAC → local/link
IP → host/network
Port → process

DLL → device-to-device
NL  → host-to-host
TL  → process-to-process

Routing → find path
Forwarding → send packet using path

DLL error control → link-level
TL error control → end-to-end
```

The final page of the PDF begins the next topic, **Switching**, after briefly revisiting the layered stack. So I would treat **Switching as the start of the next lecture/topic**, not as material actually developed in this Lecture 5.