You're right. The last one was basically a giant code block, which kills **Obsidian headings, callouts, tables, and LaTeX rendering**.

Use this version directly in Obsidian:

# Number System & 2's Complement — Practice Q&A

> [!info] Purpose  
> Practice for **unsigned integers, signed 2's complement, decimal ↔ binary conversion, sign extension, ranges, and overflow**.

---

## Q1. 7-Bit Integer Range

### Question

With **7 bits**, find:

1. Largest unsigned integer
    
2. Largest positive signed 2's-complement integer
    

### Unsigned

For `n` bits:

$$  
0 \text{ to } 2^n-1  
$$

For 7 bits:

$$  
2^7-1=127  
$$

Binary:

```text
1111111 = 127
```

### Signed 2's Complement

For `n` bits:

$$  
-2^{n-1} \text{ to } 2^{n-1}-1  
$$

For 7 bits:

$$  
-2^6 \text{ to } 2^6-1  
$$

$$  
-64 \text{ to } 63  
$$

Largest positive:

```text
0111111 = 63
```

> [!success] Answer  
> **Largest unsigned:** `1111111 = 127`
> 
> **Largest signed positive:** `0111111 = 63`

---

## Q2. Decimal → 2's Complement Using Minimum Bits

### Question

Represent using the **minimum possible number of bits**:

- `-256`
    
- `-154`
    
- `-107`
    
- `135`
    

### Answer

|Decimal|Minimum Bits|2's Complement|
|--:|--:|---|
|`-256`|9|`100000000`|
|`-154`|9|`101100110`|
|`-107`|8|`10010101`|
|`135`|9|`010000111`|

### How to Find Minimum Bits

For `n`-bit signed 2's complement:

$$  
-2^{n-1}\le x\le2^{n-1}-1  
$$

Choose the **smallest `n`** for which the number fits.

### Example — `135`

8-bit signed range:

$$  
-128\text{ to }127  
$$

`135` does **not** fit.

9-bit signed range:

$$  
-256\text{ to }255  
$$

So `135` needs 9 bits:

```text
135 = 010000111
```

> [!tip] GATE Pattern  
> If the question says **minimum/fewest bits**, find the smallest signed range containing the number.

---

## Q3(a). Convert `-67` to 8-Bit 2's Complement

### Step 1 — Convert `+67`

```text
67 = 01000011
```

### Step 2 — Flip All Bits

```text
01000011
↓
10111100
```

### Step 3 — Add `1`

```text
  10111100
+ 00000001
----------
  10111101
```

> [!success] Answer  
> $$-67 = \boxed{10111101}$$

---

## Q3(b). Convert `1001 0001 1111 1010` to Decimal

Given:

```text
1001 0001 1111 1010
↑
MSB = 1
```

Since:

```text
MSB = 1
```

the number is **negative**.

### Step 1 — Flip

```text
1001 0001 1111 1010
↓
0110 1110 0000 0101
```

### Step 2 — Add `1`

```text
0110 1110 0000 0101
+                   1
---------------------
0110 1110 0000 0110
```

### Step 3 — Convert to Decimal

```text
0110 1110 0000 0110 = 28166
```

Original number was negative.

> [!success] Answer  
> $$\boxed{-28166}$$

---

## Q3(c). Represent `-23` Using 8 Bits

### Step 1 — Convert `+23`

```text
23 = 00010111
```

### Step 2 — Flip

```text
00010111
↓
11101000
```

### Step 3 — Add `1`

```text
11101000
+      1
--------
11101001
```

> [!success] Answer  
> $$-23 = \boxed{11101001}$$

---

## Q3(d). Represent `-23` Using 32 Bits

We already know:

```text
8-bit -23 = 11101001
```

Its MSB is:

```text
1
```

Therefore, when increasing the bit width, copy `1` into the higher-order bits.

```text
8-bit:

11101001


32-bit:

11111111 11111111 11111111 11101001
```

> [!success] Answer  
> `11111111 11111111 11111111 11101001`

> [!important] Sign Extension  
> When increasing the width of a signed 2's-complement number:
> 
> ```text
> MSB = 0 → Extend with 0s
> MSB = 1 → Extend with 1s
> ```

---

## Q4. GATE CSE 2003

Given divisor:

```text
11111011
```

MSB is `1`, so it represents a negative number.

### Convert It

Flip:

```text
11111011
↓
00000100
```

Add `1`:

```text
00000101
```

Therefore:

$$  
11111011=-5  
$$

Now evaluate the choices:

|Option|Binary|Decimal|
|---|---|--:|
|A|`11100111`|`-25`|
|B|`11100100`|`-28`|
|C|`11010111`|`-41`|
|D|`11011011`|`-37`|

We need a number divisible by `-5`.

$$  
\frac{-25}{-5}=5  
$$

> [!success] Answer  
> **Option A — `11100111`**

---

## Q5. GATE CSE 2016

### Question

Convert:

```text
1111 1111 1111 0101
```

to decimal.

### Step 1 — Check MSB

```text
MSB = 1
```

Therefore, negative.

### Step 2 — Flip

```text
1111 1111 1111 0101
↓
0000 0000 0000 1010
```

### Step 3 — Add `1`

```text
0000 0000 0000 1011
```

### Step 4 — Convert

$$  
1011_2=8+2+1=11  
$$

Since the original number was negative:

> [!success] Answer  
> $$\boxed{-11}$$

---

## Q6. Does Signed vs Unsigned Matter for `0101`?

Given:

```text
0101
```

### Unsigned

$$  
0101=4+1=5  
$$

### Signed 2's Complement

MSB is `0`, so convert normally:

$$  
0101=5  
$$

Therefore:

```text
Unsigned → 5
Signed   → 5
```

> [!success] Answer  
> For `0101`, both interpretations give **5**.

> [!warning] Important  
> This is **not true for every bit pattern**.
> 
> Example:
> 
> ```text
> 1011
> 
> Unsigned → 11
> Signed   → -5
> ```
> 
> When MSB = `1`, signed and unsigned interpretations can differ.

---

## Q7. 5-Bit Signed and Unsigned Range

### Signed 2's Complement

Formula:

$$  
-2^{n-1}\text{ to }2^{n-1}-1  
$$

For 5 bits:

$$  
-2^4\text{ to }2^4-1  
$$

$$  
-16\text{ to }15  
$$

Therefore:

```text
10000 → -16
01111 → +15
```

### Unsigned

Formula:

$$  
0\text{ to }2^n-1  
$$

For 5 bits:

$$  
0\text{ to }31  
$$

Therefore:

```text
00000 → 0
11111 → 31
```

> [!success] Answer
> 
> |Type|Minimum|Maximum|
> |---|--:|--:|
> |5-bit Signed|`-16`|`+15`|
> |5-bit Unsigned|`0`|`31`|

---

## Q8. Binary → Decimal

### Part A — Unsigned

#### `1010`

$$  
8+2=10  
$$

```text
1010 → 10
```

#### `10101010`

$$  
128+32+8+2=170  
$$

```text
10101010 → 170
```

#### `11000001`

$$  
128+64+1=193  
$$

```text
11000001 → 193
```

### Answers

|Binary|Unsigned Decimal|
|---|--:|
|`1010`|`10`|
|`10101010`|`170`|
|`11000001`|`193`|

---

### Part B — Signed 2's Complement

#### `1010`

MSB = `1` → negative.

Flip:

```text
1010
↓
0101
```

Add `1`:

```text
0110
```

$$  
0110=6  
$$

Therefore:

```text
1010 → -6
```

---

#### `00110100`

MSB = `0`.

Convert normally:

$$  
32+16+4=52  
$$

Therefore:

```text
00110100 → 52
```

---

#### `11000001`

MSB = `1`.

Flip:

```text
11000001
↓
00111110
```

Add `1`:

```text
00111111
```

$$  
00111111=63  
$$

Therefore:

```text
11000001 → -63
```

### Answers

|Binary|Signed Decimal|
|---|--:|
|`1010`|`-6`|
|`00110100`|`52`|
|`11000001`|`-63`|

---

# Bonus — GATE CSE 2022 Overflow

We are using **4-bit signed 2's-complement numbers**.

### Step 1 — Find the Range

$$  
-2^3\text{ to }2^3-1  
$$

Therefore:

$$  
\boxed{-8\text{ to }+7}  
$$

### Check Options

|Option|First|Second|Addition|Overflow?|
|---|--:|--:|--:|---|
|A|`-5`|`-2`|`-7`|No|
|B|`-4`|`-6`|`-10`|**Yes**|
|C|`+3`|`+4`|`+7`|No|
|D|`-7`|`-1`|`-8`|No|

Why B?

$$  
-4+(-6)=-10  
$$

But 4-bit signed integers can only represent:

$$  
-8\text{ to }+7  
$$

Therefore `-10` cannot be represented.

> [!success] Answer  
> **Option B**

---

# Quick Revision

## Integer Ranges

|Representation|Range|
|---|---|
|`n`-bit Unsigned|$0$ to $2^n-1$|
|`n`-bit Signed 2's Complement|$-2^{n-1}$ to $2^{n-1}-1$|

---

## Decimal → 2's Complement

```text
Positive
   ↓
Convert to binary normally
```

```text
Negative
   ↓
Take magnitude
   ↓
Convert to binary
   ↓
Flip bits
   ↓
Add 1
```

Example:

```text
+5 → 0101

-5:
0101
 ↓ flip
1010
 ↓ +1
1011
```

---

## 2's Complement → Decimal

```text
Check MSB
   ↓
┌─────────────┐
│             │
0             1
↓             ↓
Non-negative  Negative
↓             ↓
Normal        Flip
conversion     ↓
              +1
               ↓
             Decimal
               ↓
             Add -
```

Example:

```text
0101 → +5

1011
↓ flip
0100
↓ +1
0101
↓
5
↓
-5
```

---

## Sign Extension

> [!important]  
> To increase the bit width while preserving a signed 2's-complement value, **copy the MSB into the new higher bits**.

```text
MSB = 0 → Extend with 0s
MSB = 1 → Extend with 1s
```

Examples:

```text
+5:

0101
↓
00000101
```

```text
-5:

1011
↓
11111011
```

---

## GATE Solving Order

> [!tip] Before Calculating  
> Ask these questions in order:
> 
> **1. What is the bit width?**
> 
> **2. Is it signed or unsigned?**
> 
> **3. If signed, what is the MSB?**
> 
> **4. Which conversion direction am I doing?**

Then:

```text
Unsigned
→ Normal binary conversion

Signed + MSB 0
→ Normal binary conversion

Signed + MSB 1
→ Negative number
→ Flip + 1
```

---

# Final Cheat Sheet

```text
UNSIGNED RANGE
0 → 2ⁿ - 1


SIGNED 2's COMPLEMENT RANGE
-2ⁿ⁻¹ → 2ⁿ⁻¹ - 1


POSITIVE DECIMAL → BINARY
Convert normally


NEGATIVE DECIMAL → 2's COMPLEMENT
Magnitude → Binary → Flip → +1


2's COMPLEMENT → DECIMAL

MSB 0
→ Convert normally

MSB 1
→ Flip → +1 → Convert → Add minus sign


SIGN EXTENSION

MSB 0 → Copy 0s to the left

MSB 1 → Copy 1s to the left


OVERFLOW

Result outside representable range
→ Overflow
```

This formatting uses actual Obsidian **H2/H3 headings, callouts (`> [!important]`), LaTeX equations, tables, and individual code blocks**, so it should render properly instead of appearing as one giant block. The questions and answers are based on the homework sheet.