# 📘 GATE CN — Data Rate, Baud Rate & Bandwidth (Cheat Sheet)

---

# 1. Data Rate (Bit Rate)

### Definition

Number of **bits transmitted per second**.

### Unit

- bps
- kbps
- Mbps
- Gbps

### Formula

$$
\boxed{\text{Data Rate}=\text{Baud Rate}\times\log_2(M)}
$$

### Use When

- Find transmission speed.
- Given Baud Rate & Signal Levels.
- Asked "How many bits/sec?"

---

# 2. Baud Rate (Symbol Rate)

### Definition

Number of **symbols (signals) transmitted per second**.

### Unit

- Baud
- Symbols/sec

### Formula

$$
\boxed{\text{Baud Rate}=\frac{\text{Data Rate}}{\log_2(M)}}
$$

### Use When

- Find Symbols/sec.
- Given Data Rate & Signal Levels.

---

# 3. Bits per Symbol

### Definition

Number of bits represented by **one symbol**.

### Formula

$$
\boxed{\text{Bits/Symbol}=\log_2(M)}
$$

### Use When

- Find how many bits one signal carries.
- Given Signal Levels.

---

# 4. Signal Levels

### Definition

Number of distinct voltage (or signal) levels used.

### Formula

$$
\boxed{M=2^{(\text{Bits/Symbol})}}
$$

### Use When

- Given Bits/Symbol.
- Asked to find modulation levels.

---

# 5. Bandwidth

### Definition

Range of frequencies a channel can carry.

### Unit

- Hz
- kHz
- MHz
- GHz

### Formula

$$
\boxed{\text{Bandwidth}=f_H-f_L}
$$

### Use When

- Highest & Lowest frequencies are given.
- Asked for channel bandwidth.

---

# 📊 Quick Values

| Signal Levels (M) | Bits/Symbol |
|------------------:|------------:|
| 2 | 1 |
| 4 | 2 |
| 8 | 3 |
| 16 | 4 |
| 32 | 5 |
| 64 | 6 |
| 128 | 7 |
| 256 | 8 |

---

# 🎯 Formula Flow

Signal Levels

↓

$$
\log_2(M)
$$

↓

Bits/Symbol

↓

Baud Rate × Bits/Symbol

↓

Data Rate

---

# ⚡ GATE Tricks

### If

$$
M=2
$$

✅ Data Rate = Baud Rate

---

### If

$$
M>2
$$

✅ Data Rate > Baud Rate

---

### Remember

- **Data Rate** → Information (Bits/sec)
- **Baud Rate** → Signals (Symbols/sec)
- **Bandwidth** → Frequency Range (Hz)
- **Signal Levels** → Different Voltages

---

# 🔥 Common GATE Questions

| Given | Find | Formula |
|--------|------|----------|
| Baud + M | Data Rate | DR = BR × log₂(M) |
| Data Rate + M | Baud | BR = DR / log₂(M) |
| M | Bits/Symbol | log₂(M) |
| Bits/Symbol | M | 2ⁿ |
| Highest & Lowest Frequency | Bandwidth | fH − fL |

---

# 🚀 Memory Trick

**Bandwidth** → Width of the pipe

**Baud Rate** → Signals travelling in the pipe

**Data Rate** → Information carried by those signals