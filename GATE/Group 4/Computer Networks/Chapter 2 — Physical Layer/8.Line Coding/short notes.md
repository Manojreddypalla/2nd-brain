# 🚀 GATE Corner – Line Coding

> [!tip] Exam Focus
> **PYQ Weight:** ⭐⭐⭐⭐⭐
> Frequently Asked:
> - Identify encoding waveform
> - Compare encoding schemes
> - Bandwidth
> - Synchronization
> - DC Component
> - Self-clocking
> - Error Detection

---

## 📌 Encoding Rules

| Encoding | Rule |
|----------|------|
| **NRZ-L** | Level represents Bit |
| **NRZ-I** | Transition represents **1** |
| **Manchester** | Middle Transition represents Bit |
| **Differential Manchester** | Beginning Transition represents Bit, Middle Transition Always Present |
| **AMI** | 1's Alternate (+V, -V) |
| **Pseudoternary** | 0's Alternate (+V, -V) |

---

## 📌 Voltage Levels

| Encoding | Voltage Levels |
|----------|----------------|
| Unipolar NRZ | +V, 0 |
| Unipolar RZ | +V, 0 |
| Polar NRZ-L | +V, -V |
| Polar NRZ-I | +V, -V |
| Polar RZ | +V, 0, -V |
| Manchester | +V, -V |
| Differential Manchester | +V, -V |
| AMI | +V, 0, -V |
| Pseudoternary | +V, 0, -V |

---

## 📌 Master Comparison

| Encoding | Bandwidth | Synchronization | DC Component | Error Detection |
|----------|-----------|-----------------|--------------|-----------------|
| Unipolar NRZ | ⭐ Low | ❌ Poor | High | ❌ |
| Unipolar RZ | ⭐⭐⭐ High | ✅ Better | High | ❌ |
| Polar NRZ-L | ⭐ Low | ❌ Poor | Low | ❌ |
| Polar NRZ-I | ⭐ Low | ✅ Better | Low | ❌ |
| Polar RZ | ⭐⭐⭐ High | ✅ Good | Low | ❌ |
| Manchester | ⭐⭐⭐ High | ⭐ Excellent | None | ❌ |
| Differential Manchester | ⭐⭐⭐ High | ⭐ Excellent | None | ❌ |
| AMI | ⭐⭐ Medium | ✅ Good | None | ✅ Bipolar Violation |
| Pseudoternary | ⭐⭐ Medium | ✅ Good | None | ✅ Bipolar Violation |

---

## 📌 Advantages & Disadvantages

| Encoding | Advantages | Disadvantages |
|----------|------------|---------------|
| Unipolar NRZ | Simple, Low BW | High DC, Poor Sync |
| Unipolar RZ | Better Sync | High BW, High DC |
| Polar NRZ-L | Low BW | Long runs lose Sync |
| Polar NRZ-I | Better for long 1's | Long 0's lose Sync |
| Polar RZ | Better Sync | High BW |
| Manchester | Self Clocking, No DC | High BW |
| Differential Manchester | Self Clocking, No DC, Polarity Inversion Resistant | High BW, Complex |
| AMI | No DC, Error Detection | Long 0's Problem |
| Pseudoternary | No DC, Error Detection | Long 1's Problem |

---

## 📌 Block Coding

| Scheme | Conversion | Efficiency |
|---------|------------|------------|
| **4B/5B** | 4 → 5 bits | 80% |
| **8B/10B** | 8 → 10 bits | 80% |

**Efficiency Formula**

$$
\text{Efficiency}=\frac{\text{Data Bits}}{\text{Encoded Bits}}\times100\%
$$

---

## 📌 Important Concepts

### Self Clocking
- ✅ Manchester
- ✅ Differential Manchester

---

### No DC Component
- Manchester
- Differential Manchester
- AMI
- Pseudoternary

---

### Polarity Inversion Resistant
- Differential Manchester

---

### Bipolar Violation Detection
- AMI
- Pseudoternary

---

### High Bandwidth
- Manchester
- Differential Manchester
- Polar RZ
- Unipolar RZ

---

### Low Bandwidth
- NRZ-L
- NRZ-I

---

### Best Synchronization
- Manchester
- Differential Manchester

---

## 📌 Memory Tricks

```text
NRZ-L  → Level = Bit

NRZ-I  → Transition = 1

Manchester
→ Middle decides Bit

Differential Manchester
→ Beginning decides Bit
→ Middle always transitions

AMI
→ 1's Alternate

Pseudoternary
→ 0's Alternate

4B/5B
→ 4 → 5

8B/10B
→ 8 → 10
```

---

## 🎯 Last Minute Revision (30 Seconds)

```text
NRZ-L  → Level = Bit
NRZ-I  → Transition = 1

Manchester
✓ Middle Transition
✓ Self Clocking
✓ No DC

Differential Manchester
✓ Beginning Transition
✓ Middle Always
✓ No DC
✓ Polarity Resistant

AMI
✓ 1's Alternate
✓ No DC
✓ Error Detection

Pseudoternary
✓ 0's Alternate
✓ No DC
✓ Error Detection

4B/5B = 80%
8B/10B = 80%
```