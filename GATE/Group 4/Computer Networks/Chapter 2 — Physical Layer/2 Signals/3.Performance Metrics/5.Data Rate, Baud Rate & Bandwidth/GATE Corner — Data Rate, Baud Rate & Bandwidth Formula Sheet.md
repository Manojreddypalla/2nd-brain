# 📘 GATE CN — Formula Corner
## Topic: Data Rate, Baud Rate & Bandwidth

> **Physical Layer | GATE Important ⭐⭐⭐⭐⭐**

---

# 1. Data Rate (Bit Rate)

### Formula

$$
\boxed{\text{Data Rate}=\text{Baud Rate}\times\text{Bits per Symbol}}
$$

### Unit

- bps
- kbps
- Mbps
- Gbps

### Meaning

> Number of **bits transmitted per second**.

---

# 2. Bits per Symbol

### Formula

$$
\boxed{\text{Bits/Symbol}=\log_2(M)}
$$

Where

- **M** = Number of Signal Levels

---

# 3. Combined Formula ⭐⭐⭐⭐⭐

Substituting,

$$
\boxed{\text{Data Rate}=\text{Baud Rate}\times\log_2(M)}
$$

---

# 4. Baud Rate

### Formula

$$
\boxed{\text{Baud Rate}=\frac{\text{Data Rate}}{\log_2(M)}}
$$

### Unit

- Baud
- Symbols/sec

### Meaning

> Number of **symbols transmitted per second**.

---

# 5. Signal Levels

### Formula

$$
\boxed{M=2^{(\text{Bits/Symbol})}}
$$

---

# 6. Bandwidth

### Formula

$$
\boxed{\text{Bandwidth}=f_H-f_L}
$$

Where

- $f_H$ = Highest Frequency
- $f_L$ = Lowest Frequency

### Unit

- Hz
- kHz
- MHz
- GHz

---

# 7. Efficiency (Bits per Symbol)

$$
\boxed{\frac{\text{Data Rate}}{\text{Baud Rate}}=\log_2(M)}
$$

---

# 📊 Signal Levels Table

| Signal Levels (M) | Bits/Symbol | Relation |
|------------------:|------------:|----------|
| 2 | 1 | Data Rate = Baud Rate |
| 4 | 2 | Data Rate = 2 × Baud Rate |
| 8 | 3 | Data Rate = 3 × Baud Rate |
| 16 | 4 | Data Rate = 4 × Baud Rate |
| 32 | 5 | Data Rate = 5 × Baud Rate |
| 64 | 6 | Data Rate = 6 × Baud Rate |

---

# ⚡ GATE Shortcuts

## Case 1

If

$$
M=2
$$

Then

- Bits/Symbol = 1
- Data Rate = Baud Rate

---

## Case 2

If

$$
M>2
$$

Then

- One Symbol carries Multiple Bits
- Data Rate > Baud Rate

---

## Case 3

If

$$
\text{Bits/Symbol}=n
$$

Then

$$
M=2^n
$$

---

# 🧠 Memory Flow

```text
Signal Levels (M)
        │
        ▼
Bits/Symbol = log₂(M)
        │
        ▼
Baud Rate × Bits/Symbol
        │
        ▼
Data Rate
```

---

# 🎯 Future Formulas (Next Topic)

## Nyquist Bit Rate (Noiseless Channel)

$$
\boxed{\text{Maximum Data Rate}=2B\log_2(M)}
$$

Where

- $B$ = Bandwidth (Hz)
- $M$ = Signal Levels

---

## Shannon Capacity (Noisy Channel)

$$
\boxed{C=B\log_2(1+\text{SNR})}
$$

Where

- $C$ = Channel Capacity
- $B$ = Bandwidth
- SNR = Signal-to-Noise Ratio (Linear)

---

# 🚀 15-Second Revision

- ✅ Data Rate = Baud Rate × log₂(M)
- ✅ Baud Rate = Data Rate ÷ log₂(M)
- ✅ Bits/Symbol = log₂(M)
- ✅ M = 2^(Bits/Symbol)
- ✅ Bandwidth = fH − fL
- ✅ M = 2 → Data Rate = Baud Rate
- ✅ M > 2 → Data Rate > Baud Rate
- ✅ Nyquist → 2B log₂(M)
- ✅ Shannon → B log₂(1 + SNR)