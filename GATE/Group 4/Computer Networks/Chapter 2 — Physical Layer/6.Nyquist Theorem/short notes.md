---
title: Nyquist Bit Rate Theorem
subject: Computer Networks
module: Physical Layer
tags: [gate, cn, physical-layer, nyquist]
---

# 📘 Nyquist Bit Rate Theorem

## Definition

Nyquist Bit Rate Theorem gives the **maximum data rate** of a **noiseless communication channel** having bandwidth **B Hz**.

---

## Assumptions

- No Noise
- Ideal Channel
- Only Bandwidth limits communication
- Receiver can distinguish all signal levels perfectly

> [!important]
> Nyquist theorem is valid **only for noiseless channels**.

---

# Inter-Symbol Interference (ISI)

**Inter-Symbol Interference (ISI)** occurs when one transmitted symbol overlaps with the next due to the limited bandwidth of the communication channel.

### Effects

- Receiver cannot distinguish symbols correctly.
- Bit error rate increases.
- Communication becomes unreliable.

---

# Nyquist Maximum Baud Rate

Nyquist proved that the maximum symbol transmission rate is

$$
\boxed{\text{Maximum Baud Rate}=2B}
$$

where

- **B** = Channel Bandwidth (Hz)

**Unit:** Baud (Symbols/second)

---

# Bits per Symbol

Each symbol can represent multiple bits depending on the number of signal levels.

$$
\boxed{\text{Bits/Symbol}=\log_2L}
$$

where

- **L** = Number of Signal Levels

### Examples

| Signal Levels (L) | Bits/Symbol |
|-------------------|------------:|
| 2 | 1 |
| 4 | 2 |
| 8 | 3 |
| 16 | 4 |

---

# Nyquist Maximum Data Rate

Using

$$
\text{Data Rate}=\text{Baud Rate}\times\text{Bits/Symbol}
$$

Therefore,

$$
\boxed{\text{Maximum Data Rate}=2B\log_2L}
$$

where

- **B** = Bandwidth (Hz)
- **L** = Number of Signal Levels

---

# Special Case

For binary signaling,

$$
L=2
$$

Since

$$
\log_2(2)=1
$$

Therefore,

$$
\boxed{\text{Maximum Data Rate}=2B}
$$

---

# Important Observations

- Increasing **Bandwidth** increases the Baud Rate.
- Increasing **Signal Levels** increases Bits/Symbol.
- Increasing both increases the Maximum Data Rate.

---

# Limitations

Nyquist theorem assumes a **perfect communication channel**.

It does **not** consider:

- Thermal Noise
- Crosstalk
- Interference
- Attenuation

> [!warning]
> For **noisy channels**, Nyquist theorem cannot determine the actual channel capacity. Use the **Shannon Capacity Theorem**.

---

# Formula Sheet

### Maximum Baud Rate

$$
\boxed{2B}
$$

### Bits per Symbol

$$
\boxed{\log_2L}
$$

### Maximum Data Rate

$$
\boxed{2B\log_2L}
$$

---

# Common GATE Mistakes

❌ **Bandwidth = Data Rate**

- Wrong
- Bandwidth is measured in **Hz**
- Data Rate is measured in **bps**

---

❌ **Baud Rate = Data Rate**

- True **only when** \(L=2\)

Otherwise,

$$
\text{Data Rate}=\text{Baud Rate}\times\log_2L
$$

---

❌ **Nyquist works for noisy channels**

- Wrong
- Use **Shannon Capacity Theorem**

---

# Nyquist vs Shannon

| Nyquist | Shannon |
|----------|----------|
| Noiseless Channel | Noisy Channel |
| Depends only on Bandwidth | Depends on Bandwidth and SNR |
| Gives Maximum Baud/Data Rate | Gives Maximum Channel Capacity |

---

# PYQ Focus

Questions are commonly asked on:

- Maximum Baud Rate
- Maximum Data Rate
- Number of Signal Levels
- Difference between Baud Rate and Data Rate
- Nyquist vs Shannon
- Conceptual questions on noiseless channels

---

# Memory Trick

```text
Bandwidth (B)
      │
      ▼
Maximum Baud = 2B
      │
      ▼
Bits/Symbol = log₂L
      │
      ▼
Maximum Data Rate
      │
      ▼
2B log₂L
```

---

# Quick Revision

> [!summary]
>
> - Nyquist assumes a **Noiseless Channel**.
> - Maximum Baud Rate = **2B**
> - Bits/Symbol = **log₂L**
> - Maximum Data Rate = **2B log₂L**
> - If **L = 2**, then Maximum Data Rate = **2B**
> - For **Noisy Channels**, use **Shannon Capacity Theorem**.