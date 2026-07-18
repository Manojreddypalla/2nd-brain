# GATE CN Notes — Data Rate, Baud Rate & Bandwidth

> **Module:** Physical Layer → Digital Transmission  
> **GATE Weightage:** ⭐⭐⭐⭐⭐ (Foundation for Nyquist, Shannon, Encoding & Modulation)

---

# 1. Big Picture (Don't Memorize, Visualize)

Think of communication like this:

```text
Application Data
       │
       ▼
Bits
       │
       ▼
Physical Layer
       │
Groups bits into Symbols
       │
       ▼
Electrical Signal (Voltage)
       │
       ▼
Transmission Medium (Wire/Fiber/Air)
```

The **Physical Layer does NOT send bits directly.**

It sends **symbols** (electrical signals).

A symbol may represent **1 bit, 2 bits, 3 bits, ...**

---

# 2. Three Important Terms

## A. Data Rate (Bit Rate)

### Definition

Number of **bits transmitted per second**.

### Unit

```text
bps
kbps
Mbps
Gbps
Tbps
```

### Measures

> **Amount of information transferred.**

### Think

> **How many bits reached the receiver every second?**

---

## B. Baud Rate (Symbol Rate)

### Definition

Number of **symbols transmitted per second**.

A **symbol** is **one physical signal** (voltage, phase, frequency, etc.).

### Unit

```text
Baud
or
Symbols/sec
```

### Measures

> **How many physical signals are sent every second?**

### Think

> **How many times did we transmit one symbol?**

---

## C. Bandwidth

### Definition

The **range of frequencies** that a communication channel can carry.

### Unit

```text
Hz
kHz
MHz
GHz
```

### Formula

```text
Bandwidth = Highest Frequency − Lowest Frequency
```

Example

```text
Channel supports

1000 Hz → 5000 Hz

Bandwidth

= 5000 − 1000

= 4000 Hz
```

### Think

> **How wide is the communication pipe?**

---

# 3. Mental Model

Imagine a highway.

```text
Bandwidth
=
Width of Highway
```

```text
Baud Rate
=
Number of Trucks/sec
```

```text
Data Rate
=
Boxes carried/sec
```

More boxes can fit in one truck.

Similarly,

One symbol can carry multiple bits.

---

# 4. What is a Symbol?

A symbol is **one transmitted signal**.

It can be

- Voltage level
    
- Phase
    
- Frequency
    
- Amplitude
    

depending on the modulation technique.

Example

Suppose

|Voltage|Bits|
|---|---|
|1V|00|
|2V|01|
|3V|10|
|4V|11|

Now sender wants to send

```text
11
```

Instead of sending

```text
1
1
```

it sends

```text
4V
```

Receiver gets

```text
4V

↓

11
```

One voltage

↓

One symbol

↓

Two bits

---

# 5. Data Rate vs Baud Rate

## Binary Signaling

Only two voltage levels.

```text
0
1
```

Each symbol carries

```text
1 bit
```

Example

```text
1010
```

Transmission

```text
1
0
1
0
```

Symbols

```text
4
```

Bits

```text
4
```

Therefore

```text
Baud Rate = Data Rate
```

---

## Multi-Level Signaling

Suppose

```text
00 → 1V

01 → 2V

10 → 3V

11 → 4V
```

Now

```text
10110010
```

becomes

```text
10

11

00

10
```

Signals sent

```text
3V

4V

1V

3V
```

Only

```text
4 symbols
```

Receiver converts back

```text
10110010
```

Result

```text
4 Symbols

↓

8 Bits
```

So

```text
Baud Rate

=

4
```

```text
Data Rate

=

8
```

---

# 6. Relationship

## Formula

```text
Data Rate

=

Baud Rate × Bits per Symbol
```

Since

```text
Bits/Symbol

=

log₂(M)
```

where

```text
M

=

Number of Signal Levels
```

Final Formula

```text
Data Rate

=

Baud Rate × log₂(M)
```

---

# 7. Signal Levels Table

|Signal Levels (M)|Bits/Symbol|
|---|---|
|2|1|
|4|2|
|8|3|
|16|4|
|32|5|
|64|6|

Notice

Whenever

```text
Signal Levels Double

↓

Bits/Symbol Increase by 1
```

---

# 8. Important Cases

### Binary

```text
M = 2

Bits/Symbol = 1

Data Rate = Baud Rate
```

---

### 4-Level

```text
M = 4

Bits/Symbol = 2

Data Rate

=

2 × Baud Rate
```

---

### 8-Level

```text
M = 8

Bits/Symbol = 3

Data Rate

=

3 × Baud Rate
```

---

### 16-Level

```text
M = 16

Bits/Symbol = 4
```

---

# 9. Comparison Table

|Feature|Data Rate|Baud Rate|Bandwidth|
|---|---|---|---|
|Meaning|Bits/sec|Symbols/sec|Frequency Range|
|Layer|Physical|Physical|Physical|
|Measures|Information|Signals|Channel Width|
|Unit|bps|Baud|Hz|
|Can Increase By|Better Encoding / More Levels / More Bandwidth|Faster Symbol Transmission|Better Channel|

---

# 10. Common Misconceptions

### ❌ Wrong

> One symbol always equals one bit.

### ✅ Correct

One symbol **may carry multiple bits.**

---

### ❌ Wrong

Baud Rate means packets/sec.

### ✅ Correct

Baud Rate means

```text
Symbols/sec
```

Not

- Packets
    
- Frames
    
- Segments
    

---

### ❌ Wrong

Bandwidth means Internet Speed.

### ✅ Correct

Bandwidth means

```text
Frequency Range
```

Internet speed is measured by

```text
Data Rate
```

---

# 11. GATE Tricks

### Trick 1

If

```text
M = 2
```

Immediately write

```text
Baud Rate = Data Rate
```

---

### Trick 2

Whenever

```text
M > 2
```

Immediately think

```text
One Symbol

↓

Multiple Bits
```

Therefore

```text
Data Rate > Baud Rate
```

---

### Trick 3

If question asks

> "How many signal changes?"

Think

```text
Baud Rate
```

If question asks

> "How many bits transferred?"

Think

```text
Data Rate
```

---

# 12. Connection with Future Topics

```text
Bandwidth
        │
        ▼
Nyquist Theorem
        │
        ▼
Maximum Baud Rate
        │
        ▼
Maximum Data Rate
        │
        ▼
Shannon Capacity
```

Today's topic is the foundation for:

- ✅ Line Encoding
    
- ✅ Digital Modulation
    
- ✅ Nyquist Bit Rate
    
- ✅ Shannon Capacity
    
- ✅ ASK, FSK, PSK, QAM
    

---

# 13. 30-Second Revision (Exam Sheet)

```text
DATA RATE
----------
• Bits/sec
• Unit: bps
• Information transferred

BAUD RATE
----------
• Symbols/sec
• Unit: Baud
• One symbol = One physical signal
• Symbol may carry multiple bits

BANDWIDTH
----------
• Frequency range
• Unit: Hz
• Channel capacity (frequency width)

FORMULAS
--------
Bits/Symbol = log₂(M)

Data Rate = Baud Rate × log₂(M)

M = Number of Signal Levels

IMPORTANT
---------
M = 2
→ Data Rate = Baud Rate

M > 2
→ Data Rate > Baud Rate

Remember:
Bandwidth = Pipe Width
Baud Rate = Symbols/sec
Data Rate = Bits/sec
```

## 🎯 Memory Trick (One Line)

> **Bandwidth decides how much frequency the channel offers. Baud Rate tells how many symbols you send each second. Data Rate tells how many bits those symbols actually carry.**

If you remember **Pipe → Symbols → Bits**, you'll be able to derive almost every GATE question on this topic instead of memorizing formulas.