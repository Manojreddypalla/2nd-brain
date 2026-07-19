Perfect. Since you've already understood **Nyquist**, Shannon becomes much easier because it's essentially the **real-world version** of Nyquist.

---

# 📘 Shannon Capacity Theorem (GATE Quick Notes)

## Why do we need Shannon?

Nyquist assumes:

- No Noise
    
- Ideal Channel
    

But in reality, every communication channel has:

- Thermal Noise
    
- Crosstalk
    
- Interference
    
- Attenuation
    

Even if we increase the number of signal levels indefinitely, the receiver will eventually be unable to distinguish them because of noise.

> **Claude Shannon answered:** _What is the maximum data rate of a noisy communication channel?_

---

# Signal-to-Noise Ratio (SNR)

Before Shannon, understand **SNR**.

$$  
\boxed{\text{SNR}=\frac{\text{Signal Power}}{\text{Noise Power}}}  
$$

- Higher SNR → Better communication
    
- Lower SNR → More errors
    

---

## SNR in Decibels (dB)

In most GATE questions, SNR is given in **dB**.

$$  
\boxed{\text{SNR}_{dB}=10\log_{10}(\text{SNR})}  
$$

To convert back:

$$  
\boxed{\text{SNR}=10^{\frac{\text{SNR}_{dB}}{10}}}  
$$

### Common Values

|SNR (dB)|SNR|
|---|--:|
|0|1|
|10|10|
|20|100|
|30|1000|
|40|10000|

Memorize these—they appear frequently.

---

# Shannon Capacity Formula

Maximum channel capacity:

$$  
\boxed{C=B\log_2(1+\text{SNR})}  
$$

where

- (C) = Capacity (bps)
    
- (B) = Bandwidth (Hz)
    
- SNR = Signal-to-Noise Ratio (linear)
    

---

# Observations

### Increase Bandwidth

↑ Bandwidth

↓

↑ Capacity

---

### Increase SNR

↑ Signal Power

↓

↑ Capacity

---

### Increase Noise

↑ Noise

↓

↓ SNR

↓

↓ Capacity

---

# Limitations

Shannon gives the **theoretical maximum**.

It does **not** guarantee a practical communication system can achieve this exactly.

---

# Nyquist vs Shannon

|Nyquist|Shannon|
|---|---|
|Noiseless Channel|Noisy Channel|
|Depends on Bandwidth|Depends on Bandwidth + SNR|
|(2B\log_2L)|(B\log_2(1+\text{SNR}))|

---

# Formula Sheet

### Shannon Capacity

$$  
\boxed{C=B\log_2(1+\text{SNR})}  
$$

### SNR

$$  
\boxed{\frac{S}{N}}  
$$

### SNR (dB)

$$  
\boxed{10\log_{10}\left(\frac{S}{N}\right)}  
$$

---

# Example 1 (Easy)

Bandwidth = **3000 Hz**

SNR = **100**

Capacity?

### Solution

$$  
C=3000\log_2(101)  
$$

Since

$$  
\log_2(101)\approx6.66  
$$

$$  
C\approx3000\times6.66  
$$

$$  
\boxed{19980\text{ bps}}  
$$

≈ **20 kbps**

---

# Example 2 (Very Common)

Bandwidth = **4 kHz**

SNR = **30 dB**

Find capacity.

### Step 1

Convert dB

$$  
30\text{ dB}  
$$

↓

$$  
10^{30/10}=1000  
$$

---

### Step 2

Apply Shannon

$$  
C=4000\log_2(1001)  
$$

Since

$$  
\log_2(1001)\approx9.97  
$$

$$  
C\approx4000\times9.97  
$$

$$  
\boxed{39880\text{ bps}}  
$$

≈ **40 kbps**

---

# Example 3 (Concept)

Bandwidth remains constant.

SNR increases.

What happens?

✅ Capacity increases.

---

# Example 4 (Concept)

Bandwidth doubles.

Noise remains same.

Capacity?

✅ Capacity increases (roughly doubles if SNR stays constant).

---

# GATE Tricks ⚠️

❌ Use dB directly in Shannon formula.

Wrong.

Always convert:

$$  
20\text{ dB}=100  
$$

Then substitute **100**, **not 20**.

---

❌ Confuse Nyquist and Shannon.

Remember:

- **No Noise → Nyquist**
    
- **Noise/SNR → Shannon**
    

---

# Memory Trick

```text
No Noise
    │
    ▼
Nyquist
2B log₂L

Noise Present
    │
    ▼
Shannon
B log₂(1+SNR)
```

---

# GATE Practice Questions

### Q1

Bandwidth = **5 kHz**

SNR = **31**

Find capacity.

---

### Q2

Bandwidth = **2 kHz**

SNR = **15 dB**

Find capacity.

---

### Q3

A communication channel has **bandwidth = 4 kHz** and **SNR = 20 dB**. Find the Shannon capacity.

---

### Q4

A channel has **bandwidth = 10 kHz** and **SNR = 0 dB**. Find its capacity.

---

### Q5 (Concept)

Which parameter does **Nyquist** ignore but **Shannon** explicitly considers?

A. Bandwidth  
B. Signal Levels  
C. Noise (SNR)  
D. Baud Rate

---

## Answers

**Q1**

[  
C=5000\log_2(32)=5000\times5=\boxed{25\text{ kbps}}  
]

---

**Q2**

15 dB:

[  
SNR=10^{1.5}\approx31.62  
]

[  
C=2000\log_2(32.62)\approx2000\times5.03=\boxed{10.06\text{ kbps}}  
]

---

**Q3**

20 dB → (SNR=100)

[  
C=4000\log_2(101)\approx4000\times6.66=\boxed{26.64\text{ kbps}}  
]

---

**Q4**

0 dB → (SNR=1)

[  
C=10000\log_2(2)=10000\times1=\boxed{10\text{ kbps}}  
]

---

**Q5**

✅ **Answer: C (Noise / SNR)**

---

## ⭐ 30-Second Revision

- **Nyquist → Noiseless channel**
    
- **Shannon → Noisy channel**
    
- (C=B\log_2(1+\text{SNR}))
    
- Convert **dB → Linear SNR** before applying the formula.
    
- Higher **Bandwidth** or **SNR** ⇒ Higher channel capacity.