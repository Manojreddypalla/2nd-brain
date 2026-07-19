# 📐 Physical Layer – Formula Sheet (GATE)

---

## 1. Wavelength

$$
\lambda=\frac{v}{f}
$$

Where,

- $\lambda$ = Wavelength (m)
- $v$ = Velocity of wave (m/s)
- $f$ = Frequency (Hz)

---

## 2. Time Period

$$
T=\frac{1}{f}
$$

---

## 3. Frequency

$$
f=\frac{1}{T}
$$

---

## 4. Bit Rate

$$
\text{Bit Rate}=\frac{\text{Number of Bits}}{\text{Time}}
$$

**Unit:** bps (bits per second)

---

## 5. Baud Rate

$$
\text{Baud Rate}=\frac{\text{Number of Signal Changes}}{\text{Time}}
$$

**Unit:** Baud (Symbols/sec)

---

## 6. Bit Rate and Baud Rate Relationship

$$
\boxed{\text{Bit Rate}=\text{Baud Rate}\times\log_2(L)}
$$

Where,

- $L$ = Number of Signal Levels

Equivalent formulas

$$
\boxed{\text{Baud Rate}=\frac{\text{Bit Rate}}{\log_2(L)}}
$$

$$
\boxed{L=2^{\frac{\text{Bit Rate}}{\text{Baud Rate}}}}
$$

---

## 7. Nyquist Theorem (Noise-Free Channel)

$$
\boxed{\text{Bit Rate}=2B\log_2(L)}
$$

Where,

- $B$ = Bandwidth (Hz)
- $L$ = Number of Signal Levels

### Binary Signaling ($L=2$)

$$
\boxed{\text{Bit Rate}=2B}
$$

---

## 8. Shannon Capacity (Noisy Channel)

$$
\boxed{C=B\log_2(1+\text{SNR})}
$$

Where,

- $C$ = Channel Capacity (bps)
- $B$ = Bandwidth (Hz)
- SNR = Signal-to-Noise Ratio (Linear)

---

## 9. SNR Conversion

### dB → Linear

$$
\boxed{\text{SNR}=10^{\frac{\text{SNR(dB)}}{10}}}
$$

### Linear → dB

$$
\boxed{\text{SNR(dB)}=10\log_{10}(\text{SNR})}
$$

---

## 10. Transmission Time

$$
\boxed{\text{Transmission Time}=\frac{\text{Data Size}}{\text{Bit Rate}}}
$$

Where,

- Data Size = bits
- Bit Rate = bps
- Time = seconds

---

## 11. Block Coding Efficiency

$$
\boxed{\text{Efficiency}=\frac{\text{Original Bits}}{\text{Encoded Bits}}\times100}
$$

### 4B/5B

$$
\boxed{\frac{4}{5}\times100=80\%}
$$

### 8B/10B

$$
\boxed{\frac{8}{10}\times100=80\%}
$$

---

# 📋 Formula Summary

| Formula | Expression |
|---------|------------|
| Wavelength | $\lambda=\frac{v}{f}$ |
| Time Period | $T=\frac{1}{f}$ |
| Frequency | $f=\frac{1}{T}$ |
| Bit Rate | $\frac{\text{Bits}}{\text{Time}}$ |
| Baud Rate | $\frac{\text{Signal Changes}}{\text{Time}}$ |
| Bit Rate | $\text{Baud}\times\log_2(L)$ |
| Nyquist | $2B\log_2(L)$ |
| Shannon | $B\log_2(1+\text{SNR})$ |
| SNR (Linear) | $10^{\frac{\text{SNR(dB)}}{10}}$ |
| SNR (dB) | $10\log_{10}(\text{SNR})$ |
| Transmission Time | $\frac{\text{Data Size}}{\text{Bit Rate}}$ |
| Block Coding Efficiency | $\frac{\text{Original Bits}}{\text{Encoded Bits}}\times100$ |

---

# ⭐ GATE Quick Facts

- **Nyquist → Noise-Free Channel**
- **Shannon → Noisy Channel**
- **Binary Signaling ⇒ $L=2$**
- **Bit Rate = Baud × $\log_2(L)$**
- **Always convert SNR (dB) → Linear before using Shannon**
- **4B/5B Efficiency = 80%**
- **8B/10B Efficiency = 80%**