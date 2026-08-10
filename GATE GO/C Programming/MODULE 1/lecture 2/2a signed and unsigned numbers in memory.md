### 1. Signed integer representation

For a **signed integer**, the MSB (Most Significant Bit) acts as the **sign bit** in the 2's complement representation:

- `0` → positive
- `1` → negative

Example: 8-bit signed `-9`

```
+9  = 00001001
~9  = 11110110
+1  = 11110111
```

So:

```
-9 → 11110111
```

The important point is that **the bits are stored in memory**; signed/unsigned determines **how those bits are interpreted**. The lecture demonstrates this using `int y = -9`.

---

### 2. Unsigned integer representation

For an **unsigned integer**, there is **no sign bit**.

All bits contribute to the magnitude.

For an `N`-bit unsigned number:

```
Range = 0 to 2^N - 1
```

Example for 8 bits:

```
00000000 → 0
11111111 → 255
```

So unsigned values cannot represent negative numbers.

---

## 3. Same bits → different interpretation

This is the **main idea** to remember.

Suppose memory contains:

```
11110111
```

If interpreted as:

```
signed  → -9
unsigned → 247
```

Same bit pattern, different interpretation.

### Mental model

Think of memory as a box containing only:

```
11110111
```

The **data type tells you how to read the box**.

---

## 4. Signed vs Unsigned

||Signed|Unsigned|
|---|---|---|
|Negative values|✅|❌|
|Sign bit|Yes|No|
|8-bit range|`-128` to `127`|`0` to `255`|
|16-bit range|`-2^15` to `2^15-1`|`0` to `2^16-1`|
|32-bit range|`-2^31` to `2^31-1`|`0` to `2^32-1`|

---

# 5. Important `printf()` trap

The lecture shows:

```
int y = -9;

printf("%d", y);
printf("%u", y);
```

`%d` is for **signed decimal** and `%u` is for **unsigned decimal**.

The lecture's key observation is:

```
%d → -9
%u → huge positive number
```

because the same underlying bit pattern is being interpreted differently.

### ⚠️ GATE trap

Don't think:

> `%u` changes the variable into unsigned.

It **doesn't change the stored bits**. The important idea is the **interpretation**.

Also, in strictly conforming C, passing an `int` to `%u` is a format/type mismatch and the behavior is technically undefined. For GATE-style questions, however, follow the representation/conversion rule being tested.

---

# 6. `signed char` vs `unsigned char`

This is especially important because `char` is only **8 bits** in the lecture examples.

### Signed char

```
8 bits
range = -128 to 127
```

Example:

```
signed char f = -65;
```

Memory contains the 8-bit 2's complement representation of `-65`.

The lecture demonstrates:

```
printf("%d", f);   // -65
```

and `%u` producing a large unsigned value after integer promotion.

---

### Unsigned char

```
8 bits
range = 0 to 255
```

Example from the lecture:

```
unsigned char f = -65;
```

The stored 8-bit pattern is interpreted as an unsigned value:

```
-65 → 191
```

because:

```
256 - 65 = 191
```

The lecture shows both `%d` and `%u` producing `191` after the relevant integer promotion.

---

# 7. Very important: Integer Promotion

When `char` or `short` participates in an expression, it is **promoted to `int`**.

```
char / signed char / unsigned char
              ↓
             int
```

The lecture explicitly states:

> Small integer types (`char` or `short`) used in an expression are implicitly converted to `int`.

This is why questions involving `signed char`, multiplication, and `printf()` can look weird.

---

# GATE-Type Questions

### Q1. Consider:

```
signed char x = -9;
```

What is the 8-bit representation of `x`?

```
9      = 00001001
~9     = 11110110
+1     = 11110111
```

**Answer:**

```
11110111
```

---

### Q2. The same 8-bit pattern is:

```
11110111
```

What is its value as:

**(a) signed**

```
-9
```

**(b) unsigned**

```
247
```

because:

```
256 - 9 = 247
```

**Answer:** `-9` signed, `247` unsigned.

---

### Q3. What is the range of an 8-bit unsigned integer?

```
0 → 2^8 - 1
```

**Answer:**

```
0 to 255
```

---

### Q4. What is the range of an 8-bit signed integer using 2's complement?

```
-2^7 → 2^7 - 1
```

**Answer:**

```
-128 to +127
```

---

### Q5. Lecture example

```
unsigned char f = -65;
```

What 8-bit bit pattern gets interpreted?

Since:

```
256 - 65 = 191
```

**Answer:**

```
f represents 191
```

The lecture demonstrates exactly this case.

---

### ⭐ One thing to burn into memory

```
MEMORY
   ↓
bits don't know signed/unsigned
   ↓
DATA TYPE
   ↓
decides how those bits are interpreted
```

So whenever you see a GATE question involving signed/unsigned:

**1. Determine number of bits → 2. Write the bit pattern → 3. Determine signed/unsigned interpretation → 4. Check integer promotion if `char`/`short` participates in an expression.**