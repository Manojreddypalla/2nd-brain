Yep — here’s the **ultra-short revision version** of your subnetting notes, keeping only the stuff you actually want to recall quickly for GATE.

## 🌐 Subnetting — Short Notes

### 1. Core Idea

```text
IPv4 = 32 bits

IP Address
┌────────────┬────────────┐
│ Network ID │  Host ID   │
└────────────┴────────────┘
```

Subnetting = **borrow Host bits → create Subnet bits**.

```text
Network | Subnet | Host
```

More borrowed bits → **more subnets, fewer hosts/subnet**.

---

### 2. CIDR

```text
Network bits = Prefix

Host bits = 32 - Prefix
```

Example:

```text
/26

Network bits = 26
Host bits = 6
```

---

### 3. Must-Know Formulas ⭐

```text
Host bits = 32 - Prefix

Total IPs = 2^HostBits

Usable Hosts = 2^HostBits - 2

Number of Subnets = 2^BorrowedBits

New Prefix = Old Prefix + BorrowedBits

Borrowed Bits = New Prefix - Old Prefix
```

Network/Broadcast:

```text
Network = IP AND Mask

Broadcast = Network + Block Size - 1

First Host = Network + 1

Last Host = Broadcast - 1
```

---

### 4. CIDR Cheat Sheet ⭐

|CIDR|Mask Last Octet|Total|Usable|
|--:|--:|--:|--:|
|/24|0|256|254|
|/25|128|128|126|
|/26|192|64|62|
|/27|224|32|30|
|/28|240|16|14|
|/29|248|8|6|
|/30|252|4|2|

Mask pattern:

```text
128 → 192 → 224 → 240 → 248 → 252
```

---

### 5. Block Size Trick 🔥

```text
Block Size = 256 - Mask Value
```

Example:

```text
/26 → mask = 192

256 - 192 = 64
```

Boundaries:

```text
0
64
128
192
```

So `192.168.1.77/26`:

```text
77 ∈ 64–127

Network   = 192.168.1.64
Broadcast = 192.168.1.127
```

---

### 6. If Question Says **H Hosts**

Find smallest `h`:

```text
2^h - 2 ≥ H

Prefix = 32 - h
```

Example: **60 hosts**

```text
2^5 - 2 = 30 ❌
2^6 - 2 = 62 ✅

h = 6
Prefix = /26
```

---

### 7. If Question Says **N Equal Subnets**

Find:

```text
2^n ≥ N
```

Then:

```text
New Prefix = Old Prefix + n
```

Example:

```text
/24 → 4 subnets

2^2 = 4

/24 + 2 = /26
```

---

### 8. FLSM vs VLSM

```text
FLSM → all subnets same size

VLSM → different subnet sizes
```

For VLSM:

```text
1. Sort hosts Largest → Smallest
2. Find smallest 2^h - 2 ≥ Hosts
3. Prefix = 32 - h
4. Allocate block
5. Next subnet = previous Broadcast + 1
```

Example:

```text
100 hosts → /25
50 hosts  → /26
20 hosts  → /27
10 hosts  → /28
```

---

## 🧠 GATE Pattern Recognition

```text
Find Network?
→ IP AND Mask / Block boundary

H Hosts?
→ 2^h - 2 ≥ H

N Equal Subnets?
→ 2^n ≥ N
→ New Prefix = Old Prefix + n

Different Host Requirements?
→ VLSM
→ Largest first

Broadcast?
→ Network + Block Size - 1

Next Subnet?
→ Previous Broadcast + 1
```

### ⚡ The one relationship to remember

```text
Borrow MORE host bits
        ↓
MORE subnets
        ↓
FEWER host bits
        ↓
FEWER hosts/subnet
```

That’s basically the **entire subnetting chapter compressed into a revision sheet**.