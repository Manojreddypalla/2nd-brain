Perfect. Now we reach the layer that actually **puts packets onto the local network**.

This is where concepts like **MAC Address, Ethernet, Switches, ARP, VLANs, CRC** begin. Layer 2 is one of the most important GATE topics.

---

# 📄 OSI Model – Layer 2: Data Link Layer

> **Module 1 → Functions of Each Layer → Layer 2**

---

# 🎯 Why Does the Data Link Layer Exist?

Imagine the Network Layer says:

> "I know the destination IP address."

Great.

But here's the question:

> **How do I actually send this packet to the next device?**

Knowing **where** the destination is isn't enough.

You still need to deliver it to the **next hop**.

That's the job of the **Data Link Layer**.

---

# 🧠 Think of This Analogy

Suppose Amazon has already decided the route.

```text
Hyderabad
    ↓
Nagpur
    ↓
Delhi
```

Now the first truck reaches the **Hyderabad warehouse**.

How does the parcel go from:

```text
Warehouse A
        ↓
Warehouse B
```

Someone has to perform **each local delivery**.

That's exactly what Layer 2 does.

---

# 💡 Core Idea

The Data Link Layer provides **Node-to-Node (Hop-to-Hop) Communication**.

It delivers data **between two directly connected devices**.

Examples:

- Laptop → Switch
    
- Switch → Router
    
- Router → Switch
    
- Switch → Server
    

Notice:

It **doesn't care about Google**.

It only cares about the **next device**.

---

# The Biggest Difference

### Layer 4

> Did the complete message reach the application?

---

### Layer 3

> Which path should I take?

---

### Layer 2

> **How do I send this packet to the next directly connected device?**

---

# Real Example

Suppose you open YouTube.

```text
Laptop
     │
Switch
     │
Router
     │
ISP
```

Network Layer decides:

> Send toward Router.

Data Link Layer asks:

> What's the router's MAC address?

Then creates a frame and sends it.

---

# Why Can't Layer 3 Do This?

Layer 3 only knows

```text
Destination IP

142.250.xxx.xxx
```

But your network card cannot transmit using an IP address.

The NIC understands only

```text
MAC Address
```

So Layer 2 converts the packet into something that can travel on the local network.

---

# Mental Model

Imagine you're in an apartment.

The Network Layer says:

> Deliver this parcel to Flat 402.

The Data Link Layer asks:

> **Which door should I knock on next?**

Not the final destination.

Just the next door.

---

# Main Responsibilities

### 1. Framing

Receives a packet from Layer 3.

Adds a Layer 2 header and trailer.

Creates a

```text
Frame
```

---

### 2. Physical Addressing

Uses

```text
MAC Address
```

instead of IP.

Example

```text
00:1A:2B:3C:4D:5E
```

---

### 3. Error Detection

Uses

```text
CRC
```

to detect transmission errors.

Notice

Detects

Does **not necessarily correct**.

---

### 4. Flow Control

Controls transmission speed between directly connected devices.

---

### 5. Media Access Control

Suppose

```text
Laptop A
Laptop B
Laptop C
```

All want to send at the same time.

Who goes first?

Layer 2 solves this.

---

# 🌍 Real-Life Example

Suppose your laptop sends data.

```text
Laptop
      │
Frame
      │
Switch
      │
Router
```

The switch reads

```text
Destination MAC
```

and forwards accordingly.

---

# Devices

Layer 2 devices include

- Switch
    
- Bridge
    
- NIC (Network Interface Card)
    

---

# Protocols

We'll study these in depth later.

Main Layer 2 protocols include:

- Ethernet (IEEE 802.3)
    
- Wi-Fi MAC (IEEE 802.11)
    
- PPP
    
- HDLC
    
- Frame Relay
    
- ATM
    
- ARP _(Layer 2/3 boundary)_
    
- STP (Spanning Tree Protocol)
    
- VLAN (IEEE 802.1Q)
    
- LACP
    

---

# PDU

```text
Frame
```

---

# Address Used

```text
MAC Address
```

Physical Address

---

# Important Observation

Every time a packet reaches a router,

the router removes

```text
Old Frame
```

Creates

```text
New Frame
```

using the MAC addresses of the next hop.

The **IP packet usually remains the same**, but the **Layer 2 frame changes at every hop**.

This is one of the most important concepts in networking.

---

# 🚫 What It Does NOT Handle

- Routing
    
- IP Addressing
    
- Port Numbers
    
- End-to-End Reliability
    

---

# Common Misconceptions

❌ Switch uses IP Address.

✔ Switch forwards based on **MAC Address**.

---

❌ MAC Address changes every hop.

✔ The **device's MAC address** is fixed (barring spoofing), but the **source and destination MAC addresses in the frame** change at every hop because each hop has different directly connected devices.

---

❌ Data Link Layer routes packets.

✔ It only delivers frames to the **next directly connected device**.

---

# Before we write the final notes...

I want to ask you one question.

Suppose your laptop wants to send a packet to Google's server.

Your laptop knows Google's **IP address**.

**Question:**

How does your laptop know the **MAC address of your home router**, which is the **next hop**?

That single question leads us directly into **ARP**, one of the highest-weight Layer 2 topics in GATE. Once you answer it, Layer 2 will click just like Layer 4 did.