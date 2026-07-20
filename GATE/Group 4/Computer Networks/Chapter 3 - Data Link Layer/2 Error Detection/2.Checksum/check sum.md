# 3.2.3 Checksum

> [!info] Definition
> **Checksum** is an error detection technique in which the sender computes a value called the **checksum** by adding all data words using **1's complement arithmetic**. The checksum is transmitted along with the data. The receiver performs the same calculation to verify whether the data has been corrupted.

---

# Why is Checksum Needed?

VRC and LRC are simple but cannot detect many multiple-bit and burst errors.

Checksum provides **better error detection** with low computational cost and is widely used in Internet protocols such as **IP, TCP, UDP, and ICMP**.

---

# Basic Idea

Instead of adding a single parity bit, Checksum:

1. Divides the data into equal-sized words.
2. Adds all words using **1's complement addition**.
3. Takes the **1's complement** of the final sum.
4. Appends this value (Checksum) to the transmitted data.

The receiver repeats the same process.

If the final result is **all 1s**, the data is accepted.

Otherwise, an error is detected.

---

# Terminology

## Data Word

A fixed-size block of data.

Example (4-bit words)

```
1010
1100
0111
```

---

## Checksum

The **1's complement** of the sum of all data words.

---

## One's Complement

Flip every bit.

Example

| Binary | One's Complement |
|--------|------------------|
| 1010 | 0101 |
| 1111 | 0000 |
| 0000 | 1111 |

---

# Sender Algorithm

```
Divide Data into Equal Words

        │
        ▼

Add All Words
(1's Complement Addition)

        │
        ▼

Take 1's Complement

        │
        ▼

Checksum

        │
        ▼

Transmit Data + Checksum
```

---

# Receiver Algorithm

```
Receive Data + Checksum

        │
        ▼

Add All Words Including Checksum

        │
        ▼

Result = All 1s ?

      /      \
    Yes      No
     │        │
Accept     Error
```

---

# One's Complement Addition

Unlike normal binary addition,

if there is a carry beyond the Most Significant Bit (MSB),

the carry is **wrapped around** and added back to the Least Significant Bit (LSB).

Example

```
 1110
+0011
------
10001
```

Result

```
0001
```

Carry

```
1
```

Wrap-around addition

```
0001
+0001
------
0010
```

Final Sum

```
0010
```

---

# Example

Suppose the sender wants to transmit two 4-bit words.

```
1010

0101
```

### Step 1: Add

```
1010
0101
------
1111
```

### Step 2: Take One's Complement

```
1111

↓

0000
```

Checksum

```
0000
```

Sender transmits

```
1010

0101

0000
```

---

# Receiver Verification

Receiver adds

```
1010

0101

0000
```

Result

```
1111
```

Since the result is **all 1s**,

✅ Accept the data.

---

# Example with Carry

```
1110

0011
```

### Normal Addition

```
1110
0011
------
10001
```

Wrap-around carry

```
0001

+0001
------
0010
```

One's Complement

```
0010

↓

1101
```

Checksum

```
1101
```

---

# Error Detection Capability

Checksum detects

- ✅ Most single-bit errors
- ✅ Many multiple-bit errors
- ✅ Many burst errors

However,

Checksum cannot detect **every possible error pattern**.

CRC is still stronger.

---

# Advantages

- Better than VRC and LRC.
- Easy to compute.
- Low computational overhead.
- Widely used in networking.

---

# Disadvantages

- Cannot detect every error.
- Weaker than CRC.
- Does not correct errors.

---

# Applications

Checksum is used in

- IPv4 Header
- TCP
- UDP
- ICMP

---

# VRC vs LRC vs Checksum

| Feature | VRC | LRC | Checksum |
|----------|-----|-----|----------|
| Extra Information | 1 Parity Bit | 1 Parity Row | Checksum Word |
| Detects Single-Bit Errors | ✅ | ✅ | ✅ |
| Detects Multiple-Bit Errors | Limited | Better | Better |
| Detects Burst Errors | Poor | Moderate | Good |
| Complexity | Low | Medium | Medium |
| Used In | Simple Systems | Legacy Systems | Internet Protocols |

---

# Memory Trick

```
Parity

↓

Count 1s

Checksum

↓

Add Binary Numbers

↓

Take 1's Complement
```

---

# Summary

- Checksum divides data into equal-sized words.
- Uses **1's complement addition**.
- Takes the **1's complement** of the final sum.
- Receiver adds all words including the checksum.
- If the final result is **all 1s**, the data is accepted.
- Used in **IP, TCP, UDP, and ICMP**.

---

# Revision Box

## Formula

```
Checksum

=

1's Complement

(

Sum of All Data Words

)
```

## Sender

```
Data

↓

Add

↓

1's Complement

↓

Checksum
```

## Receiver

```
Data + Checksum

↓

Add

↓

All 1s ?

↓

Yes → Accept

No → Error
```

## Used In

- IPv4
- TCP
- UDP
- ICMP

## Better Than

- VRC
- LRC

## Weaker Than

CRC