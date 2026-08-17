Yep. I went through the **full L-7(4).pdf** and condensed it into **GATE-oriented, notebook-ready notes**. I’m keeping the lecture's scope and examples, while removing repetition and slides that don't need to be copied.

# Computer Networks — L-7(4): Error Detection, Correction & Hamming Distance

---

# 1. Framing

### Framing

**Framing = breaking a continuous stream of bits into discrete chunks called frames.**

Why?

The receiver needs to know:

- where a frame starts
    
- where a frame ends
    

Otherwise, a continuous bit stream has no obvious boundaries.

---

# 2. Error Handling

During transmission, the channel may introduce **noise**, causing bits to change.

Error handling has two goals:

- **Error Detection** → determine whether an error occurred.
    
- **Error Correction** → determine/correct the original transmitted data.
    

### Types of errors

1. **Single-bit error**
    
    - Only one bit is corrupted.
        
2. **Burst error**
    
    - Multiple bits in a sequence are affected.
        

---

# 3. Coding the Data

To handle errors, extra/redundant bits are added to the original data.

### Block Coding

For:

- (k) = data bits
    
- (n-k) = additional/redundant bits
    

```text
|--------- n bits ---------|
| k data bits | n-k check bits |
```

Therefore:

[  
\boxed{\text{Codeword length}=n}  
]

Number of possible data words:

[  
\boxed{2^k}  
]

> **GATE:** You are usually given/asked about valid codewords and their Hamming distances. You do not need to derive an arbitrary set of codewords unless the question specifically asks for encoding.

---

# 4. Error-Control Methods

The lecture divides error control into:

### Error Detection

- Parity
    
- CRC
    
- Checksum
    

### Error Correction

- Hamming codes
    

This lecture mainly develops:

[  
\boxed{\text{Parity} \rightarrow \text{Hamming Distance} \rightarrow \text{Error Correction}}  
]

---

# 5. Repetition Code

### Idea

Instead of sending a bit once, send it multiple times.

Example:

```text
Data    Codeword

00   →  00 00 00
01   →  01 01 01
10   →  10 10 10
11   →  11 11 11
```

The receiver already knows the set of **valid codewords**.

For example:

```text
000000
010101
101010
111111
```

If the receiver receives something like:

```text
01 00 00
```

it knows this is **not a valid codeword**, so an error occurred.

### Problem with repetition

If the received pattern happens to become **another valid codeword**, the receiver cannot detect the error.

Example:

```text
Sent:     00 00 00
Received: 01 01 01
```

The receiver sees a valid codeword and cannot know that an error occurred.

Therefore repetition has overhead and is not an ideal general solution.

---

# 6. Valid vs Invalid Codeword

This concept is **very important**.

The receiver knows the complete list of valid codewords.

### Valid

```text
Received → one of the valid codewords
```

Receiver says:

> No error detected.

### Invalid

```text
Received → not present in valid-codeword set
```

Receiver says:

> Error detected.

But:

[  
\boxed{\text{Valid received codeword does NOT guarantee no error}}  
]

An error may have transformed one valid codeword into another valid codeword.

---

# 7. Parity Checking

Parity adds **one extra bit** to the data.

The parity bit makes the total number of 1s either:

- **Even parity** → total number of 1s is even.
    
- **Odd parity** → total number of 1s is odd.
    

---

## Even Parity

Choose (P) such that:

[  
\boxed{\text{Total number of 1s including }P\text{ is even}}  
]

For data:

```text
1101010
```

Number of 1s = 4.

Already even:

```text
P = 0
```

So:

```text
11010100
```

is the codeword.

---

## Odd Parity

Choose (P) such that:

[  
\boxed{\text{Total number of 1s including }P\text{ is odd}}  
]

---

## XOR formula

For **even parity**:

[  
\boxed{P=D_1\oplus D_2\oplus\cdots\oplus D_k}  
]

Because XOR gives:

- 1 → odd number of 1s
    
- 0 → even number of 1s
    

The parity bit is chosen so that the **total** number of 1s becomes even.

---

# 8. What Can Single Parity Detect?

For a single parity code:

[  
\boxed{d_{\min}=2}  
]

Therefore:

[  
d_{\min}-1=1  
]

So it can **guarantee detection of 1-bit errors**.

More generally:

### Even parity

If an **odd number of bits** are corrupted:

```text
1 error
3 errors
5 errors
7 errors
...
```

the parity changes, so the receiver detects the error.

If an **even number of bits** are corrupted:

```text
2 errors
4 errors
6 errors
...
```

parity may remain correct, so the error can go undetected.

Therefore:

[  
\boxed{\text{Single parity detects all odd-number-of-bit errors}}  
]

but

[  
\boxed{\text{cannot guarantee detection of even-number-of-bit errors}}  
]

---

## Single parity cannot correct the error

Parity tells us:

> "Something is wrong."

It does **not tell us which bit is wrong**.

Therefore:

[  
\boxed{\text{Single parity detects, but does not correct}}  
]

The lecture's MSQ specifically marks that all single-bit errors are detectable, but correction is not possible because the corrupted bit's position is unknown.

---

# 9. Hamming Distance

Now the main topic.

### Definition

The **Hamming distance** between two equal-length bit strings is:

[  
\boxed{\text{Number of corresponding positions in which they differ}}  
]

Example:

```text
000011
010011
 ↑
```

Only one position differs:

[  
\boxed{d=1}  
]

Another example:

```text
01101010
11011011
```

They differ at 4 positions:

[  
\boxed{d=4}  
]

---

# 10. Minimum Hamming Distance

For a set of valid codewords:

[  
\boxed{d_{\min}=\text{minimum Hamming distance between any two valid codewords}}  
]

In other words:

> Compare every pair of valid codewords and take the smallest distance.

### Example

```text
C = {0000, 0011, 0101, 0110, 1001, 1010, 1100, 1111}
```

The minimum distance between any two valid codewords is:

[  
\boxed{d_{\min}=2}  
]

---

# 11. How Error Detection Actually Works

This connects directly to what we were discussing.

Suppose valid codewords are:

```text
A
B
C
D
```

The receiver receives:

```text
R
```

It compares (R) against the set of valid codewords.

### If R is invalid

```text
R ∉ {A,B,C,D}
```

→ error is detected.

### If R is another valid codeword

```text
R ∈ {A,B,C,D}
```

→ receiver may not detect the error.

This is why **distance between valid codewords matters**.

---

# 12. Detection Capability

If:

[  
\boxed{d_{\min}=d}  
]

then the code can **guarantee detection of up to**:

[  
\boxed{d-1\text{ errors}}  
]

### Why?

Two valid codewords are at least (d) bits apart.

Therefore, changing fewer than (d) bits cannot transform one valid codeword into another valid codeword.

So:

[  
\boxed{\text{Maximum guaranteed detectable errors}=d_{\min}-1}  
]

### Example

[  
d_{\min}=4  
]

Then:

[  
\boxed{3\text{ errors can be detected}}  
]

But 4 errors **may** transform one valid codeword into another valid codeword, so detection is no longer guaranteed.

---

# 13. Correction Capability

Now we want more than detecting the error.

We want to determine:

> **Which valid codeword was originally transmitted?**

If:

[  
d_{\min}=d  
]

then maximum guaranteed correctable errors:

[  
\boxed{  
t=\left\lfloor\frac{d-1}{2}\right\rfloor  
}  
]

Equivalent condition:

[  
\boxed{d_{\min}\ge 2t+1}  
]

where (t) = number of errors to correct.

---

# 14. Why (2t+1)?

Suppose:

[  
d_{\min}=5  
]

and the channel introduces at most 2 errors.

Imagine:

```text
Valid A ●──────● Valid B
       distance = 5
```

A received word can be 2 bits away from A:

```text
A ●──○──○ R
```

It cannot simultaneously be 2 bits away from B.

Because:

[  
2+2=4<5  
]

Therefore the received word has a **unique nearest valid codeword**.

So we can correct it.

Hence:

[  
\boxed{d_{\min}\ge2t+1}  
]

---

# 15. Detection vs Correction — VERY IMPORTANT

|(d_{\min})|Capability|
|---|---|
|(d_{\min}-1)|Maximum guaranteed detectable errors|
|(\lfloor(d_{\min}-1)/2\rfloor)|Maximum guaranteed correctable errors|

### Example

If:

[  
d_{\min}=5  
]

Detection:

[  
5-1=\boxed{4}  
]

Correction:

[  
\left\lfloor\frac{5-1}{2}\right\rfloor  
=\boxed{2}  
]

---

# 16. How Correction Is Actually Done

Suppose valid codewords are:

```text
A = 000000
B = 001111
C = 010011
D = 011100
```

Received:

```text
R = 000011
```

Compare R with **all valid codewords**:

```text
d(R,A) = 2
d(R,B) = 2
d(R,C) = 1  ← minimum
d(R,D) = 4
```

Therefore:

[  
\boxed{R\rightarrow C}  
]

So:

```text
Received:
000011

Corrected:
010011
```

This is the **nearest-valid-codeword idea**.

The lecture uses exactly this type of decoding question later.

---

# 17. Important: Detection vs Correction

Suppose:

[  
d_{\min}=3  
]

Then:

### Detection

Can detect:

[  
3-1=\boxed{2}  
]

errors.

### Correction

Can correct:

[  
\left\lfloor\frac{3-1}{2}\right\rfloor  
=\boxed{1}  
]

error.

So:

```text
dmin = 3

Detect → 2 errors
Correct → 1 error
```

---

# 18. Why a Valid Received Codeword Can Be an Error

This is one of the biggest GATE traps.

Suppose:

```text
Valid A ●────────● Valid B
             3
```

If enough bits change to move A → B, the receiver receives another **valid codeword**.

Therefore:

```text
Valid → Valid
```

can mean:

[  
\boxed{\text{Undetectable error}}  
]

The receiver cannot know that an error occurred.

The lecture explicitly demonstrates this distinction between:

```text
valid → invalid   → detectable
valid → valid     → not detectable
```

---

# 19. Visual Mental Model

Think of valid codewords as **houses**:

```text
Valid A ●────────────● Valid B
```

The invalid bit patterns are the spaces between them.

### Detection

If noise moves you from:

```text
● valid
 ↓
○ invalid
```

→ **error detected**

### Correction

If you land near one particular valid codeword:

```text
● A
 \
  ○ received
```

→ choose A.

### Problem

If noise takes you directly to:

```text
● A → ● B
```

→ receiver cannot know that an error happened.

That's why we need large separation between valid codewords.

---

# 20. Single Parity and Hamming Distance Connection

The lecture shows:

[  
\boxed{d_{\min}=2}  
]

for single-bit parity codes.

Therefore:

Detection:

[  
2-1=\boxed{1}  
]

Correction:

[  
\left\lfloor\frac{2-1}{2}\right\rfloor  
=0  
]

So:

[  
\boxed{\text{Single parity detects 1-bit errors but corrects 0 bits}}  
]

This unifies the parity topic with Hamming distance.

---

# 21. General Formula — GATE MUST KNOW

### To detect (s) errors:

[  
\boxed{d_{\min}\ge s+1}  
]

because:

[  
\boxed{s=d_{\min}-1}  
]

### To correct (c) errors:

[  
\boxed{d_{\min}\ge2c+1}  
]

because:

[  
\boxed{c=\left\lfloor\frac{d_{\min}-1}{2}\right\rfloor}  
]

The lecture derives these conditions explicitly.

---

# 22. If Both Detection and Correction Are Asked

For the lecture's **common case** where a code must:

- correct (c) errors
    
- detect (d) errors
    
- with (d\ge2c)
    

the required minimum distance is:

[  
\boxed{d_{\min}=d+c+1}  
]

The lecture summarizes this relationship on the final slides.

### Special cases

Only detection:

[  
\boxed{d_{\min}=d+1}  
]

Only correction:

[  
\boxed{d_{\min}=2c+1}  
]

---

# 23. GATE Question Pattern: Given (d_{\min})

### Example

[  
d_{\min}=11  
]

Detection:

[  
11-1=\boxed{10}  
]

Correction:

[  
\left\lfloor\frac{11-1}{2}\right\rfloor  
=\boxed{5}  
]

Therefore:

```text
Detect → 10 errors
Correct → 5 errors
```

The lecture's MSQ asks exactly this and gives **detecting 5** and **correcting 5** as the valid statements.

---

# 24. GATE Question Pattern: Given Required Correction

If the question says:

> Correct up to 5 errors.

Then:

[  
d_{\min}\ge2(5)+1  
]

[  
\boxed{d_{\min}\ge11}  
]

Example from the lecture:

> Guarantee correction of up to 5 errors → minimum Hamming distance = 11.

---

# 25. GATE Question Pattern: Given Required Detection

If the question says:

> Detect up to 5 errors.

Then:

[  
d_{\min}\ge5+1  
]

[  
\boxed{d_{\min}\ge6}  
]

---

# 26. Codeword Questions — What To Do

If the question gives:

```text
A = 000000
B = 001111
C = 010011
D = 011100
```

and gives a received word:

```text
R = 000011
```

Do:

### Step 1

Compare R with every valid codeword.

### Step 2

Calculate Hamming distance.

### Step 3

Find minimum distance.

### Step 4

If there is a **unique valid codeword within the correction capability**, correct to it.

That's exactly how the lecture's UGC NET question is solved.

---

# 27. Important GATE Trap: "Closest" Doesn't Always Mean Correct

Don't blindly say:

> "Choose the closest codeword."

You need the **channel/error assumption**.

Example:

If:

[  
d_{\min}=5  
]

then correction is guaranteed only up to:

[  
2\text{ errors}  
]

If the received word is 3 bits away from some valid codeword, you **cannot guarantee** that this was the transmitted codeword.

So:

[  
\boxed{\text{Correction requires a guaranteed error bound}}  
]

The lecture explicitly discusses this using the assumption that the channel produces at most one bit error before correction.

---

# 28. "Invalid Codewords" and Error Spheres

For a valid codeword, all patterns at distance less than the correction radius are considered possible erroneous versions of it.

For (t)-error correction:

[  
\boxed{\text{radius}=t}  
]

Example:

[  
d_{\min}=5  
]

[  
t=2  
]

So:

```text
             distance 2
                ↓
        ○ ○ ○ ○ ○
       ○    A    ○
        ○ ○ ○ ○ ○
```

The radius-2 regions around valid codewords **must not overlap**.

If they overlap, the receiver could not uniquely determine which valid codeword was sent.

This is the geometric intuition behind:

[  
d_{\min}\ge2t+1  
]

---

# 29. Useful Codeword Construction Question

The lecture also gives questions such as:

> "Two-out-of-five code"

Meaning:

> All binary words of length 5 containing exactly two 1s.

Number of codewords:

[  
\boxed{\binom52=10}  
]

The lecture finds:

[  
d_{\min}=2  
]

Therefore:

[  
\boxed{\text{Detect 1 error}}  
]

[  
\boxed{\text{Correct 0 errors}}  
]

### General pattern

If a code consists of all (n)-bit words containing exactly (r) ones:

[  
\boxed{\text{Number of codewords}=\binom nr}  
]

This can appear in GATE-style questions.

---

# 30. One-Bit Codes / Parity Connection

For single parity codes:

[  
\boxed{d_{\min}=2}  
]

Therefore:

[  
\boxed{\text{detect 1}}  
]

[  
\boxed{\text{correct 0}}  
]

The lecture explicitly uses this to connect the previously studied parity code to Hamming distance.

---

# 31. Quick Formula Sheet

Write this box in your notes:

```text
Hamming Distance:
d(x,y) = number of differing bit positions

Minimum Hamming Distance:
dmin = minimum distance between any two VALID codewords

Detection:
Maximum guaranteed detectable errors = dmin - 1

Correction:
Maximum guaranteed correctable errors
= floor((dmin - 1)/2)

To detect s errors:
dmin ≥ s + 1

To correct c errors:
dmin ≥ 2c + 1

Single parity:
dmin = 2
→ detect 1-bit errors
→ correct 0 bits
```

These are the central results of the lecture.

---

# 32. GATE Traps ⚠️

### Trap 1

**Detection ≠ Correction**

[  
d_{\min}-1  
\neq  
\left\lfloor\frac{d_{\min}-1}{2}\right\rfloor  
]

---

### Trap 2

A received word being valid does **not** prove that no error occurred.

```text
Valid → Valid
```

can be an undetectable error.

---

### Trap 3

Single parity does **not** locate the erroneous bit.

Therefore it cannot correct it.

---

### Trap 4

Single parity detects **all odd-number-of-bit errors**, not all errors.

Even number of errors can escape detection.

---

### Trap 5

"Nearest valid codeword" gives guaranteed correction only within the correction capability.

---

### Trap 6

For:

[  
d_{\min}=d  
]

don't confuse:

```text
Detection = d - 1
Correction = floor((d-1)/2)
```

---

# 33. What You Actually Need to Memorize for GATE

If you're writing this in your notebook, **this is the core**:

```text
ERROR CONTROL
│
├── Detection
│   ├── Parity
│   ├── CRC
│   └── Checksum
│
└── Correction
    └── Hamming codes
```

### Hamming Distance

[  
\boxed{d(x,y)=#\text{different bit positions}}  
]

### Minimum Distance

[  
\boxed{d_{\min}=\min{d(C_i,C_j)}}  
]

### Detection

[  
\boxed{d_{\min}-1}  
]

### Correction

[  
\boxed{\left\lfloor\frac{d_{\min}-1}{2}\right\rfloor}  
]

### Required distance

[  
\boxed{\text{detect }s\Rightarrow d_{\min}\ge s+1}  
]

[  
\boxed{\text{correct }c\Rightarrow d_{\min}\ge2c+1}  
]

### Received word

```text
Received
   ↓
Compare with VALID codewords
   ↓
Hamming distances
   ↓
Nearest valid codeword
   ↓
Correct — IF within guaranteed correction capability
```

### Parity

[  
\boxed{d_{\min}=2}  
]

[  
\boxed{\text{detect 1, correct 0}}  
]

---

## 🧠 The one mental model to remember

**Valid codewords are the "legal states."**

The channel may move the transmitted codeword to some other bit pattern.

```text
VALID ● ─── ○ ─── ○ ─── ● VALID
       \____ invalid ____/
```

- Land on an **invalid** pattern → **error detected**.
    
- Land close enough to one valid codeword → **correct to it**.
    
- Land on **another valid codeword** → error can be **undetectable**.
    

And **(d_{\min})** tells us how far apart those legal states are. That's the entire reason Hamming distance matters.