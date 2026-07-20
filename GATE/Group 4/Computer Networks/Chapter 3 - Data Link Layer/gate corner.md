# GATE Corner ⭐⭐⭐⭐⭐
## Module 3.3 – Error Correction

These are the most frequently asked concepts from **Hamming Distance, Hamming Code, and Error Detection & Correction** in GATE.

---

# 1. Hamming Distance

## Definition

> Minimum number of bit changes required to convert one binary string into another.

Example

```
101101

100001
```

Different bits

```
2
```

Hamming Distance = **2**

---

## Formula

```
Hamming Distance = Number of differing bit positions
```

---

## Important Formula

If minimum Hamming Distance is

```
d
```

Then

### Detectable Errors

```
d - 1
```

### Correctable Errors

```
(d - 1) / 2
```

Take Floor Value

or

```
⌊(d-1)/2⌋
```

---

## Examples

```
d = 3

Detection = 2

Correction = 1
```

```
d = 5

Detection = 4

Correction = 2
```

```
d = 7

Detection = 6

Correction = 3
```

---

## Frequently Asked GATE Question

Minimum Hamming Distance required to

Correct 1-bit error?

```
3
```

Correct 2-bit errors?

```
5
```

Correct 3-bit errors?

```
7
```

---

# 2. Hamming Code

## Formula

```
2^r ≥ m + r + 1
```

where

```
m = Data Bits

r = Parity Bits
```

---

## Parity Bit Positions

Always powers of 2

```
1
2
4
8
16
32
...
```

---

## Coverage Pattern

### P1

```
Take 1

Skip 1
```

Checks

```
1 3 5 7 ...
```

---

### P2

```
Take 2

Skip 2
```

Checks

```
2 3 6 7 ...
```

---

### P4

```
Take 4

Skip 4
```

Checks

```
4 5 6 7 ...
```

---

### P8

```
Take 8

Skip 8
```

---

## Golden Rule ⭐

```
Parity Bit

Always

Checks Itself First
```

Never skip the parity bit.

---

## Syndrome Order

Always write

```
P8 P4 P2 P1
```

Example

```
P4 =1

P2 =0

P1 =1

↓

101₂

↓

5
```

Error at

```
Bit 5
```

---

## No Error Condition

```
Syndrome = 0000

↓

No Error
```

---

# 3. Error Detection vs Error Correction

## Error Detection

Answers

```
Did an error occur?
```

Examples

```
CRC

Checksum

VRC

LRC
```

Needs

```
Retransmission
```

---

## Error Correction

Answers

```
Did an error occur?

+

Where did it occur?
```

Example

```
Hamming Code
```

No retransmission

(for single-bit error)

---

## Comparison

| Error Detection | Error Correction |
|-----------------|------------------|
| Detect only | Detect + Correct |
| Retransmission | No retransmission |
| Less redundancy | More redundancy |
| Simpler | More complex |

---

# 4. Hamming Code Numericals

## Sender Steps

```
Find r

↓

Place Parity Bits

↓

Insert Data

↓

Calculate P1

↓

Calculate P2

↓

Calculate P4

↓

Transmit
```

---

## Receiver Steps

```
Receive Code

↓

Check P1

↓

Check P2

↓

Check P4

↓

Form Syndrome

↓

Binary to Decimal

↓

Locate Error

↓

Flip Bit
```

---

## Always Remember

```
Binary Syndrome

↓

Decimal Value

↓

Error Position
```

---

## If Syndrome = 000

```
No Error
```

---

## If Syndrome = 011

```
3

↓

Error at Bit 3
```

---

## If Syndrome = 101

```
5

↓

Error at Bit 5
```

---

## If Syndrome = 111

```
7

↓

Error at Bit 7
```

---

# Most Common GATE Traps ⚠️

❌ Using

```
2^r > m+r+1
```

Correct

```
2^r ≥ m+r+1
```

---

❌ Parity bits not placed at powers of two.

Correct

```
1

2

4

8

16
```

---

❌ Skipping parity bit while calculating parity.

Correct

```
Parity bit checks itself first.
```

---

❌ Writing syndrome as

```
P1 P2 P4
```

Correct

```
P8 P4 P2 P1
```

---

❌ Forgetting to convert syndrome to decimal.

Binary

↓

Decimal

↓

Error Position

---

# Super Revision (30 Seconds)

```
Hamming Distance

↓

Detect = d−1

Correct = ⌊(d−1)/2⌋
```

```
Hamming Code

↓

2^r ≥ m+r+1
```

```
Parity Positions

↓

1

2

4

8

16...
```

```
Parity checks itself first.
```

```
Syndrome

↓

P8 P4 P2 P1

↓

Decimal

↓

Error Position
```

```
CRC

↓

Detection Only
```

```
Hamming Code

↓

Detection + Correction
```

```
Standard Hamming Code

↓

Corrects 1-bit Error

Detects 2-bit Errors
```

---

# Expected GATE Questions ⭐⭐⭐⭐⭐

- Calculate the Hamming Distance between two codewords.
- Given **m**, find the number of parity bits using `2^r ≥ m+r+1`.
- Place parity bits correctly in a Hamming code.
- Compute the parity bits (even/odd parity).
- Find the syndrome from a received codeword.
- Identify the erroneous bit using the syndrome.
- Correct the received codeword.
- Determine how many errors can be **detected** or **corrected** from a given minimum Hamming distance.
- Compare **CRC vs Hamming Code** or **Error Detection vs Error Correction**.