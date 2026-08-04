# ⚙️ Systems C — Day 2: Integers Are Just Bits

**Day 1:** variables → bytes → addresses ✅  
**Day 2:** bits → integer representation → signed/unsigned → overflow

Today we're building something you'll need constantly later for **bit manipulation, memory, binary formats, hardware registers, networking, and kernel code**.

⏱️ **40–50 min**

## 1. Core intuition — memory doesn't contain “numbers”

Suppose:

```
unsigned char x = 5;
```

We humans see:

```
x = 5
```

Memory fundamentally contains bits:

```
00000101
```

The important idea is:

> **Bits don't inherently know what they mean. The type tells C how to interpret them.**

For example, this 8-bit pattern:

```
11111111
```

could be interpreted as:

```
unsigned → 255

signed   → -1
```

Same bits.

Different interpretation.

---

# 2. Unsigned integers

With **N bits**, there are:

```
2^N
```

possible bit patterns.

For 8 bits:

```
00000000
00000001
00000010
...
11111111
```

That's 256 possibilities.

So an 8-bit **unsigned** integer represents:

```
0 → 255
```

More generally:

```
0 → 2^N - 1
```

For 8 bits:

```
2^8 - 1 = 255
```

Visualize the boundaries:

```
00000000 =   0
00000001 =   1
00000010 =   2
...
11111110 = 254
11111111 = 255
```

Every bit contributes to the magnitude.

---

# 3. Signed integers

We also need negative numbers.

Modern C systems overwhelmingly use **two's complement** representation.

For an 8-bit signed integer:

```
00000000 =    0
00000001 =    1
...
01111111 =  127

10000000 = -128
...
11111111 =   -1
```

So the range is:

```
-128 → 127
```

More generally for N bits:

```
-2^(N-1) → 2^(N-1) - 1
```

Why isn't it:

```
-127 → +127
```

Because there are still:

```
256
```

possible patterns.

We need one representation for zero, leaving the range asymmetric:

```
128 negative values
128 non-negative values
```

---

# 4. Derive `-1`

This is worth understanding once.

Start with:

```
1

00000001
```

To obtain its two's-complement negative:

### Flip every bit

```
00000001
↓
11111110
```

### Add 1

```
11111110
       1
────────
11111111
```

Therefore:

```
11111111
```

represents `-1` in 8-bit two's complement.

Similarly:

```
5

00000101

flip:
11111010

+1:
11111011
```

So:

```
11111011
```

represents `-5`.

Don't turn this into a memorization ritual. The useful intuition is that two's complement lets binary arithmetic work elegantly across positive and negative values.

---

# 🔬 5. Lab — inspect integer sizes

Create:

```
day02.c
```

Start with:

```
#include <stdio.h>

int main(void)
{
    unsigned int a = 10;
    signed int b = -10;

    printf("a = %u\n", a);
    printf("b = %d\n", b);

    printf("unsigned int size = %zu bytes\n", sizeof(a));
    printf("signed int size   = %zu bytes\n", sizeof(b));

    return 0;
}
```

Compile:

```
gcc -Wall -Wextra day02.c -o day02
```

Run:

```
./day02
```

On a typical machine you'll probably find:

```
unsigned int size = 4 bytes
signed int size   = 4 bytes
```

Meaning:

```
4 bytes × 8 bits
        ↓
      32 bits
```

But don't bake “int = 32 bits everywhere” into your brain. C's standard integer types have rules and minimum ranges, while exact-width types exist when you specifically need exact widths.

---

# 6. Exact-width integers

Systems programming often needs something stronger than:

```
int
```

Suppose you're dealing with a binary field that must be **exactly 32 bits**.

C provides integer types through:

```
#include <stdint.h>
```

such as:

```
uint8_t
uint16_t
uint32_t
uint64_t

int8_t
int16_t
int32_t
int64_t
```

Read:

```
uint32_t
│ │  │
│ │  └─ type
│ └──── 32 bits
└────── unsigned integer
```

So:

```
uint32_t x;
```

means an unsigned integer type of exactly 32 bits, on implementations that provide that exact-width type.

Try:

```
#include <stdio.h>
#include <stdint.h>

int main(void)
{
    uint8_t  a = 10;
    uint16_t b = 10;
    uint32_t c = 10;
    uint64_t d = 10;

    printf("%zu\n", sizeof(a));
    printf("%zu\n", sizeof(b));
    printf("%zu\n", sizeof(c));
    printf("%zu\n", sizeof(d));

    return 0;
}
```

You should typically see:

```
1
2
4
8
```

This distinction will become extremely useful later.

---

# 7. Unsigned overflow — think of a clock

Now:

```
#include <stdio.h>
#include <stdint.h>

int main(void)
{
    uint8_t x = 255;

    printf("before = %u\n", (unsigned)x);

    x++;

    printf("after  = %u\n", (unsigned)x);

    return 0;
}
```

Think **before running it**.

An 8-bit unsigned value has:

```
11111111
```

which is:

```
255
```

Add one:

```
  11111111
+        1
──────────
1 00000000
```

But `x` only has 8 bits.

The low 8 bits are:

```
00000000
```

So:

```
255
 ↓ +1
 0
```

Unsigned arithmetic is defined modulo `2^N` for an N-bit unsigned type.

Mental model:

```
          255
           │
250 ───────┼───────
           │
           ▼
0 ◄────────┘
│
1
│
2
```

Like a clock wrapping around.

---

# ⚠️ 8. Signed overflow is different

This distinction is **very important in C**.

Don't assume:

```
signed char x = 127;
x++;
```

must behave like:

```
127 → -128
```

For signed integer arithmetic, overflowing the representable range is generally **undefined behavior**.

That means C does not promise the wraparound semantics you get with unsigned arithmetic.

So remember:

```
UNSIGNED OVERFLOW
→ defined modulo arithmetic


SIGNED OVERFLOW
→ undefined behavior
```

This distinction matters enormously when compilers optimize low-level code.

---

# 9. One subtle trap

Consider:

```
unsigned int x = 10;

if (x < 0) {
    printf("negative\n");
}
```

Can this condition ever be true?

No.

`x` is unsigned.

Its range starts at:

```
0
```

This sounds obvious now, but mixing signed and unsigned values can produce surprisingly nasty bugs.

We'll revisit integer conversions later when they become relevant.

---

# 🔨 Today's challenge

Write a program containing:

```
uint8_t a;
int8_t b;
uint16_t c;
uint32_t d;
```

Print each one's:

```
value
size
address
```

Then experiment with:

```
uint8_t x = 255;

printf("%u\n", (unsigned)x);

x++;

printf("%u\n", (unsigned)x);
```

Before executing it, predict the output.

Then change:

```
255
```

to:

```
254
```

and increment twice.

Predict again.

The point isn't the answer.

It's training yourself to visualize:

```
variable
   ↓
fixed number of bits
   ↓
finite set of patterns
   ↓
type determines interpretation
```

---

# 🧠 Day 2 reasoning check

You should now be able to reason about this:

```
11111111
```

and say:

> “You haven't given me enough information to tell you its numerical value.”

Because if it's:

```
uint8_t
```

you interpret it one way.

If it's:

```
int8_t
```

you interpret it differently.

That's systems thinking.

---

# 📝 Day 2 Revision Note

```
# Systems C — Day 2

Memory fundamentally stores bits.

Type determines how bits are interpreted.


Unsigned N-bit range:

0 → 2^N - 1


Signed N-bit range:

-2^(N-1) → 2^(N-1) - 1


Example 8-bit:

unsigned:
0 → 255

signed:
-128 → 127


<stdint.h>

uint8_t
uint16_t
uint32_t
uint64_t

int8_t
int16_t
int32_t
int64_t


Unsigned overflow:
defined modulo 2^N

255 + 1 in uint8_t
→ 0


Signed overflow:
undefined behavior


Key intuition:

bits ≠ meaning

bits + type
→ interpretation
```

## 🎯 Where we are

```
Day 1 ✅
Variables → bytes → addresses

Day 2 ✅
Bytes → bits → integer representation
              ↓
       signed / unsigned
              ↓
           overflow

Day 3
              ↓
       POINTERS BEGIN
```