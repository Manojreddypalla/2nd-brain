# GATE Corner – Multiplexing

## Definition

**Multiplexing** is the technique of combining multiple signals into a **single communication channel** to improve bandwidth utilization.

**MUX** → Combines signals

**DEMUX** → Separates signals

---

# Types of Multiplexing

```text
Multiplexing
│
├── Frequency Division Multiplexing (FDM)
├── Time Division Multiplexing (TDM)
│     ├── Synchronous TDM
│     └── Statistical TDM
└── Wavelength Division Multiplexing (WDM)
```

---

# FDM (Frequency Division Multiplexing)

### Principle

- Channel divided into **Frequency Bands**
- Every user gets a **different frequency**
- All users transmit **simultaneously**

### Characteristics

- Analog communication
- Guard Bands required
- Continuous transmission

### Applications

- Radio
- Television
- Cable TV
- Satellite Communication

### Advantages

- Simultaneous communication
- Low delay

### Disadvantages

- Guard Band wastes bandwidth
- Cross-talk possible

---

# TDM (Time Division Multiplexing)

### Principle

- Channel divided into **Time Slots**
- Every user gets the **entire bandwidth** for a short time

### Characteristics

- Digital communication
- Requires synchronization
- No Guard Bands

### Applications

- Telephone Networks
- Digital Communication
- ISDN

### Advantages

- Efficient bandwidth utilization
- No frequency interference

### Disadvantages

- Synchronization required
- Idle slots may waste bandwidth

---

# Synchronous TDM

### Principle

- Fixed time slots
- Slot assigned even if there is **no data**

### Advantages

- Simple implementation
- Predictable transmission

### Disadvantages

- Idle slots wasted
- Lower bandwidth utilization

---

# Statistical TDM (Asynchronous TDM)

### Principle

- Dynamic time slot allocation
- Slot assigned **only when data exists**

### Advantages

- Better bandwidth utilization
- No idle slots
- Higher throughput

### Disadvantages

- More complex
- Buffer required
- Variable delay

---

# WDM (Wavelength Division Multiplexing)

### Principle

- Multiple **wavelengths of light**
- Single optical fiber
- Simultaneous transmission

### Characteristics

- Used only in Optical Fiber
- Very high bandwidth
- Long-distance communication

### Applications

- Fiber Networks
- Internet Backbone
- Undersea Cables

### Advantages

- Extremely high capacity
- Efficient fiber utilization

### Disadvantages

- Expensive
- Complex optical equipment

---

# Comparison

| Feature | FDM | TDM | WDM |
|----------|-----|-----|-----|
| Resource Shared | Frequency | Time | Wavelength |
| Medium | Copper/Wireless | Copper | Optical Fiber |
| Communication | Analog | Digital | Optical |
| Simultaneous Transmission | Yes | No | Yes |
| Guard Band | Yes | No | No |

---

# Synchronous vs Statistical TDM

| Feature | Synchronous TDM | Statistical TDM |
|----------|-----------------|-----------------|
| Slot Allocation | Fixed | Dynamic |
| Empty Slot | Wasted | Not Wasted |
| Bandwidth Utilization | Low | High |
| Buffer Required | No | Yes |
| Complexity | Low | High |

---

# Memory Tricks

✅ **FDM → Frequency**

✅ **TDM → Time**

✅ **WDM → Wavelength**

✅ **Guard Band → Only FDM**

✅ **Optical Fiber → WDM**

✅ **Fixed Slot → Synchronous TDM**

✅ **Dynamic Slot → Statistical TDM**

---

# GATE PYQ Facts ⭐

- FDM mainly used in **Analog Communication**
- TDM mainly used in **Digital Communication**
- WDM is used only in **Optical Fiber**
- Statistical TDM provides **better bandwidth utilization**
- Synchronous TDM wastes bandwidth due to **idle time slots**
- Guard Bands prevent **interference** between adjacent frequency channels
- WDM uses different **light wavelengths**, not electrical frequencies

---

# One-Minute Revision

```text
Multiplexing
│
├── FDM
│   • Frequency
│   • Analog
│   • Guard Band
│
├── TDM
│   • Time Slots
│   • Digital
│   • Synchronization
│
│   ├── Synchronous
│   │   • Fixed Slot
│   │   • Idle Slot Wasted
│   │
│   └── Statistical
│       • Dynamic Slot
│       • Better Utilization
│
└── WDM
    • Wavelength
    • Optical Fiber
    • High Capacity
```
