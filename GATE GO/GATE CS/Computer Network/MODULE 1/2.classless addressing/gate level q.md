# Classful & Classless Addressing — Questions

## Core Rules

### Network Address
Set all **Host ID bits = 0**.

### Broadcast Address
Set all **Host ID bits = 1**.

### CIDR `/n`

- Network bits = `n`
- Host bits = `32 - n`
- Total addresses = `2^(32-n)`
- Network Address → Host bits = all `0`
- Broadcast Address → Host bits = all `1`

---

# Classful Addressing Questions

## Q1. Find Direct Broadcast Address

**Given:**

172.30.10.5 (Class B)

Class B:

Network = first 16 bits  
Host = last 16 bits

So:

172.30 | 10.5

For Broadcast Address:

Host bits → all 1

Therefore:

172.30 | 255.255

**Answer:**

172.30.255.255

---

## Q2. Find Direct Broadcast Address

**Given:**

10.20.15.8 (Class A)

Class A:

Network = first 8 bits  
Host = remaining 24 bits

10 | 20.15.8

Broadcast:

10 | 255.255.255

**Answer:**

10.255.255.255

---

## Q3. Find Network Address

**Given:**

132.15.44.9 (Class B)

Class B:

Network = first 16 bits  
Host = last 16 bits

132.15 | 44.9

For Network Address:

Host bits → all 0

132.15 | 0.0

**Answer:**

132.15.0.0

---

## Q4. Find Network Address

**Given:**

192.50.11.25 (Class C)

Class C:

Network = first 24 bits  
Host = last 8 bits

192.50.11 | 25

Network Address:

Host bits → all 0

192.50.11 | 0

**Answer:**

192.50.11.0

---

# Number of Addresses

## Q5. How many Class A networks are possible?

Class A begins with:

0xxxxxxx

The first bit is fixed.

Remaining network bits = 7

Number of combinations:

2^7 = 128

**Answer: 128**

---

## Q6. How many addresses exist inside one Class A network?

Class A has:

8 Network bits  
24 Host bits

Therefore:

Total addresses = 2^24

Usable host addresses:

2^24 - 2

The two reserved addresses are:

- Network Address
- Broadcast Address

---

# Why Classful Addressing Was Wasteful

## Q7. Organization needs 5000 IP addresses. What is the problem?

Class C provides:

2^8 = 256 addresses

Not enough.

Class B provides:

2^16 = 65,536 addresses

Enough, but far too many.

If approximately 5000 are needed:

65536 - 5000 = 60,536

A huge number of addresses are wasted.

This is one major reason for moving from **Classful Addressing → Classless Addressing (CIDR)**.

---

# Classless Addressing / CIDR

## Fundamental Idea

In Classless Addressing there is no fixed Class A/B/C boundary.

The prefix is explicitly given:

IP/n

Example:

192.168.1.18/28

`/28` means:

28 Network bits  
32 - 28 = 4 Host bits

---

# CIDR Questions

## Q8. Given 167.199.170.82/27, find:

1. Number of addresses
2. First address
3. Last address

### Step 1 — Host Bits

/27

Host bits:

32 - 27 = 5

### Step 2 — Number of Addresses

2^5 = 32

### Step 3 — Last Octet

82 in binary:

82 = 01010010

Because the first 24 bits are already in the first three octets, `/27` takes another 3 network bits.

Split:

010 | 10010
^^^   ^^^^^
Net    Host

### First Address / Network Address

Host bits → all 0

010 | 00000

= 01000000
= 64

**First Address:**

167.199.170.64/27

### Last Address / Broadcast

Host bits → all 1

010 | 11111

= 01011111
= 95

**Last Address:**

167.199.170.95/27

### Final Answer

Number of addresses = 32  
First Address = 167.199.170.64  
Last Address = 167.199.170.95

---

## Q9. Given 205.16.37.39/28, how many total addresses?

### Host Bits

32 - 28 = 4

Therefore:

Total addresses = 2^4

= 16

**Answer: 16**

---

## Q10. Given 205.16.37.39/28, find Broadcast Address

### Step 1

/28 means:

Network bits = 28  
Host bits = 4

First three octets already contain 24 bits.

Therefore the last octet is:

NNNN | HHHH

### Step 2 — Convert 39

39 = 00100111

Split:

0010 | 0111
^^^^   ^^^^
Net    Host

### Step 3 — Broadcast

Broadcast means:

Host bits → ALL 1

0010 | 1111

= 00101111

Convert to decimal:

32 + 8 + 4 + 2 + 1

= 47

**Answer:**

205.16.37.47

---

## Q11. Given 192.168.1.18/28, find Network ID

### Step 1

/28:

Network bits = 28  
Host bits = 4

### Step 2

18 in binary:

00010010

Split:

0001 | 0010
^^^^   ^^^^
Net    Host

### Step 3 — Network Address

Network Address means:

Host bits → ALL 0

0001 | 0000

= 00010000

= 16

**Answer:**

192.168.1.16

### Extra

Broadcast:

0001 | 1111

= 31

Therefore this block is:

Network   = 192.168.1.16
First Host = 192.168.1.17
Last Host = 192.168.1.30
Broadcast = 192.168.1.31

---

## Q12. Given 10.10.10.130/26, find Broadcast Address

### Step 1

/26 means:

Network bits = 26  
Host bits = 32 - 26 = 6

The first three octets already contain 24 network bits.

Therefore last octet:

NN | HHHHHH

### Step 2 — Convert 130

130 = 10000010

Split:

10 | 000010
^^   ^^^^^^
Net   Host

### Step 3 — Broadcast

Host bits → ALL 1

10 | 111111

= 10111111

Convert:

128 + 32 + 16 + 8 + 4 + 2 + 1

= 191

**Answer:**

10.10.10.191

### Extra

Network Address:

10 | 000000

= 10000000

= 128

Therefore:

Network   = 10.10.10.128
Broadcast = 10.10.10.191

---

# Same Network Question

## Q13. Which addresses represent the same /17 network as 152.3.128.0/17?

/17 means:

17 Network bits  
15 Host bits

The first two octets already provide:

16 bits

So we need one additional network bit from the third octet.

152.3.128.0

Third octet:

128 = 10000000

Split:

1 | 0000000
^
Network bit

Therefore the network begins at:

152.3.128.0

For the last address:

Keep the network bit `1`.

Set remaining host bits → all 1.

Third octet:

1 | 1111111

= 255

Fourth octet also becomes:

255

Therefore:

Network Address:
152.3.128.0

Broadcast Address:
152.3.255.255

So every address from:

152.3.128.0
to
152.3.255.255

belongs to this `/17` block.

---

# ⭐ Binary Method — Most Important

Given:

IP/n

### Step 1

Host bits:

h = 32 - n

### Step 2

Convert the octet containing the Network/Host boundary into binary.

### Step 3 — Split

Example `/28`:

NNNN | HHHH

Example `/27`:

NNN | HHHHH

Example `/26`:

NN | HHHHHH

### Step 4

For **Network Address**:

Host bits → ALL 0

### Step 5

For **Broadcast Address**:

Host bits → ALL 1

---

# Quick Pattern

/24 → 8 Host bits → 256 addresses

/25 → 7 Host bits → 128 addresses

/26 → 6 Host bits → 64 addresses

/27 → 5 Host bits → 32 addresses

/28 → 4 Host bits → 16 addresses

/29 → 3 Host bits → 8 addresses

/30 → 2 Host bits → 4 addresses

---

# 🧠 One-Line Memory Rule

Network Address
→ Host bits = 000000...

Broadcast Address
→ Host bits = 111111...

Total Addresses
→ 2^(Host Bits)

Host Bits
→ 32 - Prefix