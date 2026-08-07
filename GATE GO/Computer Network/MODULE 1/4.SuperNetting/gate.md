Yep. I checked the **actual question sections of `5.Supernetting(1).pdf`**, including the LPM questions, aggregation question, subnetting questions, and the GATE PYQs. I’ll put the **GATE-important patterns first**, then the **question → method → answer** notes so you can keep this as your question-revision sheet.

# 🔥 GATE IMPORTANT — WRITE THIS FIRST

## 1. Longest Prefix Match ⭐⭐⭐

When multiple forwarding-table entries match the destination:

[  
\boxed{\text{Choose the matching prefix with the largest }/n}  
]

Example:

```text
198.15.0.0/16  → 1
198.15.7.0/24  → 7
198.15.7.3/32  → 4
```

Destination `198.15.7.3`:

```text
/16 ✓
/24 ✓
/32 ✓
```

Therefore:

```text
→ /32 → Interface 4
```

For `198.15.7.4`:

```text
/16 ✓
/24 ✓
/32 ✗
```

Therefore:

```text
→ /24 → Interface 7
```

---

## 2. Network ID

[  
\boxed{\text{Network ID} = IP\ AND\ Subnet\ Mask}  
]

Use this when determining whether an IP belongs to a subnet/route.

---

## 3. Host Bits

[  
\boxed{\text{Host bits}=32-\text{prefix}}  
]

[  
\boxed{\text{Total addresses}=2^{host\ bits}}  
]

[  
\boxed{\text{Usable hosts}=2^{host\ bits}-2}  
]

---

## 4. Block Size Shortcut ⭐

If the interesting mask octet is `M`:

[  
\boxed{\text{Block size}=256-M}  
]

Example:

```text
/26
Mask = 255.255.255.192

Block size = 256 - 192
           = 64
```

Therefore:

```text
0–63
64–127
128–191
192–255
```

No need to convert the whole IP into binary.

---

## 5. Subnet Membership

For an IP:

```text
Find its block
↓
Check which range it falls into
↓
That is its subnet
```

---

## 6. Supernetting / Route Aggregation ⭐⭐⭐

```text
Multiple networks
       ↓
Common prefix
       ↓
One larger route
```

Purpose:

> **Reduce forwarding-table entries.**

But aggregation must preserve correct forwarding behavior.

If a broad aggregate and a specific route both match:

[  
\boxed{\text{Longest Prefix Match wins}}  
]

The lecture explicitly connects aggregation with LPM.

---

# 🧠 QUESTION + ANSWER NOTES

---

## Q1 — LPM

Forwarding table:

```text
198.15.0.0/16  → Interface 1
198.15.7.0/24  → Interface 7
198.15.7.3/32  → Interface 4
```

### Destination: `198.15.7.3`

Matches all three.

Longest:

```text
/32
```

### Answer:

[  
\boxed{\text{Interface 4}}  
]

### Destination: `198.15.7.4`

Matches:

```text
/16 ✓
/24 ✓
/32 ✗
```

### Answer:

[  
\boxed{\text{Interface 7}}  
]

---

# Q2 — Forwarding Table

Destination:

```text
201.10.7.17
```

Routes include:

```text
201.10.0.0/21 → 3
201.10.6.0/23 → 2
```

`201.10.7.17` belongs to both ranges.

Compare:

```text
/21
/23 ← longer
```

### Answer:

[  
\boxed{2}  
]

### Pattern

> **Don't stop at the first match. Check whether a more specific prefix also matches.**

---

# Q3 — Binary Prefix Matching

The lecture gives destination addresses in binary and forwarding ranges such as:

```text
11010000 00101111 00010...
11010000 00101111 00011000...
11010000 00101111 00011...
```

The important technique is:

```text
Compare destination with each prefix
↓
Find every match
↓
Longest prefix wins
```

So this is the **same LPM concept**, just expressed directly in binary.

---

# Q4 — LPM Example

Destination:

```text
128.143.71.21
```

Forwarding table contains:

```text
10.0.0.0/8       → R1
128.143.0.0/16   → R2
128.143.64.0/20  → R3
128.143.192.0/20 → R3
128.143.71.0/24  → R4
128.143.71.55/32 → R3
default           → R5
```

Matching routes:

```text
/16 ✓
/20 ✓
/24 ✓
/32 ✗
```

Longest:

```text
/24
```

### Answer:

[  
\boxed{R4}  
]

---

# Q5 — Forwarding Table Aggregation

Given entries:

```text
128.42.222.3     /24 → R1
128.42.128.4     /17 → R2
18.0.0.0         /8  → R4
0.0.0.0          /0  → R4
128.42.127.3     /21 → R1
128.42.216.0     /21 → R1
128.42.128.4     /16 → R3
```

### Important observation

Some entries are **already covered by broader entries with the same next hop**.

For example:

```text
18.0.0.0/8 → R4
default     → R4
```

The default already covers everything, so the `/8` doesn't change forwarding.

Similarly, a broader `/21` route can make a more-specific `/24` entry unnecessary when the next hop is the same.

### Key lesson:

> **Aggregation/simplification can remove redundant entries when forwarding behavior remains unchanged.**

The lecture's final table removes such redundant entries.

---

# Q6 — VLSM + Forwarding Tables

The lecture gives a topology where three subnets with hosts and three subnets without hosts must be allocated from a larger network.

Requirements include roughly:

```text
Subnet A → 250 interfaces
Subnet B → 120 interfaces
Subnet C → 120 interfaces

D, E, F → 2 interfaces each
```

### Thinking

For each subnet:

```text
Required hosts
↓
Find smallest power of 2
↓
Determine prefix
↓
Allocate address range
```

Then construct router forwarding tables using:

[  
\boxed{\text{Longest Prefix Matching}}  
]

The lecture's solution then assigns the corresponding prefixes to A–F and builds each router's forwarding table.

### GATE takeaway

This is a **VLSM + LPM combination question**.

---

# Q7 — Aggregated Address Allocation

A router has a single public-facing interface and three internal subnets:

```text
A → 15 hosts
B → 12 hosts
C → 45 hosts
```

The goal is:

> Assign addresses so that **one aggregate route** can represent all internal networks while minimizing unnecessary advertised addresses.

The lecture emphasizes that the address allocation itself should be designed to create a useful common prefix.

### Pattern

When asked to **minimize advertised address range**:

```text
Don't just allocate randomly.
↓
Place subnets so their prefixes have
the largest possible common prefix.
```

---

# Q8 — GATE CSE 2004

Routing table:

```text
128.75.43.0   255.255.255.0   Eth0
128.75.43.0   255.255.128.0   Eth1
192.12.17.0   255.255.255.255 Eth3
Default                         Eth2
```

Destinations:

```text
128.75.43.16
192.12.17.10
```

### Destination 1

`128.75.43.16` matches both:

```text
/24
/17
```

Longest:

```text
/24 → Eth0
```

### Destination 2

`192.12.17.10` does **not** match:

```text
192.12.17.0/32
```

Therefore:

```text
→ Default → Eth2
```

### Answer:

[  
\boxed{\text{Eth0 and Eth2}}  
]

---

# Q9 — GATE CSE 2014

Destination:

```text
131.23.151.76
```

Routes:

```text
131.16.0.0/12 → 3
131.28.0.0/14 → 5
131.19.0.0/16 → 2
131.22.0.0/15 → 1
```

Check which prefixes contain the destination.

The longest matching prefix is:

```text
131.22.0.0/15
```

### Answer:

[  
\boxed{1}  
]

### GATE pattern

> **Convert only the relevant octet/range. Don't blindly convert all 32 bits.**

---

# Q10 — GATE CSE 2015

Forwarding table:

```text
128.96.170.0/23 → Interface 0
128.96.168.0/23 → Interface 1
128.96.166.0/23 → R2
128.96.160.0/22 → R3
default          → R4
```

Destinations:

### `128.96.171.92`

Falls in:

```text
170–171
```

### Answer:

[  
\boxed{\text{Interface 0}}  
]

### `128.96.167.151`

Falls in:

```text
166–167
```

### Answer:

[  
\boxed{R2}  
]

### `128.96.163.151`

Falls in:

```text
160–163
```

### Answer:

[  
\boxed{R3}  
]

### `128.96.169.192`

Falls in:

```text
168–169
```

### Answer:

[  
\boxed{\text{Interface 1}}  
]

### `128.96.165.121`

Doesn't match any specific prefix.

### Answer:

[  
\boxed{R4\text{ (default)}}  
]

---

# Q11 — GATE IT 2006

Destination:

```text
144.16.68.117
```

Relevant routes:

```text
144.16.68.0/24   → Eth2
144.16.68.64/27  → Eth3
```

Does `.117` belong to:

```text
68.64 – 68.95
```

No.

So `/27` doesn't match.

But:

```text
144.16.68.0/24
```

does.

### Answer:

[  
\boxed{\text{Eth2}}  
]

---

# Q12 — 8-bit LPM

Forwarding prefixes:

```text
00  → interface 0
010 → interface 1
01  → interface 2
1   → interface 3
10  → interface 4
```

Because of LPM, overlapping prefixes are resolved by the longer one.

Ranges:

|Interface|Range|
|---|---|
|0|`00xxxxxx` → `0x00–0x3F`|
|1|`010xxxxx` → `0x40–0x5F`|
|2|`011xxxxx` → `0x60–0x7F`|
|4|`10xxxxxx` → `0x80–0xBF`|
|3|`11xxxxxx` → `0xC0–0xFF`|

### Example destinations

```text
01000010 → interface 1
11110010 → interface 3
01111000 → interface 2
```

### Key idea

A prefix like:

```text
01
```

doesn't simply own every `01...` address.

If:

```text
010
```

exists, it gets those addresses because:

[  
3>2  
]

**Longest prefix wins.**

---

# Q13 — Equal Prefix Ranges

Forwarding prefixes:

```text
00 → 0
01 → 1
10 → 2
11 → 3
```

Each prefix has 2 fixed bits.

Remaining:

[  
8-2=6  
]

host bits.

Therefore:

[  
2^6=64  
]

addresses each.

### Ranges

```text
00 → 00000000–00111111
   → 0x00–0x3F

01 → 0x40–0x7F

10 → 0x80–0xBF

11 → 0xC0–0xFF
```

### Answer

```text
Interface 0 → 64 addresses
Interface 1 → 64
Interface 2 → 64
Interface 3 → 64
```

---

# Q14 — Specific Prefix Overrides Broad Prefix

Routing table:

```text
142.150.64.0/20   → A
142.150.71.128/28 → B
142.150.71.128/30 → D
142.150.0.0/16    → C
```

Destination:

```text
142.150.71.132
```

Matches:

```text
/16 ✓
/20 ✓
/28 ✓
/30 ✗
```

Longest:

```text
/28
```

### Answer:

[  
\boxed{B}  
]

---

## Follow-up variants

### b) Force exactly `142.150.71.132` to A

Add:

```text
142.150.71.132/32 → A
```

Why?

Because `/32` is more specific than `/28`.

[  
\boxed{/32 > /28}  
]

### c) Send everything else to C

Add:

```text
0.0.0.0/0 → C
```

That's the **default route**.

---

# Q15 — GATE CSE 2019

IPs:

```text
M = 100.10.5.2
N = 100.10.5.5
P = 100.10.5.6
```

Mask:

```text
255.255.255.252
```

That's:

```text
/30
```

Block size:

[  
256-252=4  
]

Ranges:

```text
0–3
4–7
```

Therefore:

```text
M = .2 → subnet .0–.3

N = .5 → subnet .4–.7

P = .6 → subnet .4–.7
```

### Answer:

[  
\boxed{\text{Only N and P belong to the same subnet}}  
]

---

# Q16 — GATE CSE 2006

```text
C1 = 203.197.2.53
Mask = 255.255.128.0
```

So:

```text
/17
```

For C2:

```text
C2 = 203.197.75.201
Mask = 255.255.192.0
```

So:

```text
/18
```

### Under C1's mask

Both fall in:

```text
203.197.0.0/17
```

So C1 thinks:

```text
same network
```

### Under C2's mask

C1:

```text
203.197.0.0/18
```

C2:

```text
203.197.64.0/18
```

So C2 thinks:

```text
different networks
```

### Answer:

[  
\boxed{\text{C — C1 assumes same, C2 assumes different}}  
]

---

# Q17 — GATE IT 2008: Gateway

Host X:

```text
192.168.1.97
```

Mask:

```text
255.255.255.224
```

Therefore:

```text
/27
```

Block size:

[  
256-224=32  
]

Ranges:

```text
0–31
32–63
64–95
96–127 ← X is here
128–159
...
```

So X belongs to:

```text
192.168.1.96/27
```

Possible gateway:

```text
R1 = .135
R1 = .110
R2 = .67
R2 = .155
```

Only:

```text
.110
```

falls inside:

```text
96–127
```

### Answer:

[  
\boxed{192.168.1.110}  
]

---

# Q18 — GATE IT 2006: Broadcast Address

Given broadcast:

```text
144.16.95.255
```

Possible masks:

```text
255.255.224.0
255.255.240.0
255.255.248.0
```

All can produce a subnet whose broadcast address ends at:

```text
144.16.95.255
```

Therefore:

[  
\boxed{\text{All of the above}}  
]

Answer:

[  
\boxed{D}  
]

### Important pattern

A broadcast address doesn't uniquely determine one subnet mask here.

**Multiple subnet masks can produce the same broadcast address.**

---

# Q19 — GATE IT 2008: Number of Distinct Subnets

Given:

```text
X  = 192.168.1.97
R1 = 192.168.1.135
R1 = 192.168.1.110
R2 = 192.168.1.67
R2 = 192.168.1.155
```

Mask:

```text
255.255.255.224 = /27
```

Block size:

```text
32
```

Network IDs:

```text
.97  → .96
.135 → .128
.110 → .96
.67  → .64
.155 → .128
```

Unique networks:

```text
.64
.96
.128
```

Therefore:

[  
\boxed{3}  
]

### Answer:

[  
\boxed{C}  
]

---

# 🧠 THE PATTERNS YOU SHOULD RECOGNIZE INSTANTLY

This is the **real GATE value** of all those questions.

### Pattern 1 — "Which interface?"

Think:

```text
Destination IP
↓
Find ALL matching prefixes
↓
Longest /n
↓
Interface
```

---

### Pattern 2 — "Does IP belong to subnet?"

Think:

```text
IP AND MASK
      ↓
Network ID
```

or use the **block-size shortcut**.

---

### Pattern 3 — "How many hosts?"

```text
Host bits = 32 - prefix

Addresses = 2^h

Usable = 2^h - 2
```

---

### Pattern 4 — "Which subnet?"

```text
Block size = 256 - mask value
↓
Find range containing IP
```

---

### Pattern 5 — "Gateway?"

```text
Find X's subnet
↓
Check which router IP lies in SAME subnet
```

---

### Pattern 6 — "How many different subnets?"

Calculate **network ID for every IP** and count unique network IDs.

---

### Pattern 7 — "Can I aggregate?"

Think:

```text
Same forwarding direction?
        ↓
Common prefix?
        ↓
Can represent with broader prefix?
        ↓
Check extra addresses
        ↓
LPM handles specific exceptions
```

---

### Pattern 8 — "Broadcast given, find mask?"

Don't assume one answer.

Ask:

> **Which masks can produce this broadcast?**

The GATE 2006 question is specifically designed to catch that mistake.

---

# ⭐ FINAL GATE CHEAT SHEET

Write this at the **very end of your question notes**:

```text
IPv4 = 32 bits

Host bits = 32 - /n

Addresses = 2^(host bits)

Usable hosts = 2^(host bits) - 2

Network ID = IP AND Mask

Block size = 256 - interesting mask octet

Network range:
Network → Broadcast

Broadcast = last address of subnet

LPM:
Multiple matches → largest /n wins

Default route:
0.0.0.0/0

Supernetting:
Multiple smaller routes → one broader route

Aggregation:
Same forwarding direction + common prefix

If aggregate overlaps a specific route:
→ Longest Prefix Match handles it
```

### Most important exam hierarchy:

[  
\boxed{  
\text{LPM}

\text{Network ID}

\text{Block Size}

\text{Subnet Range}

\text{Aggregation}  
}  
]

And one **very important exam habit** from these questions:

> **Don't calculate everything. First identify what the question is actually asking.**

If it asks **only subnet mask → stop at mask.**  
If it asks **interface → focus on matching + LPM.**  
If it asks **same subnet → calculate network IDs/ranges.**  
If it asks **number of subnets → count borrowed bits.**

The PDF's question section is heavily built around exactly these patterns.