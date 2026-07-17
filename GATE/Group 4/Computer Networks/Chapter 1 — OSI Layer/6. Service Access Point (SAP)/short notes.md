# Module 1 — Service Access Point (SAP) | Short Notes

## Definition ⭐

> **SAP (Service Access Point)** is the **interface** through which a higher layer accesses the services of its adjacent lower layer.

---

## Core Idea

- **Higher Layer** → Service User
    
- **Lower Layer** → Service Provider
    
- **SAP** → Interface between them
    

```text
Higher Layer
     │
    SAP
     │
Lower Layer
```

---

## SAP Exists Between Every Adjacent Layer

```text
Application
   │
  SAP
   │
Transport
   │
  SAP
   │
Network
   │
  SAP
   │
Data Link
   │
  SAP
   │
Physical
```

---

## What is a Service?

A **service is not data**.

It is the **functionality** performed by the lower layer.

Examples:

- **Transport:** Reliable delivery, Segmentation
    
- **Network:** Routing, Logical Addressing
    
- **Data Link:** Framing, CRC, MAC Addressing
    
- **Physical:** Signal Transmission
    

---

## Sender (Encapsulation)

The higher layer requests the lower layer to perform its service.

```text
Application
    ↓
Transport
    ↓
Network
    ↓
Data Link
    ↓
Physical
```

Example:

> "Here is my data. Do your job."

---

## Receiver (Decapsulation)

The process is **automatic**.

Each layer:

1. Receives data
    
2. Checks/Processes its header
    
3. Removes its header
    
4. Passes the payload upward
    

```text
Physical
    ↑
Data Link
    ↑
Network
    ↑
Transport
    ↑
Application
```

---

## Key Point ⭐⭐⭐

- **Sending:** Upper layer **requests** lower-layer services.
    
- **Receiving:** Data arrival **automatically triggers** each layer to process and pass data upward.
    

---

## SAP vs Service vs Protocol

|Term|Meaning|
|---|---|
|**Service**|Functionality provided by lower layer|
|**SAP**|Interface to access that service|
|**Protocol**|Rules between peer layers on different hosts|

---

## GATE Points

- SAP exists **between adjacent layers of the same host**.
    
- SAP is **not a protocol**.
    
- SAP is **not the service itself**.
    
- SAP is **bidirectional** (used in both encapsulation and decapsulation).
    
- **Service relationship is always one-way:** Higher layer uses services, lower layer provides them.
    

---

## One-Line Revision 🎯

> **SAP is the bidirectional interface between adjacent layers through which the higher layer accesses the services of the lower layer.**


