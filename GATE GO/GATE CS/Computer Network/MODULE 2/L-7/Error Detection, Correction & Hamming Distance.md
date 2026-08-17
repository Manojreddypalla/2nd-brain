# Computer Networks — Lecture 7

## Error Detection, Correction & Hamming Distance

> **GATE Scope:** Framing → Error types → Error control → Block coding → Parity → Hamming Distance → Error detection/correction.

---

# 1. Framing

**Framing:** Dividing a continuous stream of bits into **discrete frames/chunks** so that the receiver knows where each frame starts and ends.

The fundamental problem is:

> Given a continuous bit stream, **where is the beginning and end of a frame?**

---

# 2. Error Handling

**Error handling:** Detect and/or correct errors in received frames.

During transmission:

```text
Sender → Channel + Noise → Receiver
```

The received data may differ from the transmitted data.

### Types of errors

### Single-bit error

Only **one bit** is corrupted.

```text
Sent:     00000100
Received: 00001100
              ↑
```

### Burst error

Multiple bits within a span are corrupted.

```text
Sent:     000000000000
Received: 001101000000
             ↑↑↑
```

**GATE trigger:** Don't confuse _number of corrupted bits_ with _length of a burst_. A burst can contain fewer corrupted bits than its span.

---

# 3. Error-Control Methods

Two broad approaches:

```text
Error Control
├── Error Detection
│   ├── Parity
│   ├── CRC
│   └── Checksum
│
└── Error Correction
    └── Hamming Codes
```

The lecture first develops **parity**, then **Hamming distance**, which is required before studying Hamming codes.

---

# 4. Block Coding

Idea:

> Add extra bits to the original data to create a **codeword**.

```text
Data bits + Check/Parity bits
        ↓
     Codeword
```

If there are `k` data bits and `(n-k)` check bits:

```text
Codeword length = n
Data bits       = k
Check bits      = n-k
```

The additional bits can be placed at the **start, middle, end, or other positions**, depending on the coding scheme.

For `k` data bits:

```text
Number of possible data words = 2^k
```

---

# 5. Repetition Coding — Intuition

Simplest idea:

> Send the same data multiple times.

Example:

```text
00 → 00 00 00
01 → 01 01 01
10 → 10 10 10
11 → 11 11 11
```

Receiver checks the copies.

If it receives:

```text
01 00 00
```

it knows something is wrong because the three copies don't agree.

### Problems

- Large overhead.
    
- Doesn't provide a clean way to know the correct data in every situation.
    
- Inefficient compared with proper error-control codes.
    

---

# 6. Parity Checking

The simplest error-detection scheme.

A **parity bit** is appended to the data so that the total number of `1`s satisfies a chosen condition.

## Even Parity

Total number of `1`s, **including parity bit**, must be **even**.

## Odd Parity

Total number of `1`s, **including parity bit**, must be **odd**.

---

## Even-Parity Formula

For data bits:

```text
D1, D2, ..., Dk
```

Parity bit:

```text
P = D1 ⊕ D2 ⊕ ... ⊕ Dk
```

This produces **even parity**.

Why?

XOR gives `1` when the number of `1`s in its inputs is odd.

Therefore:

- Odd number of `1`s in data → `P = 1`
    
- Even number of `1`s in data → `P = 0`
    

so the final codeword always has an even number of `1`s.

### Example

Data:

```text
1101010
```

Number of `1`s = 4 → already even.

Therefore:

```text
P = 0

Codeword = 11010100
```

---

# 7. What Can Single-Bit Parity Detect?

Suppose even parity is used.

### One bit changes

```text
Valid codeword → Invalid codeword
```

The parity changes, so the receiver detects the error.

### Two bits change

```text
Valid codeword → Valid codeword
```

The parity may become correct again.

Therefore, the receiver **cannot detect all multiple-bit errors**.

### Important GATE fact

Single parity can detect:

> **Any odd number of bit errors**

It cannot reliably detect:

> **An even number of bit errors**

So:

```text
1 error  → Detect
2 errors → May not detect
3 errors → Detect
4 errors → May not detect
...
```

The lecture's MSQ establishes that single-bit parity can detect all single-bit errors but cannot correct them.

> **GATE clarification:** The important condition is the **number of flipped bits being odd**, not whether the bit _positions_ are odd/even.

---

# 8. Minimum Hamming Distance

## Hamming Distance

The **Hamming distance** between two equal-length binary words is:

> Number of positions in which the corresponding bits differ.

Notation:

```text
d(x,y)
```

### Example

```text
000
011
```

Differences:

```text
0 ≠ 0  → same
0 ≠ 1  → different
0 ≠ 1  → different
```

Therefore:

```text
d(000,011) = 2
```

Another example:

```text
10101
11110
```

They differ in 3 positions:

```text
d = 3
```

---

# 9. Minimum Hamming Distance — d_min

For a set of valid codewords:

> `d_min` = **smallest Hamming distance between any two valid codewords.**

```text
d_min = min d(Ci, Cj)
        for all distinct valid codewords
```

Example:

```text
C = {0000, 0011, 0101, 0110, ...}
```

Calculate distances between pairs.

The **smallest** one is `d_min`.

### Key intuition

Think of every valid codeword as a point.

```text
Valid        Valid
  ●------------●
       d_min
```

Invalid received words can lie between valid codewords.

The larger the separation between valid codewords, the more errors the code can tolerate.

---

# 10. Error Detection Using d_min

Suppose:

```text
d_min = 4
```

Between two valid codewords:

```text
Valid ● -------- ● Valid
          4
```

A received word with:

```text
1 error → invalid
2 errors → invalid
3 errors → invalid
4 errors → could become another valid codeword
```

Therefore:

```text
Maximum guaranteed detectable errors
= d_min - 1
```

### General Formula

To guarantee detection of up to `s` errors:

```text
d_min ≥ s + 1
```

### Example

If:

```text
d_min = 5
```

then:

```text
Detect up to 4 errors.
```

If the question asks:

> "To guarantee detection of up to 5 errors?"

Answer:

```text
d_min = 6
```

---

# 11. Why d_min − 1?

Suppose:

```text
d_min = 4
```

Two valid codewords are exactly 4 bits apart.

A received word that is only:

```text
1, 2, or 3
```

bits away from one valid codeword cannot yet become another valid codeword.

But after **4 flips**, it can reach another valid codeword.

Hence:

```text
Detectable errors = d_min - 1
```

---

# 12. Error Correction

Detection only tells us:

> "Something is wrong."

Correction requires:

> "Which valid codeword was originally sent?"

Therefore the receiver must be able to identify the original codeword uniquely.

Think geometrically:

```text
        invalid
          ○
          |
          | 1
          |
Valid ●---+
```

If every possible received word is closer to exactly one valid codeword, correction is possible.

---

# 13. Minimum Distance for Error Correction

To guarantee correction of up to `t` errors:

```text
d_min ≥ 2t + 1
```

Therefore:

```text
Maximum guaranteed correctable errors
= floor((d_min - 1) / 2)
```

### Examples

#### d_min = 3

```text
Correct = floor((3-1)/2)
        = 1 bit
```

#### d_min = 5

```text
Correct = floor((5-1)/2)
        = 2 bits
```

#### d_min = 7

```text
Correct = 3 bits
```

#### d_min = 15

```text
Correct = 7 bits
Detect  = 14 bits
```

The lecture explicitly demonstrates `d_min = 15 → correct 7, detect 14`.

---

# 14. The Most Important GATE Formula

Memorize this **after understanding it**:

```text
Detection:
d_min ≥ s + 1

Correction:
d_min ≥ 2t + 1
```

where:

```text
s = maximum errors to detect
t = maximum errors to correct
```

Therefore:

```text
Detect up to s:
d_min = s + 1

Correct up to t:
d_min = 2t + 1
```

---

# 15. Detection vs Correction — Don't Mix Them

|Requirement|Minimum d_min|
|---|--:|
|Detect 1 error|2|
|Detect 2 errors|3|
|Detect 3 errors|4|
|Correct 1 error|3|
|Correct 2 errors|5|
|Correct 3 errors|7|
|Correct 5 errors|11|

### Shortcut

```text
Detect t → t + 1

Correct t → 2t + 1
```

---

# 16. Single-Parity Code and d_min

Single parity code has:

```text
d_min = 2
```

Why?

Any two valid codewords must have the same parity. The smallest possible change that preserves parity requires flipping **2 bits**.

Therefore:

```text
d_min = 2
```

Hence:

```text
Detect = d_min - 1 = 1 error
Correct = floor((2-1)/2) = 0 errors
```

So single parity:

```text
✓ Detects 1-bit error
✗ Corrects 1-bit error
```

The lecture explicitly uses `d_min = 2` to establish this result.

---

# 17. Important Geometric Intuition

Suppose:

```text
d_min = 5
```

Two valid codewords:

```text
A ●----------------● B
          5
```

A received word can be at most 2 bits away from the original:

```text
A ●--1--○--1--○
```

At distance 2, it is still closer to A.

But after 3 errors:

```text
A ●---○---○---X
```

the received word can become ambiguous.

Therefore:

```text
Correctable = floor((5-1)/2) = 2
```

This is why the factor **2** appears in:

```text
2t + 1
```

---

# 18. Combined Detection + Correction

Suppose a code must:

- correct `c` errors
    
- detect `d` errors
    
- with `d ≥ c`
    

Then:

```text
d_min ≥ d + c + 1
```

The lecture summary gives this as the common combined case.

### Special cases

Only detection:

```text
c = 0

d_min = d + 1
```

Only correction:

```text
d = c

d_min = 2c + 1
```

Therefore:

```text
Detection of d + Correction of c:
d_min = d + c + 1
```

**GATE trigger:** If the question says **"correct c errors AND detect d errors"**, don't independently apply `2c+1`. Use:

```text
d_min = d + c + 1
```

---

# 19. Valid vs Invalid Codewords

A code defines a set of **valid codewords**.

Example:

```text
Valid:
00000
10101
11100
...
```

A received sequence can be:

```text
Valid → Valid
Valid → Invalid
```

### Valid → Invalid

Error can be detected.

### Valid → Valid

The receiver may **not know that an error occurred**.

This is the fundamental reason `d_min` matters.

---

# 20. Error-Correction Decision Pattern

For a received word `R`:

1. Compare `R` with valid codewords.
    
2. Calculate Hamming distance.
    
3. Find the closest valid codeword.
    
4. If the closest codeword is **uniquely identifiable**, correction is possible.
    

Example from the lecture:

```text
Received = 0101000
```

If the possible invalid words around a valid codeword do not overlap with another valid codeword's correction region, the receiver can determine the original codeword.

---

# 21. "Exactly t Errors" vs "Up to t Errors"

This is a common GATE trap.

### Up to t errors

Means:

```text
0, 1, 2, ..., t
```

errors.

To guarantee correction:

```text
d_min ≥ 2t + 1
```

### Exactly t errors

The requirement may be different because fewer errors are not necessarily being considered.

Read the wording carefully.

**GATE trigger:** Pay attention to:

- **up to**
    
- **at most**
    
- **exactly**
    
- **guarantee**
    
- **in all cases**
    

---

# 22. Important Codeword-Set Questions

If a question gives a set of codewords:

```text
C = {C1, C2, C3, ...}
```

and asks `d_min`:

### Method

Compute Hamming distance between codeword pairs:

```text
d(C1,C2)
d(C1,C3)
d(C2,C3)
...
```

Then:

```text
d_min = minimum
```

For large structured sets, look for patterns instead of comparing every pair.

The lecture includes examples such as "two-out-of-five" and "four-out-of-seven" codes, where the minimum distance is obtained by examining overlap between codewords.

---

# 23. Two-out-of-Five Code

Definition:

> All binary words of length 5 containing **exactly two 1s**.

Number of codewords:

```text
C(5,2) = 10
```

The lecture derives:

```text
d_min = 2
```

Therefore:

```text
Detect = 1 error
Correct = 0 errors
```

### General counting pattern

If a code consists of all length-`n` binary strings having exactly `k` ones:

```text
Number of codewords = C(n,k)
```

---

# 24. Four-out-of-Seven Code

Definition:

> All binary words of length 7 containing exactly four `1`s.

Number of codewords:

```text
C(7,4) = 35
```

The lecture finds:

```text
d_min = 2
```

Therefore:

```text
Detect = 1 error
Correct = 0 errors
```

---

# 25. Fast Pattern for Constant-Weight Codes

For a code where every codeword has exactly `k` ones:

Two different codewords cannot differ in only one position.

Why?

Changing one bit would change the number of `1`s.

Therefore the smallest possible difference is usually **2**.

So for:

```text
"exactly k ones"
```

expect:

```text
d_min = 2
```

when at least two distinct codewords exist.

---

# 26. GATE Problem-Solving Table

|Given|Find|
|---|---|
|`d_min`|Detect = `d_min - 1`|
|`d_min`|Correct = `floor((d_min-1)/2)`|
|Detect `s`|`d_min ≥ s+1`|
|Correct `t`|`d_min ≥ 2t+1`|
|Correct `c`, detect `d`|`d_min ≥ c+d+1`|
|Single parity|`d_min=2`|
|Exactly `k` ones in `n` bits|Number of words = `C(n,k)`|

---

# 27. GATE Traps ⚠️

### Trap 1 — Detection ≠ Correction

```text
d_min = 3
```

means:

```text
Detect up to 2 errors
Correct up to 1 error
```

NOT 2 errors corrected.

---

### Trap 2 — `d_min` itself is NOT the number of detectable errors

Wrong:

```text
d_min = 5 → detect 5
```

Correct:

```text
detect = 5 - 1 = 4
```

---

### Trap 3 — Correction needs twice the distance

```text
Correct t → d_min = 2t+1
```

not:

```text
t+1
```

---

### Trap 4 — Single parity does not correct

```text
d_min = 2

Detect = 1
Correct = 0
```

---

### Trap 5 — "Valid received codeword" does NOT mean no error

An error can transform:

```text
Valid → another Valid
```

and become **undetectable**.

---

### Trap 6 — "Odd positions" vs "odd number of errors"

For parity:

```text
Odd number of flipped bits → detected
Even number of flipped bits → may escape detection
```

The relevant property is the **count of flipped bits**.

---

# 28. GATE Question Triggers

When you see:

### "Maximum errors that can be detected?"

Immediately think:

```text
d_min - 1
```

### "Maximum errors that can be corrected?"

Immediately think:

```text
floor((d_min - 1)/2)
```

### "Minimum distance required to detect s errors?"

```text
s + 1
```

### "Minimum distance required to correct t errors?"

```text
2t + 1
```

### "Correct c AND detect d"

```text
c + d + 1
```

### "Minimum Hamming distance of a code"

Compare valid codewords and find the **smallest pairwise distance**.

---

# 29. GATE Examples

### Example 1

`d_min = 4`

```text
Detect = 4-1 = 3
Correct = floor(3/2) = 1
```

**Answer: detect 3, correct 1.**

---

### Example 2

Guarantee detection of 7 errors:

```text
d_min = 7+1
      = 8
```

---

### Example 3

Guarantee correction of 5 errors:

```text
d_min = 2(5)+1
      = 11
```

The lecture includes this exact type of NIELIT question.

---

### Example 4

`d_min = 11`

```text
Detect = 10
Correct = 5
```

So:

```text
✓ Detect 5
✓ Correct 5
✗ Detect 11
```

The lecture's MSQ confirms the valid choices are detecting 5 and correcting 5.

---

# 30. Final Mental Model

Don't memorize four unrelated formulas.

Understand this picture:

```text
             Valid codeword
                   ●
                 /   \
              t errors
               /       \
              ○         ○
               \       /
                \     /
                 2t+1
                    \
                   ●
             Another valid
```

If valid codewords are separated by `d_min`:

```text
Before reaching another valid codeword:
    guaranteed detection → d_min - 1

To uniquely identify the original codeword:
    guaranteed correction → floor((d_min-1)/2)
```

Therefore:

```text
                 d_min
                   │
        ┌──────────┴──────────┐
        ↓                     ↓
   Detection              Correction
        ↓                     ↓
   d_min - 1        floor((d_min-1)/2)
```

---

# Quick Revision ⚡

```text
Framing
→ Divide bit stream into frames.

Error types
→ Single-bit, Burst.

Error control
→ Detection + Correction.

Detection
→ Parity, CRC, Checksum.

Correction
→ Hamming codes.

Block coding
→ Add redundant/check bits.

Parity
→ Even parity: total 1s even.
→ Odd parity: total 1s odd.
→ Single parity: d_min = 2.
→ Detects 1-bit error.
→ Cannot correct 1-bit error.

Hamming distance
→ Number of differing corresponding bits.

d_min
→ Minimum Hamming distance between any two valid codewords.

Detection
→ Detect up to s errors:
   d_min ≥ s+1

Correction
→ Correct up to t errors:
   d_min ≥ 2t+1

Combined
→ Correct c AND detect d:
   d_min ≥ c+d+1

Maximum detection
→ d_min - 1

Maximum correction
→ floor((d_min-1)/2)

Constant-weight code
→ Exactly k ones in n bits:
   Number of codewords = C(n,k)
```

## One-line GATE memory hook

> **Distance gives a safety gap: `d_min−1` errors can be detected, but only about half that many can be corrected.**

The lecture concludes with exactly these detection/correction relationships and the combined `d+c+1` result.