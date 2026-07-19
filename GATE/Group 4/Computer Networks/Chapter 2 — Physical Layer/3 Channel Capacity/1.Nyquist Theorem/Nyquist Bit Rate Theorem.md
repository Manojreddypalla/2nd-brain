Perfect. Since this is for **GATE CSE**, here's a complete, clean, **Obsidian-ready** note. I've intentionally **not gone into advanced Fourier mathematics**, because that's beyond the syllabus. Instead, this explains exactly what you should understand for exams.

---

# 📘 Nyquist Bit Rate Theorem

## Why do we need Nyquist?

Suppose we have a communication channel.

Every channel has a **limited bandwidth**.

Example:

```text
0 Hz -------------------- B Hz
```

This means the channel can only carry frequencies up to **B Hz**.

It cannot reproduce signals that change infinitely fast.

---

# Problem

Suppose we keep sending symbols faster and faster.

Initially:

```text
|----A----|----B----|----C----|
```

Receiver easily distinguishes each symbol.

Now increase the speed.

```text
|-A-|-B-|-C-|-D-|
```

Still okay.

Increase even more.

```text
|||||||||||||||||||||
```

Now the channel cannot respond quickly enough.

The symbols begin to overlap.

The receiver becomes confused.

This phenomenon is called:

> **Inter-Symbol Interference (ISI)**

---

# Inter-Symbol Interference (ISI)

ISI occurs when one transmitted symbol affects the next symbol.

Example:

Ideal transmission

```text
____      ____
    |____|
```

After passing through a limited bandwidth channel

```text
~~~~/\~~~~/\~~~~
```

Instead of sharp pulses, the receiver gets rounded waves.

Neighboring symbols overlap.

Result:

- Receiver cannot determine where one symbol ends.
    
- Errors increase.
    

---

# Nyquist's Question

Harry Nyquist asked:

> **Given a channel of bandwidth B Hz and assuming there is NO noise, what is the maximum number of symbols that can be transmitted without ISI?**

---

# Nyquist's Assumptions

Nyquist theorem assumes an **ideal channel**.

- ✅ No Noise
    
- ✅ Perfect Receiver
    
- ✅ Perfect Transmitter
    
- ✅ Only Bandwidth limits communication
    

Because of these assumptions, Nyquist gives the **theoretical maximum**.

---

# Nyquist's Result

Nyquist mathematically proved:

[  
\boxed{\text{Maximum Baud Rate} = 2B}  
]

where

- B = Bandwidth (Hz)
    

Unit:

Baud (Symbols/sec)

---

# What does 2B mean?

Suppose

```text
Bandwidth = 3000 Hz
```

Then

```text
Maximum Baud Rate

= 2 × 3000

= 6000 Baud
```

Meaning:

The channel can transmit **at most 6000 symbols every second**.

Sending faster causes **Inter-Symbol Interference**.

---

# Why exactly 2B?

This is the most misunderstood part.

The rigorous proof uses:

- Fourier Transform
    
- Nyquist Pulse
    
- Sampling Theory
    
- Zero Inter-Symbol Interference
    

These topics are **outside the GATE CSE syllabus**.

For GATE, remember:

> Nyquist mathematically proved that a band-limited noiseless channel can support at most **2B symbols/sec**.

The factor **2** is **not guessed** and **not because we ignore the negative half of a sine wave**.

The "positive half + negative half" explanation is only an intuition to remember the formula.

---

# Maximum Data Rate

Previously we learned

# [  
\text{Data Rate}

\text{Baud Rate}  
\times  
\text{Bits/Symbol}  
]

Maximum Baud Rate

[  
=2B  
]

Bits per Symbol

[  
=\log_2(M)  
]

Therefore

[  
\boxed{\text{Maximum Data Rate}=2B\log_2(M)}  
]

---

# Variables

|Symbol|Meaning|Unit|
|---|---|---|
|B|Bandwidth|Hz|
|M|Signal Levels|—|
|Baud|Symbols/sec|Baud|
|Data Rate|Bits/sec|bps|

---

# Derivation

Known:

Maximum Baud Rate

[  
=2B  
]

Also,

# [  
\text{Data Rate}

\text{Baud Rate}  
\times  
\text{Bits/Symbol}  
]

Substitute

[  
=2B  
\times  
\log_2(M)  
]

Hence

[  
\boxed{\text{Maximum Data Rate}=2B\log_2(M)}  
]

---

# Special Case

If

[  
M=2  
]

then

[  
\log_2(2)=1  
]

Therefore

[  
\boxed{\text{Maximum Data Rate}=2B}  
]

---

# Example 1

Bandwidth

3000 Hz

Binary Signaling

(M=2)

Maximum Data Rate

# [  
2\times3000\times1

6000\text{ bps}  
]

---

# Example 2

Bandwidth

3000 Hz

Signal Levels

4

Bits/Symbol

[  
\log_2(4)=2  
]

Maximum Data Rate

# [  
2\times3000\times2

12000\text{ bps}  
]

---

# Example 3

Bandwidth

5000 Hz

Signal Levels

16

Bits/Symbol

[  
\log_2(16)=4  
]

Maximum Data Rate

# [  
2\times5000\times4

40000\text{ bps}  
]

---

# Observations

Increasing Bandwidth

↓

Higher Baud Rate

↓

Higher Data Rate

---

Increasing Signal Levels

↓

More Bits/Symbol

↓

Higher Data Rate

---

Increasing both

↓

Highest Data Rate

---

# Limitations

Nyquist assumes:

- No Noise
    
- Ideal Channel
    

Real communication channels contain:

- Thermal Noise
    
- Crosstalk
    
- Interference
    
- Attenuation
    

Therefore,

Nyquist gives only the **theoretical maximum**.

For noisy channels we use:

> **Shannon Capacity Theorem**

---

# Nyquist vs Previous Formula

Previous Formula

# [  
\boxed{\text{Data Rate}

\text{Baud Rate}  
\times  
\log_2(M)}  
]

Nyquist

# [  
\boxed{\text{Maximum Data Rate}

2B\log_2(M)}  
]

Difference:

Previous formula works when **Baud Rate is known**.

Nyquist calculates the **maximum possible Baud Rate** using Bandwidth.

---

# Formula Sheet

Maximum Baud Rate

[  
\boxed{2B}  
]

Bits/Symbol

[  
\boxed{\log_2(M)}  
]

Maximum Data Rate

[  
\boxed{2B\log_2(M)}  
]

---

# GATE Corner ⭐

### Remember

- Nyquist assumes **No Noise**.
    
- Only **Bandwidth** limits communication.
    
- Maximum Baud Rate = **2B**.
    
- Maximum Data Rate = **2B log₂(M)**.
    
- If **M = 2**, then Data Rate = **2B**.
    
- If noise is present, **Nyquist no longer gives the true maximum**—use **Shannon's theorem** instead.
    

---

# Memory Trick 🧠

```text
Bandwidth (B)
      │
      ▼
Nyquist
      │
Maximum Baud = 2B
      │
      ▼
Bits/Symbol = log₂(M)
      │
      ▼
Maximum Data Rate
      │
      ▼
2B log₂(M)
```

## 💡 Exam Tip

For **GATE CSE**, **do not waste time trying to prove why the factor is exactly (2B)**. The derivation comes from advanced communication theory (Fourier analysis and pulse shaping), which is outside the syllabus. Treat **(2B)** as Nyquist's proven result, understand **what it means**, know **its assumptions**, and be able to apply the formula correctly in numerical problems. This is exactly the level expected in GATE.