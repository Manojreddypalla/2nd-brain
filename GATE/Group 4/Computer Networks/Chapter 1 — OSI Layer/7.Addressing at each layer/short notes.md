# Module 1 — Addressing at Each Layer | Short Notes

## Definition ⭐

Each OSI layer uses its **own type of address** because each layer performs a different function.

---

## Addressing at Each Layer

|Layer|Address|Purpose|
|---|---|---|
|**Application**|Domain Name / URL / Email|Identify service|
|**Transport**|Port Number|Identify application/process|
|**Network**|IP Address|Identify host|
|**Data Link**|MAC Address|Identify device in LAN|
|**Physical**|No Address|Transmit bits|

---

## Easy Memory Trick ⭐

```
Application → Name
Transport  → Port
Network    → IP
Data Link  → MAC
Physical   → No Address
```

---

## Purpose of Each Address

- **Name** → Which service? (e.g., google.com)
- **Port** → Which application? (e.g., HTTP, SSH)
- **IP** → Which computer?
- **MAC** → Which device on the local network?
- **Physical** → Sends bits/signals only.

---

## Example

```
www.google.com
      ↓
Port 443
      ↓
IP Address
      ↓
MAC Address
      ↓
Signals
```

---

# GATE Corner 🎯

### Important Points

- **Application → Name**
- **Transport → Port Number**
- **Network → IP Address**
- **Data Link → MAC Address**
- **Physical → No Address**

### GATE Traps ⚠️

❌ MAC Address identifies a process.

✔ **False** → Port Number identifies a process.

---

❌ IP Address identifies an application.

✔ **False** → IP identifies a host.

---

❌ Routers use MAC Address for routing.

✔ **False** → Routers use **IP Address**.

---

❌ Physical layer has an address.

✔ **False** → It only transmits bits.

---

## One-Line Revision 📝

> **Application → Name | Transport → Port | Network → IP | Data Link → MAC | Physical → No Address**