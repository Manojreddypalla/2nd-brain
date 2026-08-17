# Computer Networks — Lecture 7

## Framing, Error Detection, Parity & Hamming Distance

> **Scope:** These notes are based on the uploaded Lecture 7 PDF. I’ve compressed the 150-slide lecture into GATE-focused Obsidian notes while preserving the important examples, formulas, traps, and relationships. The lecture covers framing, error handling, repetition coding, parity checking, Hamming distance, error detection/correction, and related numerical questions.

---

# 1. Framing

## Why framing?

At the physical layer, data is essentially a **continuous stream of bits**.

Example:

```text
010101001101010100101010101...
```

The receiver needs to know:

- Where does one frame start?
    
- Where does one frame end?
    

### Framing

> **Framing = breaking a continuous stream of bits into discrete chunks called frames.**

Think:

```text
Continuous bit stream
───────────────────────────────>

       Frame 1       Frame 2       Frame 3
     |----------|  |----------|  |----------|
```

The lecture introduces framing before moving to error handling.

---

# 2. Error Handling

During transmission:

```text
Sender ───────────────> Receiver
          noisy channel
```

Noise can modify transmitted bits.

Example:

```text
Sent:     10101
Received: 10001
              ↑
            error
```

### Error handling

The purpose is:

> **Detect and/or correct errors in received frames.**

The lecture divides error control into:

```text
Error Control
     │
     ├── Error Detection
     │      ├── Parity
     │      ├── CRC
     │      └── Checksum
     │
     └── Error Correction
            └── Hamming codes
```

The lecture explicitly places **Parity, CRC and Checksum** under detection and **Hamming codes** under correction.

---

# 3. Types of Errors

Two important types:

## 3.1 Single-bit error

Only one bit is corrupted.

```text
Sent:     00000100
Received: 00001100
               ↑
```

Only one position changed.

---

## 3.2 Burst error

Multiple bits are corrupted within a span.

```text
Sent:     001000001100
Received: 001111001100
             ^^^^
```

The lecture distinguishes single-bit and burst errors explicitly.

> **GATE idea:** Error detection schemes don't necessarily detect _every possible pattern_ of errors. Their capability depends on the structure of the code.

---

# 4. Coding the Data

The lecture mentions two approaches:

```text
Coding
├── Block Coding
└── Convolution Coding
```

The lecture marks **convolution coding as not in syllabus**.

So for this lecture, focus on:

> **Block coding**

---

# 5. Block Coding

The basic idea:

> Add extra bits to the original data so that errors can be detected/corrected.

Suppose:

```text
k data bits
```

We append:

```text
(n-k) additional bits
```

Therefore:

```text
┌───────────────┬─────────────────┐
│   k bits      │   (n-k) bits    │
│   message     │   extra bits    │
└───────────────┴─────────────────┘
       data           parity/check
```

The lecture notes that these additional bits can be placed at the **start, middle, end, or anywhere** in the block.

---

## Number of possible data words

If there are `k` data bits:

[  
\boxed{\text{Number of possible data words}=2^k}  
]

Example:

```text
k = 3
```

Then:

[  
2^3=8  
]

possible data words.

This appears explicitly in the lecture.

---

# 6. Codeword

After adding extra/check bits, the resulting sequence is called a:

> **Codeword**

Example:

```text
Data word
   ↓
1011
   ↓ + parity/check bits
10110
   ↓
Codeword
```

So:

```text
Data word ≠ Codeword
```

The **data word** contains the original information.

The **codeword** contains:

```text
data + redundancy
```

This distinction is important for GATE.

---

# 7. Repetition Coding

The lecture first tries a very simple error-detection idea:

> **Send the same data multiple times.**

This is called the **repetition approach**.

Suppose the packet is:

```text
00
```

Repeat it three times:

```text
00 00 00
```

Therefore:

|Data|Codeword|
|---|---|
|`00`|`00 00 00`|
|`01`|`01 01 01`|
|`10`|`10 10 10`|
|`11`|`11 11 11`|

The receiver knows these four sequences are the **valid codewords**.

---

# 8. How Repetition Detects Errors

Suppose:

```text
Sent:
00 00 00
```

Received:

```text
01 00 00
```

The receiver compares the received sequence with the list of valid codewords.

Valid:

```text
00 00 00
01 01 01
10 10 10
11 11 11
```

Received:

```text
01 00 00
```

It doesn't match any valid codeword.

Therefore:

[  
\boxed{\text{Error detected}}  
]

The receiver doesn't need to know exactly **which bit** was wrong; it only knows that the received sequence is invalid.

---

# 9. Valid vs Invalid Codeword

This is one of the most important ideas in the entire lecture.

The receiver has a set:

[  
C={\text{all valid codewords}}  
]

If:

```text
Received word ∈ C
```

then receiver says:

> Valid codeword → **No error detected**

If:

```text
Received word ∉ C
```

then:

> Invalid codeword → **Error detected**

### But here's the trap

```text
Valid → Invalid
```

is detectable.

But:

```text
Valid → another Valid
```

is **not detectable**.

Why?

Because the receiver has no way of knowing which valid codeword was originally transmitted.

The lecture demonstrates exactly this case with repetition coding.

---

# 10. Limitation of Repetition

Suppose:

```text
Sent:
00 00 00
```

and **all three copies are corrupted**:

```text
01 01 01
```

But:

```text
01 01 01
```

is itself a valid codeword!

Therefore the receiver says:

```text
"No error."
```

even though an error occurred.

This is an **undetectable error**.

### Why?

Because:

```text
valid codeword → valid codeword
```

The receiver cannot distinguish it from a legitimate transmission.

The lecture explicitly notes that one- and two-bit errors are detectable, but not all three-bit errors.

---

# 11. Problems with Repetition Coding

### Advantages

- Simple.
    
- Gives an improved chance of correct reception.
    
- Detects many errors.
    

### Disadvantages

- Huge overhead.
    
- Same information is transmitted repeatedly.
    
- We don't necessarily know when the correct message has been received.
    

The lecture lists these limitations.

---

# 12. Parity Checking

A better method is:

> Add **one extra bit**, called the **parity bit**.

Structure:

```text
Data bits + Parity bit
```

The parity bit is selected according to a rule.

There are two types:

```text
Even parity
Odd parity
```

---

# 13. Even Parity

For **even parity**:

> Total number of `1`s in the complete codeword must be even.

Example:

```text
Data = 1101010
```

Number of 1s:

```text
1 1 0 1 0 1 0
↑ ↑   ↑   ↑
4 ones
```

Already even.

Therefore:

```text
Parity bit = 0
```

Codeword:

```text
11010100
```

The lecture gives the same principle: the total number of `1`s including the parity bit must be even.

---

# 14. Odd Parity

For **odd parity**:

> Total number of `1`s in the complete codeword must be odd.

If the data already contains an odd number of `1`s:

```text
Parity bit = 0
```

If data contains an even number of `1`s:

```text
Parity bit = 1
```

So:

|Number of 1s in data|Even parity bit|Odd parity bit|
|--:|--:|--:|
|Even|0|1|
|Odd|1|0|

---

# 15. XOR Form of Parity

For even parity:

[  
\boxed{P=D_1\oplus D_2\oplus\cdots\oplus D_k}  
]

Why?

XOR gives:

```text
0 ⊕ 0 = 0
0 ⊕ 1 = 1
1 ⊕ 0 = 1
1 ⊕ 1 = 0
```

Therefore:

[  
P = \text{parity of number of 1s}  
]

The lecture explicitly represents parity using XOR.

---

# 16. Parity Checking at Receiver

### Sender

```text
Data
  ↓
Calculate parity
  ↓
Data + parity
  ↓
Transmit
```

### Receiver

```text
Received codeword
        ↓
Recalculate parity
        ↓
Compare/check
        ↓
Error / No error
```

Conceptually:

[  
R' = f(D)  
]

and compare it with the received check information.

The lecture illustrates this sender/receiver process.

---

# 17. What Can Single Parity Detect?

This is a **very important GATE point**.

Suppose even parity is used.

### One bit changes

Example:

```text
Original:
10110010
```

Number of `1`s has some required parity.

Flip one bit:

```text
10100010
```

Parity changes.

Therefore:

[  
\boxed{\text{Single-bit error is always detected}}  
]

The lecture confirms this.

---

# 18. Multiple Errors with Parity

The key is **number of flipped bits**, not their physical positions.

### Odd number of errors

Example:

```text
1 error
3 errors
5 errors
7 errors
...
```

Parity changes.

Therefore:

[  
\boxed{\text{All odd-number-of-bit errors are detected}}  
]

### Even number of errors

Example:

```text
2 errors
4 errors
6 errors
...
```

Parity can remain unchanged.

Therefore:

[  
\boxed{\text{Even-number-of-bit errors may go undetected}}  
]

---

## ⚠️ GATE Trap: "odd positions"

The lecture question mentions burst errors and "odd positions," but the underlying principle is:

> **Parity depends on whether the NUMBER OF CORRUPTED BITS is odd or even.**

It does **not** mean:

> "The corrupted bits happen to be located at positions 1, 3, 5, 7."

That distinction matters.

---

# 19. Single Parity Cannot Correct

Suppose:

```text
Received codeword has wrong parity.
```

We know:

> **An error occurred.**

But do we know:

```text
Which bit changed?
```

No.

Example:

```text
10110010
↑
Could be any of the 8 positions
```

Parity only tells us:

```text
Error exists
```

not:

```text
Error is at position 4
```

Therefore:

[  
\boxed{\text{Single parity detects but does not correct}}  
]

The lecture explicitly marks the correction claim as false.

---

# 20. Summary of Single Parity

|Property|Single parity|
|---|---|
|Detect 1-bit error|✅|
|Detect all odd-number errors|✅|
|Detect all even-number errors|❌|
|Correct error|❌|
|Minimum Hamming distance|**2**|

The lecture later derives:

[  
\boxed{d_{\min}=2}  
]

for single parity.

---

# 21. Hamming Distance

Now the lecture moves to the central concept required for understanding error detection/correction.

## Definition

> **Hamming distance between two equal-length words = number of corresponding bit positions in which they differ.**

Written as:

[  
\boxed{d(x,y)=\text{number of differing bit positions}}  
]

Example:

```text
000
011
```

Differences:

```text
0 0 0
0 1 1
  ↑ ↑
```

Therefore:

[  
d(000,011)=2  
]

Another example:

```text
10101
11110
```

Count the differing positions:

[  
d=3  
]

The lecture gives these examples directly.

---

# 22. How to Calculate Hamming Distance

Simply compare bit-by-bit.

Example:

```text
01101010
11010111
```

Write vertically:

```text
0 1 1 0 1 0 1 0
1 1 0 1 0 1 1 1
↑   ↑ ↑ ↑ ↑   ↑
```

Count differences.

The lecture's example gives:

[  
\boxed{d=4}  
]

---

# 23. Minimum Hamming Distance

A code contains multiple valid codewords.

Example:

[  
C={c_1,c_2,c_3,\ldots}  
]

Calculate distances between valid codewords.

The **minimum Hamming distance** is:

[  
\boxed{d_{\min}=\min_{c_i\neq c_j}d(c_i,c_j)}  
]

In words:

> The smallest Hamming distance between any two valid codewords.

The lecture defines it exactly this way.

---

# 24. Why Minimum Hamming Distance Matters

Imagine valid codewords as points:

```text
Valid A ●────────────● Valid B
             d
```

A transmitted valid codeword can be changed by noise.

If the received word lands:

```text
Valid → Invalid
```

we can detect the error.

But if it lands:

```text
Valid A → Valid B
```

the receiver cannot know that an error occurred.

Therefore, the distance between valid codewords determines how many errors can be tolerated.

---

# 25. Example: Code Set

Consider:

[  
C={0000,0011,0101,0110,1001,1010,1100,1111}  
]

The lecture asks for the minimum Hamming distance and obtains:

[  
\boxed{d_{\min}=2}  
]

---

# 26. Valid and Invalid Codewords

Imagine:

```text
Valid ●        ● Valid
       \       /
        ○ ○ ○
      Invalid
```

If a transmitted valid codeword changes into an invalid word:

```text
Valid → Invalid
```

the receiver can detect the error.

But:

```text
Valid → Valid
```

means the receiver accepts it.

Therefore:

> **Detection depends on whether corruption can move one valid codeword into another valid codeword.**

This is the central intuition behind `dmin`.

---

# 27. Example with Valid Codewords

Suppose:

```text
Sender:
0101001
```

Receiver:

```text
0101000
```

If the received word is **not a valid codeword**, then:

[  
\boxed{\text{Error detected}}  
]

But if the received word itself belongs to the valid-codeword set:

[  
\boxed{\text{Error cannot be detected}}  
]

The lecture demonstrates both cases.

---

# 28. Detection Capability

Suppose:

[  
d_{\min}=4  
]

Then two valid codewords are at least 4 bit positions apart.

Imagine starting from one valid codeword:

```text
Valid A
```

Flip bits:

```text
1 flip → invalid
2 flips → invalid
3 flips → invalid
4 flips → potentially another valid
```

Therefore:

[  
\boxed{\text{Maximum guaranteed detectable errors}=d_{\min}-1}  
]

So:

[  
d_{\min}=4  
\Rightarrow  
3\text{ errors detectable}  
]

The lecture illustrates this progression.

---

# 29. General Error Detection Formula

To guarantee detection of up to `s` errors:

[  
\boxed{d_{\min}\ge s+1}  
]

or:

[  
\boxed{s=d_{\min}-1}  
]

### Example

Want to detect:

```text
5 errors
```

Need:

[  
d_{\min}\ge5+1  
]

[  
\boxed{d_{\min}\ge6}  
]

The lecture explicitly derives:

[  
d_{\min}=s+1  
]

for guaranteed detection.

---

# 30. Why `dmin - 1`?

Suppose:

[  
d_{\min}=4  
]

Two valid codewords:

```text
A ●────────────● B
       4
```

You can move at most 3 positions away from A without reaching another valid codeword.

Therefore:

```text
1 error → invalid
2 errors → invalid
3 errors → invalid
4 errors → may reach valid
```

Hence:

[  
\boxed{3=d_{\min}-1}  
]

is guaranteed detection capability.

---

# 31. Single Parity and Hamming Distance

Single parity has:

[  
\boxed{d_{\min}=2}  
]

Therefore detection capability:

[  
d_{\min}-1=1  
]

So:

[  
\boxed{\text{Single parity guarantees detection of 1-bit error}}  
]

This is why the lecture's earlier statement about single parity follows directly from Hamming distance.

---

# 32. Error Correction

Detection asks:

> **Did an error happen?**

Correction asks:

> **Which valid codeword was originally transmitted?**

This requires the receiver to identify the nearest valid codeword.

The lecture states that correction is possible when we can be sufficiently sure about the original codeword.

---

# 33. Intuition Behind Error Correction

Suppose:

```text
Valid A ●────────────● Valid B
          distance 5
```

If the receiver gets a word close to A:

```text
A ●───○
```

it can infer:

> This corrupted word probably came from A.

But this only works if the corrupted word isn't equally close to another valid codeword.

That's why we need enough separation between valid codewords.

---

# 34. Correction Capability

To guarantee correction of up to `t` errors:

[  
\boxed{d_{\min}\ge2t+1}  
]

Therefore:

[  
\boxed{t=\left\lfloor\frac{d_{\min}-1}{2}\right\rfloor}  
]

---

# 35. Why `2t+1`?

Suppose you want to correct:

[  
t=2  
]

errors.

You need:

[  
d_{\min}\ge2(2)+1  
]

[  
\boxed{d_{\min}\ge5}  
]

Why?

Because the received word can be at distance 2 from the original codeword.

For another valid codeword to **not** be equally close, the valid codewords must be separated by at least:

```text
2 + 1 + 2
```

= 5.

Mental picture:

```text
Valid A ●──○○──R──○○──● Valid B
          2       2
```

The received word `R` is still closer to A than B.

---

# 36. Important Formula Table

|Goal|Required minimum Hamming distance|
|---|--:|
|Detect up to `s` errors|(\boxed{s+1})|
|Correct up to `t` errors|(\boxed{2t+1})|
|Correct up to `t` and detect additional errors|see combined case below|

The lecture explicitly gives these relationships.

---

# 37. Detection vs Correction

This distinction is **extremely important for GATE**.

### Detection

If:

[  
d_{\min}=d  
]

then:

[  
\boxed{d-1}  
]

errors can always be detected.

### Correction

If:

[  
d_{\min}=d  
]

then:

[  
\boxed{\left\lfloor\frac{d-1}{2}\right\rfloor}  
]

errors can always be corrected.

---

## Example

Suppose:

[  
d_{\min}=7  
]

Detection:

[  
7-1=6  
]

Correction:

[  
\left\lfloor\frac{7-1}{2}\right\rfloor=3  
]

Therefore:

```text
Detect → 6 errors
Correct → 3 errors
```

---

# 38. Important GATE Trap

If:

[  
d_{\min}=4  
]

don't say:

> "Can correct 4 errors."

Wrong.

You can guarantee:

[  
\left\lfloor\frac{4-1}{2}\right\rfloor=1  
]

So:

```text
Detect → 3
Correct → 1
```

---

# 39. Detection vs Correction — Visual

For:

[  
d_{\min}=5  
]

```text
Valid A ●──○──○──R──○──○──● Valid B
          ← 2 → ← 2 →
```

A received word can move at most 2 bits from A while remaining uniquely closer to A.

Therefore:

[  
t=\frac{5-1}{2}=2  
]

Correct up to 2 errors.

But:

[  
5-1=4  
]

errors can be detected.

So:

[  
\boxed{\text{Detect 4, Correct 2}}  
]

---

# 40. If `dmin = 8`

Lecture example:

[  
d_{\min}=8  
]

Number of invalid words between two valid codewords:

[  
8-1=7  
]

Therefore:

### Detection

[  
\boxed{7}  
]

### Correction

[  
\left\lfloor\frac{7}{2}\right\rfloor=3  
]

Therefore:

[  
\boxed{\text{Correct 3, Detect 7}}  
]

The lecture explicitly works through this example.

---

# 41. If `dmin = 15`

Lecture example:

[  
d_{\min}=15  
]

Detection:

[  
15-1=14  
]

Correction:

[  
\frac{15-1}{2}=7  
]

Therefore:

[  
\boxed{\text{Detect 14, Correct 7}}  
]

The lecture derives exactly this.

---

# 42. If You Want to Correct `s` Errors

Suppose you want:

[  
s=\text{number of errors to correct}  
]

Required:

[  
\boxed{d_{\min}=2s+1}  
]

Example:

```text
Want to correct 5 errors
```

Then:

[  
d_{\min}=2(5)+1  
]

[  
\boxed{d_{\min}=11}  
]

The lecture uses a NIELIT question asking exactly this and obtains 11.

---

# 43. If You Want to Detect `s` Errors

Required:

[  
\boxed{d_{\min}=s+1}  
]

Example:

```text
Want to detect 5 errors
```

Then:

[  
d_{\min}=6  
]

---

# 44. If You Want to Correct `c` and Detect `d`

The lecture's common combined case:

> A code corrects `c` errors and detects `d` errors, where (d\ge c).

Then:

[  
\boxed{d_{\min}=d+c+1}  
]

Why?

You need:

- `c` distance on one side for correction.
    
- `d` distance to ensure detection boundary.
    
- one extra separating position.
    

So:

[  
\boxed{d_{\min}\ge d+c+1}  
]

The lecture gives this as the common case.

---

# 45. Example: Correct 2 and Detect 4

Suppose:

```text
Correct = 2
Detect = 4
```

Then:

[  
d_{\min}=2+4+1  
]

[  
\boxed{d_{\min}=7}  
]

This matches the lecture's discussion around `dmin = 7`.

---

# 46. Example: Correct 5 Errors

Want:

[  
t=5  
]

Then:

[  
d_{\min}=2t+1  
]

[  
=2(5)+1  
]

[  
\boxed{11}  
]

Therefore:

```text
dmin = 11
```

can correct up to:

```text
5 errors
```

and detect up to:

```text
10 errors
```

The lecture's MSQ confirms:

- Detecting 11 errors → ❌
    
- Detecting 5 errors → ✅
    
- Correcting 5 errors → ✅
    
- Correcting 11 errors → ❌
    

Answer:

[  
\boxed{B,C}  
]

---

# 47. "Can Detect `dmin-1`" Does NOT Mean "Can Correct `dmin-1`"

This is a classic mistake.

For:

[  
d_{\min}=11  
]

Detection:

[  
10  
]

Correction:

[  
5  
]

So:

```text
Detect 10
Correct 5
```

NOT:

```text
Detect 10
Correct 10
```

---

# 48. Codewords as Points

A very useful mental model:

Imagine every valid codeword is a point in a huge space.

Example:

```text
          ● A
        / 
      ○ ○ ○
    /
  ● B
```

`dmin` is the minimum distance between any two valid points.

Noise moves the transmitted point.

### Detection

As long as the received word hasn't reached another valid point:

```text
Valid → Invalid
```

we detect it.

### Correction

The receiver chooses the nearest valid point.

Therefore:

> **Larger `dmin` means stronger error protection.**

---

# 49. Invalid Codewords

Suppose:

[  
d_{\min}=6  
]

Between two valid codewords:

```text
A ● ○ ○ ○ ○ ○ ● B
```

There are:

[  
6-1=5  
]

intermediate Hamming-distance layers.

So:

[  
\boxed{\text{5 invalid layers between the two valid codewords}}  
]

The lecture uses this visualization to derive correction/detection capabilities.

---

# 50. Channel Property

The lecture also considers cases where the **channel property is known**.

Example:

> Channel guarantees that at most **one bit** can be corrupted.

Then even if:

[  
d_{\min}=3  
]

a received word one bit away from a valid codeword can be mapped back to that codeword.

The channel assumption gives additional information.

This is important in the lecture's correction examples around pages 94–110.

---

# 51. Why Channel Property Matters

Suppose:

```text
Valid A ●────────● Valid B
           3
```

Received:

```text
R
```

If the channel guarantees:

```text
at most 1 bit error
```

then the receiver knows:

```text
R must have come from a valid codeword
within distance 1.
```

If only A is within distance 1:

[  
\boxed{\text{A can be uniquely identified}}  
]

Therefore the error can be corrected.

---

# 52. But If Channel Property Is Unknown...

Suppose the received word could have:

```text
1 bit error
2 bits error
3 bits error
...
```

Then multiple valid codewords may become possible.

You cannot simply assume:

> "Choose the closest one."

You need the code's guaranteed correction capability.

The lecture emphasizes that knowing the channel property can change whether correction is possible.

---

# 53. "Exactly `t` Errors" vs "Up to `t` Errors"

This is a subtle but important distinction.

### Up to `t`

Means:

```text
0, 1, 2, ..., t errors
```

### Exactly `t`

Means:

```text
t errors only
```

For example:

> "Channel has exactly 2 bit errors."

is different from:

> "Channel can have up to 2 bit errors."

The lecture uses this distinction in its examples involving a known channel property.

---

# 54. Example: Four 5-bit Codewords

The lecture gives:

```text
A = 01100
B = 11010
C = 10101
D = 00011
```

The minimum Hamming distance is:

[  
\boxed{3}  
]

Therefore:

### Detection

[  
3-1=2  
]

So:

[  
\boxed{\text{Detect up to 2 errors}}  
]

### Correction

[  
\left\lfloor\frac{3-1}{2}\right\rfloor=1  
]

So:

[  
\boxed{\text{Correct up to 1 error}}  
]

The lecture explicitly marks this as the code's Hamming-distance property.

---

# 55. Example: Received `11101`

Using the above code:

```text
A = 01100
B = 11010
C = 10101
D = 00011
```

Suppose:

```text
Received = 11101
```

Compare distances.

The lecture shows that `A` and `C` are candidates under the **exactly two-bit error** assumption, and discusses which codeword can be identified depending on the channel property.

The key GATE lesson:

> **Never solve a correction question without first checking what the channel guarantees.**

---

# 56. "At Most One Bit Error" Example

If the channel guarantees:

[  
\text{at most 1 bit error}  
]

and:

[  
d_{\min}\ge3  
]

then one-bit errors can be corrected.

Why?

Any received word within distance 1 of a valid codeword cannot simultaneously be within distance 1 of another valid codeword.

---

# 57. Minimum Hamming Distance — Master Formula

## Detection

To detect up to `s` errors:

[  
\boxed{d_{\min}\ge s+1}  
]

---

## Correction

To correct up to `t` errors:

[  
\boxed{d_{\min}\ge2t+1}  
]

---

## Detection + Correction

To correct `c` errors and detect `d` errors:

[  
\boxed{d_{\min}\ge c+d+1}  
]

for the common case (d\ge c).

---

# 58. Code Design Perspective

You can think of designing a code as:

```text
Choose valid codewords
        ↓
Keep them far apart
        ↓
Increase dmin
        ↓
Increase error protection
```

But increasing distance generally requires more redundancy / fewer usable data words.

So there is a trade-off:

```text
More redundancy
      ↓
larger separation
      ↓
better error protection
```

---

# 59. Repetition Code Revisited Through Hamming Distance

Repetition code:

```text
00 → 00 00 00
01 → 01 01 01
10 → 10 10 10
11 → 11 11 11
```

Compare:

```text
00 00 00
01 01 01
```

All three groups differ:

[  
d=3  
]

Similarly every pair of valid codewords differs in 3 positions.

Therefore:

[  
\boxed{d_{\min}=3}  
]

Thus:

### Detection

[  
3-1=2  
]

### Correction

[  
\left\lfloor\frac{3-1}{2}\right\rfloor=1  
]

So the repetition code can:

[  
\boxed{\text{Detect 2 errors and correct 1 error}}  
]

This ties the first part of the lecture directly to Hamming distance.

---

# 60. Single Parity Revisited Through Hamming Distance

Single parity code has:

[  
d_{\min}=2  
]

Therefore:

```text
Detection = 2 - 1 = 1
Correction = floor((2-1)/2) = 0
```

So:

[  
\boxed{\text{Detect 1, Correct 0}}  
]

This explains **why** single parity cannot correct an error.

---

# 61. Two-Out-of-Five Code

The lecture gives a code consisting of all binary words of length 5 containing exactly two `1`s.

Number of codewords:

[  
\binom52  
]

[  
=\frac{5!}{2!3!}  
]

[  
\boxed{10}  
]

The lecture lists the ten codewords.

---

# 62. Minimum Distance of Two-Out-of-Five Code

Any two valid codewords each contain exactly two `1`s.

The closest possible pair differs in:

- one `1` moving from one position to another.
    

That changes:

```text
1 → 0
0 → 1
```

So exactly two positions differ.

Therefore:

[  
\boxed{d_{\min}=2}  
]

Hence:

```text
Detect → 1 error
Correct → 0 errors
```

The lecture confirms that the two-out-of-five code has minimum Hamming distance 2.

---

# 63. Four-Out-of-Seven Code

Similarly, consider all binary words of length 7 containing exactly four `1`s.

Number of codewords:

[  
\binom74  
]

[  
=\frac{7!}{4!3!}  
]

[  
\boxed{35}  
]

The lecture gives this result.

---

# 64. Minimum Distance of Four-Out-of-Seven

Again, two valid codewords can differ by moving one `1`:

```text
1 → 0
0 → 1
```

So the minimum distance is:

[  
\boxed{d_{\min}=2}  
]

Therefore:

```text
Detect → 1 error
Correct → 0 errors
```

The lecture confirms this.

---

# 65. Important Pattern: Constant-Weight Codes

If a code contains all binary strings of length `n` having exactly `k` ones:

[  
\boxed{\text{Number of codewords}=\binom nk}  
]

For this type of code:

[  
\boxed{d_{\min}=2}  
]

because the smallest possible change between two codewords is replacing one `1` with one `0` and another `0` with `1`.

Therefore:

[  
\boxed{\text{Detect 1 error, correct 0}}  
]

---

# 66. GATE/Numerical Pattern

Whenever you see:

> "Minimum Hamming distance of code is (d)"

immediately calculate:

[  
\boxed{\text{Detection}=d-1}  
]

and:

[  
\boxed{\text{Correction}=\left\lfloor\frac{d-1}{2}\right\rfloor}  
]

Example:

[  
d=11  
]

Then:

```text
Detection = 10
Correction = 5
```

---

# 67. Reverse Questions

GATE often asks the reverse.

### If you need to detect `s` errors:

[  
\boxed{d_{\min}=s+1}  
]

### If you need to correct `t` errors:

[  
\boxed{d_{\min}=2t+1}  
]

Example:

> Correct 8 errors.

[  
d_{\min}=2(8)+1=17  
]

---

# 68. Combined Error Detection/Correction

Suppose:

```text
Correct c errors
Detect d errors
```

Then, in the common case (d\ge c):

[  
\boxed{d_{\min}=c+d+1}  
]

Example:

```text
Correct 3
Detect 5
```

[  
d_{\min}=3+5+1  
]

[  
\boxed{9}  
]

---

# 69. Core Conceptual Chain

This entire lecture can be understood as one chain:

```text
Noise
  ↓
Bit errors
  ↓
Add redundancy
  ↓
Create codewords
  ↓
Separate valid codewords
  ↓
Hamming distance
  ↓
Minimum Hamming distance
  ↓
Error detection / correction capability
```

The more separated the valid codewords are:

[  
\boxed{\text{larger }d_{\min}}  
]

the more errors the code can handle.

---

# 70. Important Comparison

|Scheme|Main idea|(d_{\min})|Detect|Correct|
|---|---|--:|--:|--:|
|Repetition example|Repeat data|3|2|1|
|Single parity|Add 1 parity bit|2|1|0|
|General code|Depends on code|(d)|(d-1)|(\lfloor(d-1)/2\rfloor)|

---

# 71. Error Detection vs Error Correction

|Feature|Detection|Correction|
|---|---|---|
|Main question|Did error occur?|What was original codeword?|
|Requirement|Invalid received word|Unique valid codeword identification|
|Distance requirement|(s+1)|(2t+1)|
|Capability|(d_{\min}-1)|(\lfloor(d_{\min}-1)/2\rfloor)|
|Example|Parity|Hamming code|

---

# 72. Lecture's Final Results

The lecture ends with these important results:

### Result 1 — Detection

All errors of `d` or fewer bits can be detected **iff**:

[  
\boxed{d_{\min}=d+1}  
]

---

### Result 2 — Correction

All errors of `c` or fewer bits can be corrected **iff**:

[  
\boxed{d_{\min}=2c+1}  
]

---

### Result 3 — Common combined case

A code that:

- corrects `c` errors, and
    
- detects `d` errors, where (d\ge c),
    

requires:

[  
\boxed{d_{\min}=d+c+1}  
]

---

# 73. One-Page Mental Model

```text
                    ERROR CONTROL
                         │
          ┌──────────────┴──────────────┐
          │                             │
     DETECTION                     CORRECTION
          │                             │
   "Did error occur?"           "What was original?"
          │                             │
      Parity/CRC                  Hamming codes
          │
          └──────────────┐
                         ↓
                  CODEWORDS
                         ↓
                 HAMMING DISTANCE
                         ↓
              MINIMUM HAMMING DISTANCE
                         │
             ┌───────────┴───────────┐
             ↓                       ↓
        DETECTION                CORRECTION
             │                       │
       dmin - 1                  floor((dmin-1)/2)
             │                       │
   detect s → s+1          correct t → 2t+1
```

---

# 74. ⚠️ GATE Traps & Clarifications

### Trap 1 — Hamming distance ≠ number of errors

Hamming distance is the number of differing positions **between two words**.

It is not automatically the number of errors that occurred.

---

### Trap 2 — Use `dmin`, not arbitrary distance

Error capability depends on:

[  
\boxed{d_{\min}}  
]

not the maximum distance between codewords.

---

### Trap 3 — Detection and correction are different

For:

[  
d_{\min}=7  
]

```text
Detect = 6
Correct = 3
```

NOT 6 correction.

---

### Trap 4 — Parity does not locate an error

Parity says:

```text
Error exists
```

It doesn't say:

```text
Bit #4 is wrong
```

Therefore it cannot correct.

---

### Trap 5 — Even number of parity errors may escape

Single parity detects all **odd-number-of-bit errors**, but even-number errors can preserve parity.

---

### Trap 6 — Valid received word does NOT guarantee no error

This is crucial:

```text
Original valid
      ↓
   noise
      ↓
another valid codeword
```

The receiver accepts it.

Therefore:

[  
\boxed{\text{Undetectable error}}  
]

---

### Trap 7 — `dmin = 4` does not mean detect 4 guaranteed

It means:

[  
4-1=3  
]

errors are guaranteed detectable.

A 4-bit error **may** turn one valid codeword into another valid codeword.

---

### Trap 8 — "Up to" matters

"Correct up to 3 errors" means:

```text
0, 1, 2, 3
```

not exactly 3.

---

### Trap 9 — Channel assumptions matter

If the problem says:

> At most one bit can be corrupted

use that information.

Don't ignore the channel property.

---

# 75. Worked GATE-Style Examples

## Example 1

Given:

[  
d_{\min}=6  
]

How many errors can be detected?

[  
6-1=\boxed5  
]

How many can be corrected?

[  
\left\lfloor\frac{6-1}{2}\right\rfloor  
=2  
]

### Answer

[  
\boxed{\text{Detect 5, Correct 2}}  
]

---

## Example 2

A code must detect up to 8 errors.

Required:

[  
d_{\min}=8+1  
]

[  
\boxed9  
]

---

## Example 3

A code must correct up to 4 errors.

[  
d_{\min}=2(4)+1  
]

[  
\boxed9  
]

---

## Example 4

A code has:

[  
d_{\min}=11  
]

Then:

[  
\text{Detection}=10  
]

[  
\text{Correction}=5  
]

[  
\boxed{\text{Detect 10, Correct 5}}  
]

This is exactly the logic used in the lecture's MSQ.

---

# 76. Question Pattern: Find `dmin`

Given codewords:

```text
00000
00111
11001
11110
```

Method:

1. Compare every pair.
    
2. Calculate Hamming distance.
    
3. Find smallest distance.
    

That smallest value is:

[  
\boxed{d_{\min}}  
]

Then immediately derive:

```text
Detection = dmin - 1
Correction = floor((dmin - 1)/2)
```

---

# 77. Question Pattern: Number of Codewords

If all length-`n` words containing exactly `k` ones are allowed:

[  
\boxed{|C|=\binom nk}  
]

Examples:

[  
\binom52=10  
]

[  
\binom74=35  
]

These exact patterns appear in the lecture's practice questions.

---

# 78. Question Pattern: Valid/Invalid Received Word

Given a set of valid codewords:

```text
C = { ... }
```

and received word `R`:

### Step 1

Check:

[  
R\in C?  
]

If yes:

```text
R = valid codeword
```

→ Error **may** still have occurred.

If no:

```text
R = invalid codeword
```

→ Error definitely detected.

---

# 79. Question Pattern: Correct the Received Word

If correction capability is guaranteed:

1. Find valid codewords near the received word.
    
2. Calculate their Hamming distances.
    
3. Determine the nearest valid codeword.
    
4. Use channel assumptions if given.
    
5. If unique → correction possible.
    
6. If multiple equally possible candidates → cannot uniquely correct.
    

---

# 80. Final Formula Sheet

## Hamming Distance

[  
\boxed{d(x,y)=\text{number of differing bit positions}}  
]

## Minimum Hamming Distance

[  
\boxed{d_{\min}=\min_{x\ne y}d(x,y)}  
]

## Detect up to `s`

[  
\boxed{d_{\min}\ge s+1}  
]

## Maximum guaranteed detection

[  
\boxed{s=d_{\min}-1}  
]

## Correct up to `t`

[  
\boxed{d_{\min}\ge2t+1}  
]

## Maximum guaranteed correction

[  
\boxed{t=\left\lfloor\frac{d_{\min}-1}{2}\right\rfloor}  
]

## Correct `c`, detect `d`

[  
\boxed{d_{\min}\ge c+d+1}  
]

for the common case (d\ge c).

## Number of binary words with exactly `k` ones

[  
\boxed{\binom nk}  
]

## Even parity

[  
\boxed{P=D_1\oplus D_2\oplus\cdots\oplus D_k}  
]

with total number of `1`s even.

---

# Quick Revision

### Framing

- Continuous bit stream → discrete frames.
    
- Purpose: identify frame boundaries.
    

### Error Handling

- **Detection:** determine whether an error occurred.
    
- **Correction:** determine the original codeword.
    

### Block Coding

- Add redundant bits to data.
    
- `k` data bits → (2^k) possible data words.
    

### Repetition

```text
00 → 00 00 00
01 → 01 01 01
10 → 10 10 10
11 → 11 11 11
```

- (d_{\min}=3)
    
- Detect 2
    
- Correct 1
    

### Single Parity

- Even parity → total `1`s even.
    
- Odd parity → total `1`s odd.
    
- Single parity:
    
    - (d_{\min}=2)
        
    - Detect 1
        
    - Correct 0
        
    - Detects all odd-number-of-bit errors.
        
    - Even-number errors may escape.
        

### Hamming Distance

[  
d(x,y)=#\text{ differing positions}  
]

### Minimum Hamming Distance

[  
d_{\min}=\min d(x,y)  
]

### Detection

[  
\boxed{\text{Detect }d_{\min}-1}  
]

### Correction

[  
\boxed{\text{Correct }\left\lfloor\frac{d_{\min}-1}{2}\right\rfloor}  
]

### Required distance

```text
Detect s → dmin = s + 1
Correct t → dmin = 2t + 1
```

### Combined

[  
\boxed{d_{\min}=c+d+1}  
]

(common case (d\ge c))

### Biggest conceptual rule

> **Errors are detectable as long as corruption cannot take one valid codeword into another valid codeword.**

That is exactly what **minimum Hamming distance** measures.

---

# GATE Question Triggers

When you see these phrases, immediately think:

|Question wording|Trigger|
|---|---|
|"Minimum Hamming distance"|Compare every pair of valid codewords|
|"Detect up to `s` errors"|(d_{\min}=s+1)|
|"Correct up to `t` errors"|(d_{\min}=2t+1)|
|"Given (d_{\min}), maximum detectable"|(d_{\min}-1)|
|"Given (d_{\min}), maximum correctable"|(\lfloor(d_{\min}-1)/2\rfloor)|
|"Single parity"|(d_{\min}=2)|
|"Parity detects?"|All odd-number-of-bit errors|
|"Parity corrects?"|❌|
|"Valid → valid"|Undetectable|
|"Valid → invalid"|Detectable|
|"Exactly `k` ones in n-bit code"|(\binom nk)|
|"At most one bit error"|Use channel property|
|"Correct c + detect d"|(d_{\min}=c+d+1)|

**The most important thing to carry forward into the next lecture is:**  
**`valid codewords → Hamming distance → dmin → detection/correction capability`.** The lecture ends by making this exact connection.