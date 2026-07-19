# Comparison of Line Encoding Schemes (GATE Quick Notes)

| Encoding Scheme | Bandwidth | Synchronization | DC Component | Error Detection | Advantages | Disadvantages |
|-----------------|-----------|-----------------|--------------|-----------------|------------|---------------|
| **Unipolar NRZ** | Low | Poor | High | No | Simple, Low bandwidth | High DC, Poor synchronization |
| **Unipolar RZ** | High | Better | High | No | Better synchronization | Higher bandwidth, DC component |
| **Polar NRZ-L** | Low | Poor | Low (compared to Unipolar) | No | Simple, Lower DC | Long identical bits cause sync loss |
| **Polar NRZ-I** | Low | Better than NRZ-L | Low | No | Better synchronization for long 1s | Long 0s still cause sync loss |
| **AMI** | Medium | Good | None (≈0) | Yes (BPV) | No DC, Error detection, Good synchronization | Long runs of 0s need scrambling |
| **Pseudoternary** | Medium | Good | None (≈0) | Yes (BPV) | No DC, Error detection | Long runs of 1s need scrambling |
| **Manchester** | High | Excellent | None | Limited | Self-synchronizing, No DC | Requires double bandwidth |
| **Differential Manchester** | High | Excellent | None | Limited | Immune to polarity inversion, Excellent synchronization | High bandwidth |

---

# GATE Quick Comparison

## Bandwidth Requirement

```text
Lowest
↓
NRZ-L ≈ NRZ-I ≈ Unipolar NRZ
↓
AMI ≈ Pseudoternary
↓
Manchester ≈ Differential Manchester
Highest
```

---

## Synchronization

```text
Poor
↓
Unipolar NRZ
Polar NRZ-L

↓

NRZ-I

↓

AMI / Pseudoternary

↓

Manchester
Differential Manchester
Best
```

---

## DC Component

| Scheme | DC Component |
|---------|--------------|
| Unipolar NRZ | High |
| Unipolar RZ | High |
| Polar NRZ-L | Low |
| Polar NRZ-I | Low |
| AMI | None |
| Pseudoternary | None |
| Manchester | None |
| Differential Manchester | None |

---

## Error Detection Capability

| Scheme | Error Detection |
|---------|-----------------|
| Unipolar NRZ | No |
| Unipolar RZ | No |
| Polar NRZ-L | No |
| Polar NRZ-I | No |
| AMI | Yes (Bipolar Violation) |
| Pseudoternary | Yes (Bipolar Violation) |
| Manchester | Limited |
| Differential Manchester | Limited |

---

# GATE Facts ⭐

- **Lowest Bandwidth** → NRZ schemes.
- **Highest Bandwidth** → Manchester & Differential Manchester.
- **Best Synchronization** → Manchester & Differential Manchester.
- **No DC Component** → AMI, Pseudoternary, Manchester, Differential Manchester.
- **Error Detection (BPV)** → AMI & Pseudoternary.
- **NRZ-I** performs better than **NRZ-L** for long sequences of **1s**.
- **AMI** has problems with long runs of **0s**.
- **Pseudoternary** has problems with long runs of **1s**.

---

# Memory Trick 🧠

- **NRZ** → Low Bandwidth, Poor Sync
- **AMI** → Alternate 1s, No DC
- **Pseudoternary** → Alternate 0s, No DC
- **Manchester** → Best Sync, Highest Bandwidth
- **Differential Manchester** → Manchester + Polarity Inversion Immunity

---

# One-Line Revision

- **NRZ** → Low bandwidth, Poor synchronization.
- **AMI** → No DC, Bipolar violation detection.
- **Pseudoternary** → Reverse of AMI.
- **Manchester** → Best synchronization, Highest bandwidth.
- **Differential Manchester** → Best synchronization + Polarity inversion immunity.
- 