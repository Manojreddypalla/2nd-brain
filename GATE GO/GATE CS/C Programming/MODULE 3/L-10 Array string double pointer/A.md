# L-10 — Quick Learning Notes: Pages 1–34

## 1. Endianness

When a value occupies **multiple bytes**, there are two common ways to arrange those bytes in memory:

- **Big Endian**
    
- **Little Endian**
    

Endianness is about **byte order in memory**, not the numerical value itself.

---

## 2. Example: `int a = 5`

Assume `int = 4 bytes`.

```text
5 = 00000000 00000000 00000000 00000101
```

Memory addresses:

```text
1000   1001   1002   1003
```

### Big Endian

**Most Significant Byte (MSB) first**

```text
1000 → 00000000
1001 → 00000000
1002 → 00000000
1003 → 00000101
```

### Little Endian

**Least Significant Byte (LSB) first**

```text
1000 → 00000101
1001 → 00000000
1002 → 00000000
1003 → 00000000
```

### 🧠 Remember

```text
Big    → Big/MSB byte first
Little → Little/LSB byte first
```

---

## 3. Endianness does NOT change the value

```c
int i = 5;
printf("%d", i);
```

Output is:

```text
5
```

in both Big and Little Endian systems.

Why?

Because the system knows its own byte ordering and reconstructs the correct integer value.

---

## 4. Endianness applies to multi-byte data

Endianness matters when a single value occupies **more than one byte**.

```text
int       → multiple bytes → endian matters
short     → multiple bytes → endian matters
char      → 1 byte         → endian doesn't matter
```

A **1-byte value cannot have its byte order reversed**.

---

## 5. VERY IMPORTANT: Array order doesn't change

Consider:

```c
int a[] = {1, 2, 3, 4};
```

Both systems still have:

```text
a[0] = 1
a[1] = 2
a[2] = 3
a[3] = 4
```

Endianness **does NOT reverse array elements**.

It only changes the **byte arrangement inside each individual element**.

### ❌ Wrong

```text
Little endian → 4 3 2 1
```

### ✅ Correct

```text
Little endian → 1 2 3 4
Big endian    → 1 2 3 4
```

But the bytes making up each `int` can be arranged differently.

---

## 6. `char` array

```c
char c[] = {'a','b','c','d'};
```

Each element is **1 byte**.

Therefore:

```text
Little endian → a b c d
Big endian    → a b c d
```

No byte reversal is possible because each element itself occupies only one byte.

---

# POINTER + ENDIANNESS

## 7. Any pointer can point to an address

Example:

```c
int i = 5;

int *p;
short int *s;
char *c;
```

All three pointers can potentially contain the **same memory address**.

But the pointer type determines **how that memory is interpreted/accessed**.

Think:

```text
Memory
   ↓
same bytes
   ↓
char*   → reads 1 byte
short*  → reads 2 bytes
int*    → reads 4 bytes
```

This is the key idea behind the questions in these pages.

---

## 8. Pointer type controls access size

Suppose:

```c
int i = 5;
```

and:

```c
char *c = (char *)&i;
```

Then:

```c
*c
```

does **not** mean "give me the complete integer."

It means:

> Read the **first byte** at the address of `i` as a `char`.

---

# GATE PATTERN: READ BYTES USING POINTERS

## 9. Example: `int i = 511`

```text
511 = 00000000 00000000 00000001 11111111
```

Four bytes:

```text
00 | 00 | 01 | FF
```

### Big Endian

```text
Address →  1000   1001   1002   1003
           00     00     01     FF
```

### Little Endian

```text
Address →  1000   1001   1002   1003
           FF     01     00     00
```

---

## 10. `char *` + `int`

Suppose:

```c
int i = 511;
signed char *p = (char *)&i;
```

`p` points to the **first byte** of `i`.

### Little Endian

First byte:

```text
11111111
```

As signed `char`:

```text
11111111 = -1
```

So:

```c
*p
```

gives:

```text
-1
```

The lecture demonstrates this exact case.

---

## 11. Signed vs unsigned `char`

Same memory byte:

```text
11111111
```

### Signed char

```text
11111111 → -1
```

### Unsigned char

```text
11111111 → 255
```

So **the bytes haven't changed**.

Only the **interpretation** changes.

This is a very important GATE idea.

---

# 12. `int i = 255`

```text
255 =
00000000 00000000 00000000 11111111
```

### Little Endian

```text
Address:
1000 → 11111111
1001 → 00000000
1002 → 00000000
1003 → 00000000
```

### Big Endian

```text
Address:
1000 → 00000000
1001 → 00000000
1002 → 00000000
1003 → 11111111
```

---

## 13. Reading `int` through `int *`

```c
int i = 255;
int *p = &i;

printf("%d", *p);
```

`p` reads the complete `int`.

Therefore:

```text
Little endian → 255
Big endian    → 255
```

The system reconstructs the integer according to its own byte ordering.

---

# 14. Why pointer-based questions become tricky

Suppose:

```c
int i = 255;
short int *s = (short int *)&i;
```

Now `s` reads **2 bytes**, not 4.

So we need to ask:

> **Which 2 bytes are at the address where `s` points?**

Then interpret those bytes as a `short`.

This is where **endianness + pointer type** combine.

---

## 15. `255` with `short *`

Memory:

```text
255 = 00 00 00 FF
```

### Little Endian

```text
Address
1000 → FF
1001 → 00
1002 → 00
1003 → 00
```

`short *s` reads:

```text
FF 00
```

After interpreting according to little endian:

```text
255
```

### Big Endian

```text
1000 → 00
1001 → 00
1002 → 00
1003 → FF
```

`short *s` reads:

```text
00 00
```

Therefore:

```text
0
```

The lecture's page 33 explicitly demonstrates this difference.

---

# 16. The core mental model 🧠

When you see a GATE question involving:

```c
int *p
short *p
char *p
```

**DO NOT immediately calculate the value.**

Do this:

### Step 1 — Write the value in bytes

```text
int → 4 bytes
short → 2 bytes
char → 1 byte
```

### Step 2 — Arrange bytes according to endian

```text
Little → LSB byte first
Big    → MSB byte first
```

### Step 3 — Start at pointer's address

### Step 4 — Read according to pointer type

```text
char*  → 1 byte
short* → 2 bytes
int*   → 4 bytes
```

### Step 5 — Interpret those bytes

This solves basically the entire pattern from these pages.

---

# 17. One-byte vs multi-byte

### `char`

```text
1 byte
```

No endian difference.

### `short`

```text
2 bytes
```

Endian can matter.

### `int`

```text
4 bytes
```

Endian can matter.

**Important:** exact sizes can be implementation-dependent in C, but these lecture examples use the common `int = 4 bytes` assumption.

---

# 18. Endianness + pointer = GATE hotspot

Remember this chain:

```text
             int i = 255
                    ↓
          stored as 4 bytes
                    ↓
           endian decides
         byte arrangement
                    ↓
        pointer chooses how
         many bytes to read
                    ↓
      type chooses interpretation
```

That's the entire concept.

---

# 19. Fast Revision Table

|Concept|Remember|
|---|---|
|Big Endian|MSB byte first|
|Little Endian|LSB byte first|
|Endianness|Byte ordering|
|`char`|1 byte → endian irrelevant|
|Array order|**Never changes**|
|`char *`|Reads 1 byte|
|`short *`|Reads 2 bytes|
|`int *`|Reads 4 bytes in these examples|
|Same address + different pointer type|Different interpretation|
|Signed char `11111111`|`-1`|
|Unsigned char `11111111`|`255`|
|Endianness changes value?|**No**|
|Endianness changes array order?|**No**|
|Endianness changes byte layout?|**Yes**|

---

## 🔥 GATE Shortcut

Whenever you see:

```c
int x = something;
char *p = (char *)&x;
```

immediately think:

> **"They're asking me to peek at the bytes of `x`."**

And if you see:

```c
short *p = (short *)&x;
```

think:

> **"They're asking me to peek at 2 bytes of `x`."**

Then **draw the bytes**. Don't try to mentally calculate it.

The pages 22–33 are basically training this exact skill through `511`, `255`, `char*`, `unsigned char*`, and `short*` examples.

**Page 34 is essentially a transition/blank slide**, so there isn't a new concept to add from it.