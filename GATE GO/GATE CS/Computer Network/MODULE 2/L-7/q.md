Absolutely. This time I'll give you **the questions from L-7(4).pdf + their answers**, not notes/theory.

I went through the lecture and extracted the **actual question/problem slides**, including T/F, MSQ, numerical/conceptual questions, and the worked questions at the end. I’ll keep repeated slides only once where the same question is continued across multiple pages.

# L-7(4) — All Questions & Answers

## 1. Repetition Code

### Q1. Can the receiver detect an error?

Given valid codewords:

```text
00 → 00 00 00
01 → 01 01 01
10 → 10 10 10
11 → 11 11 11
```

If sender sends:

```text
00 00 00
```

but receiver gets:

```text
01 00 00
```

**Answer: YES.**

Reason: `01 00 00` is not a valid codeword.

---

### Q2. If sender sends `00 00 00` and receiver gets `01 01 01`, can the receiver detect the error?

**Answer: NO.**

Reason:

```text
Sent      = 00 00 00
Received  = 01 01 01
```

`01 01 01` is itself a valid codeword.

Therefore:

[  
\boxed{\text{Valid} \rightarrow \text{Valid = error may be undetectable}}  
]

---

### Q3. What does the receiver know?

The receiver always knows the **list of all valid codewords**.

If received word is not in that list → error detected.

If received word is in that list → receiver assumes it is valid and cannot know whether an error occurred.

---

### Q4. Can repetition code detect all errors?

**Answer: NO.**

For the 3-copy repetition code shown in the lecture:

- all 1-bit errors → detectable
    
- all 2-bit errors → detectable
    
- some 3-bit errors → detectable
    
- but not all 3-bit errors
    

because an error can transform one valid codeword into another valid codeword.

---

# 2. Parity Questions

## Q5. For data `11010100`, what parity bit should be added for even parity?

Number of 1s:

```text
11010100
```

There are **4 ones**.

4 is already even.

Therefore:

[  
\boxed{P=0}  
]

Codeword:

```text
11010100 0
```

---

## Q6. What is the formula for even parity?

For data bits:

[  
D_1,D_2,\ldots,D_k  
]

[  
\boxed{P=D_1\oplus D_2\oplus\cdots\oplus D_k}  
]

The parity bit is selected so that total number of 1s becomes **even**.

---

## Q7. For data `1101010`, find the even parity bit.

Number of 1s = 4.

Already even.

Therefore:

[  
\boxed{P=0}  
]

Codeword:

```text
0 1101010
```

---

## Q8. Can the receiver detect this error?

Sent:

```text
1001001 1
```

Received:

```text
1011001 1
```

**Answer: YES.**

The parity condition changes because an odd number of bits were corrupted.

---

## Q9. Can even parity detect an odd number of corrupted bits?

**Answer: YES.**

If the number of corrupted bits is:

```text
1, 3, 5, 7, ...
```

then parity changes.

[  
\boxed{\text{Odd number of errors → detected}}  
]

---

## Q10. True/False

> For even parity, receiver can detect only if the number of corrupted bits is odd.

**Answer: TRUE.**

---

# 3. MSQ — Single-Bit Even Parity

### Q11. Which of the following are true about single-bit even parity?

**A.** Receiver can detect all single-bit errors.

**B.** Receiver can detect all burst errors.

**C.** Receiver can detect burst errors if corrupted bits are at odd positions.

**D.** Receiver can CORRECT all single-bit errors because receiver knows position of corrupted bit.

### Answer:

[  
\boxed{\text{A only}}  
]

Why?

- **A → TRUE**
    
- **B → FALSE**
    
- **C → FALSE**
    
- **D → FALSE**
    

Parity detects an odd number of errors, but does **not identify the corrupted bit's position**, so it cannot correct the error.

---

# 4. Hamming Distance

## Q12. Find Hamming distance:

[  
d(000,011)  
]

Compare:

```text
000
011
```

Two positions differ.

[  
\boxed{d=2}  
]

---

## Q13. Find:

[  
d(10101,11110)  
]

Number of different positions:

[  
\boxed{3}  
]

The lecture gives this as an example of Hamming distance.

---

## Q14. Find the Hamming distance between:

```text
01101010
11011011
```

They differ in 4 positions.

[  
\boxed{d=4}  
]

---

# 5. Minimum Hamming Distance

## Q15. Find minimum Hamming distance:

[  
C={0000,0011,0101,0110,1001,1010,1100,1111}  
]

Compare pairs and find the smallest distance.

[  
\boxed{d_{\min}=2}  
]

---

## Q16. What is the minimum Hamming distance of single parity codes?

**Answer:**

[  
\boxed{d_{\min}=2}  
]

---

## Q17. Why is (d_{\min}=2) for single parity?

If one bit is changed in a valid parity codeword, it becomes invalid.

Therefore two valid codewords cannot differ by only one bit.

Hence:

[  
\boxed{d_{\min}=2}  
]

---

# 6. Diagram-Based Hamming Questions

## Q18. Calculate minimum Hamming distance if each adjacent codeword is separated by distance 1.

The diagram places valid codewords in positions where moving through the diagram requires:

```text
A → intermediate → intermediate → B
```

So:

[  
\boxed{d_{\min}=3}  
]

---

## Q19. If (A=101100), what are the possible codewords at distance 1?

Starting with:

```text
101100
```

Flip exactly one bit.

Possible codewords are:

```text
001100
111100
100100
101000
101110
101101
```

There are:

[  
\binom61=\boxed{6}  
]

possible words.

This follows directly from the lecture's adjacent-codeword question.

---

## Q20. If (A=101100), what are the possible codewords at distance 2?

Flip exactly two bits.

Number of possibilities:

[  
\binom62=15  
]

Therefore:

[  
\boxed{15\text{ possible codewords}}  
]

The lecture illustrates examples such as `100000` and `011100`.

---

# 7. Valid-Codeword Questions

## Q21. Sender sends `0101001`, receiver gets `0101000`.

Will the receiver detect the error?

The received codeword is **invalid**.

Therefore:

[  
\boxed{\text{YES}}  
]

---

## Q22. Sender sends `0101001`, receiver gets `1101000`.

Will receiver detect the error?

Three bits have changed.

The received word is **invalid**.

Therefore:

[  
\boxed{\text{YES}}  
]

---

## Q23. Sender sends `0101001`, receiver gets `1001100`.

Four bits changed.

But the received word is a **valid codeword**.

Therefore:

[  
\boxed{\text{NO}}  
]

The receiver cannot detect the error.

This is the important:

[  
\boxed{\text{valid}\rightarrow\text{valid}}  
]

case.

---

# 8. Detection Using (d_{\min})

## Q24. If minimum Hamming distance is 4, how many errors can receiver detect?

Formula:

[  
\boxed{\text{detect}=d_{\min}-1}  
]

Therefore:

[  
4-1=\boxed{3}  
]

### Answer:

[  
\boxed{3\text{ bits of error}}  
]

---

## Q25. If (d_{\min}=d), how many errors can receiver detect?

[  
\boxed{d-1}  
]

---

## Q26. To guarantee detection of up to (s) errors, what should (d_{\min}) be?

Options:

- (s)
    
- (s-1)
    
- (s+1)
    
- None
    

### Answer:

[  
\boxed{s+1}  
]

because:

[  
\boxed{d_{\min}\ge s+1}  
]

---

# 9. Error Correction

## Q27. Can we correct errors?

**Answer: YES, but only when we can uniquely determine the original codeword.**

The receiver must be sufficiently sure about which valid codeword was originally transmitted.

---

## Q28. Noisy channel: at most one bit can be wrong.

Sender:

```text
0101001
```

Receiver:

```text
1101001
```

Can receiver detect the error?

**YES.**

Can receiver correct it?

**YES, if the valid-codeword set uniquely identifies `0101001`.**

The lecture shows that the possible invalid words around valid codewords must not overlap.

---

## Q29. Why must invalid regions not overlap?

If two valid codewords have overlapping error regions, a received word could be equally associated with two possible transmitted codewords.

Then the receiver cannot uniquely correct it.

Therefore:

[  
\boxed{\text{For guaranteed correction, error regions must not overlap}}  
]

---

# 10. Correction Using (d_{\min})

## Q30. If (d_{\min}=4), how many errors can be detected?

[  
4-1=\boxed{3}  
]

But correction:

[  
\left\lfloor\frac{4-1}{2}\right\rfloor  
=\boxed{1}  
]

So:

[  
\boxed{\text{Detect 3, correct 1}}  
]

The lecture emphasizes that after 4 flips, the result **may** become another valid codeword.

---

## Q31. If minimum Hamming distance is (d), how many errors can be corrected?

[  
\boxed{  
\left\lfloor\frac{d-1}{2}\right\rfloor  
}  
]

---

# 11. Detection Formula

## Q32. What is the minimum Hamming distance required to guarantee detection of up to (s) errors?

[  
\boxed{d_{\min}=s+1}  
]

---

# 12. Correction Formula

## Q33. To guarantee correction of up to (s) errors, what should minimum Hamming distance be?

[  
\boxed{d_{\min}=2s+1}  
]

The lecture explicitly asks this question with options:

```text
2s
2s-1
2s+1
None
```

### Answer:

[  
\boxed{2s+1}  
]

---

## Q34. If we want to correct (s) errors, how many invalid words are there between two valid words?

The lecture considers:

```text
Valid A ● ─── invalid ─── ● Valid B
```

For correction of (s) errors:

[  
\boxed{2s\text{ invalid patterns are needed between valid codewords}}  
]

Therefore:

[  
\boxed{d_{\min}=2s+1}  
]

---

# 13. (d_{\min}=8)

## Q35. If minimum Hamming distance is 8, how many errors can be corrected?

# [  
\left\lfloor\frac{8-1}{2}\right\rfloor

# \left\lfloor\frac72\right\rfloor

\boxed3  
]

So:

[  
\boxed{\text{3 errors}}  
]

---

# 14. (d_{\min}=15)

## Q36. If minimum Hamming distance is 15, how many errors can be corrected?

Number of errors between two valid codewords:

[  
15-1=14  
]

Correction capability:

# [  
\frac{14}{2}

\boxed7  
]

Therefore:

[  
\boxed{\text{Correct up to 7 bits}}  
]

Detection:

[  
15-1=\boxed{14}  
]

So:

[  
\boxed{\text{Detect 14, correct 7}}  
]

---

# 15. (d_{\min}=7)

## Q37. If (d_{\min}=7), how many errors can be corrected and detected?

Detection:

[  
7-1=\boxed6  
]

Correction:

[  
\left\lfloor\frac{7-1}{2}\right\rfloor  
=\boxed3  
]

Therefore:

[  
\boxed{\text{Detect 6, correct 3}}  
]

---

# 16. NIELIT Question

## Q38. To guarantee correction of up to 5 errors in all cases, minimum Hamming distance?

Options:

- 11
    
- 6
    
- 5
    
- 2
    

Use:

[  
d_{\min}=2t+1  
]

[  
=2(5)+1  
=\boxed{11}  
]

### Answer:

[  
\boxed{11}  
]

---

# 17. UGC NET Question

## Q39. Four valid codewords have minimum Hamming distance 5. If received codeword is `000001111`, what was original codeword?

The received word can have at most:

[  
\frac{5-1}{2}=2  
]

errors corrected.

Therefore compare received word with all valid codewords and choose the unique valid codeword within distance 2.

### Answer given in lecture:

[  
\boxed{A=00001111}  
]

---

# 18. Two-Out-of-Five Code

## Q40. A "two-out-of-five" code consists of all binary words of length 5 containing exactly two 1s.

### (a) How many codewords?

Choose 2 positions out of 5:

# [  
\binom52

\boxed{10}  
]

---

### (b) What is minimum Hamming distance?

[  
\boxed{d_{\min}=2}  
]

---

### (c) How many errors can the code detect?

# [  
d_{\min}-1

# 2-1

\boxed1  
]

---

### (d) How many errors can it correct?

# [  
\left\lfloor\frac{2-1}{2}\right\rfloor

\boxed0  
]

Therefore:

[  
\boxed{\text{Detect 1, correct 0}}  
]

---

# 19. Four-Out-of-Seven Code

## Q41. A "four-out-of-seven" code consists of all binary words of length 7 containing exactly four 1s.

### (a) Number of codewords?

# [  
\binom74

\boxed{35}  
]

---

### (b) Minimum Hamming distance?

[  
\boxed{d_{\min}=2}  
]

---

### (c) How many errors can it detect?

[  
2-1=\boxed1  
]

---

### (d) How many errors can it correct?

[  
\boxed0  
]

Therefore:

[  
\boxed{\text{Detect 1, correct 0}}  
]

---

# 20. Five-Bit Code Question

Given:

```text
A = 01100
B = 11010
C = 10101
D = 00011
```

## Q42. What is the Hamming distance of this code?

Minimum distance between any two valid codewords:

[  
\boxed{d_{\min}=3}  
]

Therefore:

[  
\boxed{\text{Correct 1 bit}}  
]

and

[  
\boxed{\text{Detect 2 bits}}  
]

---

# 21. If the Channel Has At Most One Bit Error

## Q43. Given the above 5-bit code and received codeword:

```text
X = 11101
```

Can the receiver correct it?

**YES.**

Because the channel guarantees at most one bit error and the code has:

[  
d_{\min}=3  
]

so it can correct:

[  
\left\lfloor\frac{3-1}{2}\right\rfloor=1  
]

bit.

The transmitted codeword is:

[  
\boxed{C}  
]

---

# 22. Exactly Two-Bit Error

## Q44. Same code, but exactly two bits are flipped.

Can the received codeword always be corrected?

The lecture's answer:

[  
\boxed{\text{YES, if it is known that exactly two bits or zero bits were flipped}}  
]

The only valid codeword at the required distance identifies the transmitted codeword in the given example.

---

# 23. Channel Property Question

## Q45. If the channel can produce either 0 errors or exactly 2 errors, can the code correct it?

For the given code:

```text
A = 01100
B = 11010
C = 10101
D = 00011
```

the lecture demonstrates that the received word can be mapped to the appropriate codeword under this known channel property.

### Answer:

[  
\boxed{\text{YES}}  
]

---

# 24. Common Case: Detection + Correction

## Q46. If a code must correct (c) errors and detect (d) errors, what is the minimum Hamming distance?

For:

[  
d\ge2c  
]

the lecture gives:

[  
\boxed{d_{\min}=d+c+1}  
]

---

# 25. Final MSQ

## Q47. An error-control code has minimum Hamming distance 11. Which are possible?

A. Detecting 11 bits of error  
B. Detecting 5 bits of error  
C. Correcting 5 bits of error  
D. Correcting 11 bits of error

### Detection:

[  
11-1=10  
]

So:

- A → ❌
    
- B → ✅
    

### Correction:

[  
\left\lfloor\frac{10}{2}\right\rfloor=5  
]

So:

- C → ✅
    
- D → ❌
    

### Answer:

[  
\boxed{B,C}  
]

---

# 🔥 Final GATE Question Formula Set

These are the formulas you should be able to apply to **almost every numerical question in this lecture**:

|Given|Answer|
|---|---|
|(d_{\min})|Detect (d_{\min}-1)|
|(d_{\min})|Correct (\lfloor(d_{\min}-1)/2\rfloor)|
|Detect (s) errors|(d_{\min}\ge s+1)|
|Correct (c) errors|(d_{\min}\ge2c+1)|
|Single parity|(d_{\min}=2)|
|Single parity detection|1 error guaranteed|
|Single parity correction|0 errors|
|(d_{\min}=3)|Detect 2, correct 1|
|(d_{\min}=5)|Detect 4, correct 2|
|(d_{\min}=7)|Detect 6, correct 3|
|(d_{\min}=11)|Detect 10, correct 5|
|(d_{\min}=15)|Detect 14, correct 7|

The lecture's final summary gives the same detection/correction relationships.

### One thing I want you to keep crystal clear

```text
                 dmin
                  │
        ┌─────────┴─────────┐
        │                   │
     Detection           Correction
        │                   │
    dmin - 1       floor((dmin-1)/2)
```

So if GATE gives you **minimum Hamming distance**, don't start thinking about parity or codewords first. **Immediately convert (d_{\min}) into detection/correction capability.** That is the recurring pattern throughout this lecture.