# Performance Metrics — Quick Notes (GATE)

---

# 1. Data Rate (Bit Rate)

### Definition
Number of **bits transmitted per second**.

### Unit
- bps (bits per second)

### Formula

$$
\text{Bit Rate}=\frac{\text{Number of Bits}}{\text{Time}}
$$

### Key Points
- Measures transmission speed.
- Higher bit rate → Faster communication.

---

# 2. Baud Rate (Symbol Rate)

### Definition
Number of **symbols transmitted per second**.

### Unit
- Baud (Bd)

### Formula

$$
\text{Bit Rate}=\text{Baud Rate}\times\text{Bits per Symbol}
$$

or

$$
\text{Baud Rate}=\frac{\text{Bit Rate}}{\text{Bits per Symbol}}
$$

### Key Points
- One symbol may represent multiple bits.
- If 1 symbol = 1 bit:

$$
\text{Bit Rate}=\text{Baud Rate}
$$

---

# 3. Bandwidth

### Definition
Range of frequencies a channel can carry.

### Unit
- Hertz (Hz)

### Formula

$$
\text{Bandwidth}=f_{high}-f_{low}
$$

### Key Points
- Higher bandwidth → Higher data carrying capacity.
- Depends on the communication channel.

---

# 4. Bit Error Rate (BER)

### Definition
Ratio of incorrectly received bits to total transmitted bits.

### Formula

$$
BER=\frac{\text{Number of Error Bits}}{\text{Total Number of Bits Transmitted}}
$$

### Unit
- No unit (ratio or percentage)

### Key Points
- Lower BER → Better communication quality.
- Noise and interference increase BER.

---

# Formula Sheet ⭐

$$
\text{Bit Rate}=\frac{\text{Bits}}{\text{Time}}
$$

$$
\text{Bit Rate}=\text{Baud Rate}\times\text{Bits per Symbol}
$$

$$
\text{Baud Rate}=\frac{\text{Bit Rate}}{\text{Bits per Symbol}}
$$

$$
\text{Bandwidth}=f_{high}-f_{low}
$$

$$
BER=\frac{\text{Error Bits}}{\text{Total Bits}}
$$

---

# GATE Quick Facts ⭐

- **Bit Rate** → bits/sec (**bps**)
- **Baud Rate** → symbols/sec (**Baud**)
- **Bandwidth** → frequency range (**Hz**)
- **BER** → transmission error ratio
- **Bit Rate = Baud Rate × Bits/Symbol**
- **Bandwidth ≠ Data Rate**
- **Lower BER = Better communication**

---

# One-Line Revision

- **Data Rate** → Bits/sec
- **Baud Rate** → Symbols/sec
- **Bandwidth** → Frequency Range (Hz)
- **BER** → Error Bits / Total Bits