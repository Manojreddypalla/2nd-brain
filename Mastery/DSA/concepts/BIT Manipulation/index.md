Since you're preparing for **Infosys L1/L2/L3** and also building a strong DSA foundation, I'd learn Bit Manipulation in layers instead of memorizing random tricks.

---

# Bit Manipulation Roadmap

## Module 0 — Binary Fundamentals ⭐⭐⭐⭐⭐

> Goal: Think in binary instead of decimal.

-  Decimal ↔ Binary conversion
    
-  Powers of 2
    
-  Bit positions
    
-  MSB vs LSB
    
-  Binary representation of positive numbers
    
-  Two's Complement (negative numbers)
    
-  Why computers store integers in binary
    

---

## Module 1 — Bitwise Operators ⭐⭐⭐⭐⭐

Understand how each operator changes bits.

-  AND (`&`)
    
-  OR (`|`)
    
-  XOR (`^`)
    
-  NOT (`~`)
    
-  Left Shift (`<<`)
    
-  Right Shift (`>>`)
    

Know:

- Truth tables
    
- Binary examples
    
- Common use cases
    

---

## Module 2 — Basic Bit Tricks ⭐⭐⭐⭐⭐

These are asked everywhere.

### Check bit

```cpp
n & (1 << i)
```

---

### Set bit

```cpp
n |= (1 << i);
```

---

### Clear bit

```cpp
n &= ~(1 << i);
```

---

### Toggle bit

```cpp
n ^= (1 << i);
```

---

### Even / Odd

```cpp
n & 1
```

---

### Multiply/Divide by 2

```cpp
n << 1
n >> 1
```

---

## Module 3 — Famous Bit Tricks ⭐⭐⭐⭐☆

Must memorize these.

### Remove lowest set bit

```cpp
n &= (n - 1);
```

---

### Lowest set bit

```cpp
n & (-n)
```

---

### Check Power of Two

```cpp
n > 0 && !(n & (n - 1))
```

---

### Count set bits

```cpp
__builtin_popcount(n)
```

---

### Brian Kernighan Algorithm

```cpp
while(n){
    n &= (n-1);
    count++;
}
```

---

## Module 4 — XOR Patterns ⭐⭐⭐⭐⭐

Probably the most important interview topic.

Know why:

```cpp
a ^ a = 0

a ^ 0 = a

a ^ b ^ a = b
```

Problems

-  Single Number
    
-  Missing Number
    
-  Find Duplicate
    
-  Swap numbers using XOR (understand it, but prefer `std::swap`)
    

---

## Module 5 — Built-in Functions ⭐⭐⭐☆

Know these.

```cpp
__builtin_popcount()

__builtin_popcountll()

__builtin_parity()

__builtin_clz()

__builtin_ctz()
```

---

## Module 6 — Bit Masking ⭐⭐⭐⭐☆

This is where bit manipulation becomes powerful.

Represent

```text
00010110
```

as

```text
8 Boolean variables
```

Learn

-  Mask
    
-  Setting flags
    
-  Checking permissions
    
-  State compression
    

---

## Module 7 — Subsets using Bitmask ⭐⭐⭐⭐☆

Very common.

```cpp
for(mask=0;mask<(1<<n);mask++)
```

Generate all subsets.

Understand

```
000
001
010
011
100
...
```

---

## Module 8 — Common Interview Problems ⭐⭐⭐⭐⭐

Do these.

### Easy

-  Single Number
    
-  Missing Number
    
-  Number of 1 Bits
    
-  Power of Two
    
-  Counting Bits
    
-  Hamming Distance
    
-  Reverse Bits
    

---

### Medium

-  Single Number II
    
-  Single Number III
    
-  Subsets
    
-  Minimum Flips
    
-  Maximum Product of Word Lengths
    

---

## Module 9 — Advanced (Optional)

Skip this until you're comfortable with the basics.

-  Bitmask DP
    
-  SOS DP
    
-  Trie + XOR
    
-  Gaussian Elimination with XOR
    
-  Meet in the Middle
    
-  Fenwick Tree (`lowbit = x & -x`)
    
-  Traveling Salesman (Bitmask DP)
    

---

# Roadmap Summary

```text
Binary Basics
      │
      ▼
Bitwise Operators
      │
      ▼
Basic Bit Tricks
      │
      ▼
Power of Two
Count Bits
Remove Last Bit
      │
      ▼
XOR Tricks
      │
      ▼
Bit Masking
      │
      ▼
Subset Generation
      │
      ▼
Interview Problems
      │
      ▼
Advanced Bitmask DP (Optional)
```

---

# Priority for Infosys L1/L2/L3

### Must Learn (100%)

- ✅ Binary basics
    
- ✅ All operators
    
- ✅ Check/Set/Clear/Toggle bit
    
- ✅ Even/Odd
    
- ✅ XOR properties
    
- ✅ Power of Two
    
- ✅ Count set bits
    
- ✅ Brian Kernighan algorithm
    
- ✅ Bitmask subsets
    
- ✅ 10–15 LeetCode problems
    

### Can Skip for Now

- ❌ Bitmask DP
    
- ❌ SOS DP
    
- ❌ XOR Trie
    
- ❌ Advanced competitive programming tricks
    
- ❌ Complex bit hacks
    

---

## Estimated Study Time

- **Module 0–3:** 2–3 hours
    
- **Module 4 (XOR):** 2 hours
    
- **Module 5–7:** 2–3 hours
    
- **Practice (15–20 problems):** 5–7 hours
    

**Total:** Around **12–15 hours** to build a solid interview-ready foundation, which is enough for most Infosys coding rounds while leaving your main study time for higher-priority DSA topics like trees, graphs, and dynamic programming.