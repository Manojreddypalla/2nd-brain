# CN Lecture 5 — Short Notes

## 1. Subnetting

- **Subnetting:** Dividing one large network into smaller logical networks.
    
- Address structure:
    

```text
Normal:    Network Prefix | Host
Subnetted: Network Prefix | Subnet | Host
```

- **Extended Network Prefix** = Network Prefix + Subnet Number.
    
- Hosts in the **same subnet** can communicate directly.
    
- Each subnet has its own interface.
    
- `/22` → `255.255.252.0`.
    

---

## 2. Protocol

- **Protocol:** Agreed-upon rules/conventions for communication.
    
- Must be **formally defined and unambiguous**.
    
- Both communicating endpoints must understand the protocol.
    

---

## 3. Layering

- Complex communication is divided into **layers**.
    
- Each layer:
    
    - Performs specific functions.
        
    - Provides services to the layer above.
        
    - Uses services of the layer below.
        
- Main advantage → **abstraction + modularity**.
    

### OSI Model

```text
Application
Presentation
Session
Transport
Network
Data Link
Physical
```

### TCP/IP Stack

```text
Application
Transport
Network
Data Link
Physical
```

OSI's Application + Presentation + Session are combined into TCP/IP's **Application layer**.

---

# 4. Encapsulation

As data moves **down** the layers:

```text
Application Data
      ↓
Transport Header + Data
      ↓
Network Header + ...
      ↓
Data Link Header + ... + Trailer
      ↓
Bits
```

- Each layer adds its own **control information**.
    
- Receiver performs the reverse process → **decapsulation**.
    

---

# 5. PDU Names ⭐

|Layer|PDU|
|---|---|
|Application|Data|
|Transport|**Segment**|
|Network|**Datagram**|
|Data Link|**Frame**|
|Physical|**Bit**|

```text
Data → Segment → Datagram → Frame → Bits
```

---

# 6. Physical Layer

- Transmits **bits** over physical medium.
    
- Converts bits into physical signals.
    
- Deals with:
    
    - Signal representation
        
    - Bit length
        
    - Data rate
        
    - Transmission medium
        

Examples of media:

- Copper
    
- Coaxial
    
- Fiber optic.
    

---

# 7. Data Link Layer ⭐

### Functions

- **Framing**
    
- **Physical addressing → MAC**
    
- **Error control**
    
- **Access control**
    

### MAC Address

- 48-bit address.
    
- Associated with NIC/interface.
    
- Used mainly for **local/link-level communication**.
    

```text
MAC → local delivery
```

### Point-to-Point

```text
Host ───────── Host
```

Example protocol: **PPP**.

---

# 8. MAC vs IP ⭐

### MAC

```text
Identifies interface on local link
```

### IP

```text
Logical address
Used for routing/navigation
```

**Why IP is needed?**

MAC addresses don't provide the hierarchical network information required for routing.

```text
MAC → local identification
IP  → logical/network identification
```

---

# 9. Network Layer ⭐

### Functions

- **Logical addressing → IP**
    
- **Routing**
    
- **Forwarding**
    
- **Fragmentation & reassembly**
    

### Main purpose

```text
Host → Host
```

across multiple networks.

### Routing

Finding/deciding the path.

Examples:

- **DVR** → Distance Vector Routing
    
- **LSR** → Link State Routing
    

### Forwarding

Actually sending the packet according to the routing/forwarding information.

```text
Routing   → Which path?
Forwarding → Send through which interface?
```

---

# 10. Transport Layer ⭐

### Main purpose

```text
Process → Process
```

### Functions

- Port addressing
    
- Segmentation
    
- Reassembly
    
- Sequence numbers
    
- Flow control
    
- Error control
    

### Port

```text
IP address → Which host?
Port number → Which process?
```

Transport-layer error/flow control is **end-to-end**.

---

# 11. Error Control

### Data Link

```text
Link-level / neighboring devices
```

### Transport

```text
End-to-end
```

---

# 12. Application Requirements

Different applications need different transport characteristics.

- **File transfer / Email / Web:** generally require no data loss.
    
- **Real-time audio/video:** can tolerate some loss but are time-sensitive.
    
- **Interactive games:** loss-tolerant and time-sensitive.
    

---

# 13. ⭐ GATE Must-Remember

```text
Physical   → Bit
Data Link  → Frame
Network    → Datagram
Transport  → Segment
```

```text
MAC  → Local/link
IP   → Host/network
Port → Process
```

```text
Data Link → Device-to-Device
Network   → Host-to-Host
Transport → Process-to-Process
```

```text
Routing   → Find path
Forwarding → Send packet
```

```text
DLL Error Control → Link-level
TL Error Control  → End-to-end
```