Absolutely. I went through the **87-page `6.questions(2).pdf`** and organized the actual questions + answers into an **Obsidian-ready Q&A note**. The deck is mainly **Subnetting, CIDR, VLSM, Supernetting, Routing Tables, and Longest Prefix Matching**.

I’ve kept the solutions aligned with the PPT’s approach rather than replacing them with unrelated theory.

# Computer Networks — Subnetting & Routing — Full Q&A

> [!abstract] What this PPT covers
> 
> - Classful addressing
>     
> - Subnetting
>     
> - CIDR
>     
> - VLSM
>     
> - Network ID / Subnet ID
>     
> - Subnet mask
>     
> - Host calculation
>     
> - Longest Prefix Matching
>     
> - Routing tables
>     
> - Supernetting / Route Aggregation
>     
> - GATE PYQs
>     
> - Tricky subnet questions
>     

---

## 1. IPv4 Address `18.26.0.127`

### Question

Consider IPv4 address **18.26.0.127**.

1. If class-based addressing is used, what type of network is it?
    
2. Divide the network into **32 subnets**. What is the subnet mask?
    
3. What is the subnet number/address to which this IP belongs?
    
4. If CIDR is used instead, what is the prefix length?
    

### Answer

#### (a) Network class

First octet = `18`

Class A range:

```text
1 – 126
```

Therefore:

```text
18.26.0.127 → Class A
```

**Answer: Class A**

#### (b) Divide into 32 subnets

Class A default:

```text
Network bits = 8
Host bits = 24
```

Need:

```text
32 = 2^5
```

So borrow **5 host bits**.

```text
Original prefix = /8
Borrowed bits   = 5

New prefix = /13
```

Mask:

```text
11111111.11111000.00000000.00000000

= 255.248.0.0
```

**Answer: `255.248.0.0` or `/13`**

#### (c) Subnet number

The PPT identifies the subnet containing `18.26.0.127` as:

```text
18.24.0.0
```

**Answer: `18.24.0.0`**

#### (d) CIDR prefix

Physical Class-A network has:

```text
8 network bits
```

The subnet uses:

```text
5 additional bits
```

Therefore:

```text
8 + 5 = /13
```

**Answer: `/13`**

The subnet-bit construction and resulting subnet are shown across pages 2–6.

---

# 2. VLSM Example — 10, 60 and 120 Addresses

### Question

An organization receives:

```text
14.24.74.0/24
```

It requires three subnet blocks:

```text
A → 10 addresses
B → 60 addresses
C → 120 addresses
```

Design the subnets.

### Answer

Always allocate the **largest requirement first**.

```text
120 → 60 → 10
```

### Step 1 — 120 addresses

Need:

```text
2^7 = 128 addresses
```

Therefore:

```text
Host bits = 7
Prefix = 32 - 7 = /25
```

So:

```text
14.24.74.0/25
```

Range:

```text
14.24.74.0 – 14.24.74.127
```

### Step 2 — 60 addresses

Need:

```text
2^6 = 64 addresses
```

So:

```text
/26
```

Next available block:

```text
14.24.74.128/26
```

Range:

```text
14.24.74.128 – 14.24.74.191
```

### Step 3 — 10 addresses

Need:

```text
2^4 = 16 addresses
```

So:

```text
/28
```

Next available:

```text
14.24.74.192/28
```

Range:

```text
14.24.74.192 – 14.24.74.207
```

### Final

|Requirement|Prefix|Block|
|---|--:|---|
|120|`/25`|`14.24.74.0/25`|
|60|`/26`|`14.24.74.128/26`|
|10|`/28`|`14.24.74.192/28`|

> [!tip] Pattern  
> **VLSM → allocate largest subnet first.**
> 
> Otherwise, you can fragment the address space and make the larger subnet impossible to allocate later.

The PPT's worked example begins on page 7 and builds the subnet hierarchy on pages 8–9.

---

# 3. Divide `172.16.0.0/16` into 7 Networks

### Question

Divide:

```text
172.16.0.0/16
```

into seven networks requiring:

```text
Net1 → 500 hosts
Net2 → 200 hosts
Net3 → 100 hosts
Net4 → 60 hosts
Net5 → 20 hosts
Net6 → 2 hosts
Net7 → 2 hosts
```

### Answer

This is a **VLSM** problem.

Sort largest → smallest:

```text
500
200
100
60
20
2
2
```

### Net1 — 500 hosts

Need:

```text
2^9 = 512 addresses
```

Therefore:

```text
/23
```

### Net2 — 200 hosts

Need:

```text
2^8 = 256
```

Therefore:

```text
/24
```

### Net3 — 100 hosts

Need:

```text
2^7 = 128
```

Therefore:

```text
/25
```

### Net4 — 60 hosts

Need:

```text
2^6 = 64
```

Therefore:

```text
/26
```

### Net5 — 20 hosts

Need:

```text
2^5 = 32
```

Therefore:

```text
/27
```

### Net6 and Net7 — 2 hosts each

For 2 usable hosts:

```text
2^2 = 4 addresses
```

Therefore:

```text
/30
```

> [!important]  
> For ordinary IPv4 subnetting:
> 
> ```text
> usable hosts = 2^h - 2
> ```
> 
> because network ID and broadcast address are reserved.

The PPT works through this allocation tree on pages 10–11.

---

# 4. GATE CSE 2023 — Forwarding Table

### Question

A router has:

|Subnet|Mask|Interface|
|---|---|--:|
|`200.150.0.0`|`255.255.0.0`|1|
|`200.150.64.0`|`255.255.224.0`|2|
|`200.150.68.0`|`255.255.255.0`|3|
|`200.150.68.64`|`255.255.255.224`|4|

Destination:

```text
200.150.68.118
```

Which interface?

### Answer

Check the most specific prefixes.

`200.150.68.118` belongs to:

```text
200.150.68.0/24
```

It also matches:

```text
200.150.64.0/19
```

But does it belong to:

```text
200.150.68.64/27 ?
```

`/27` ranges:

```text
64 – 95
96 – 127
```

118 lies in:

```text
96 – 127
```

So it **does not belong to the subnet starting at .64**.

The longest matching valid prefix is:

```text
200.150.68.0/24
```

Therefore:

**Answer: Interface 3**

The PPT explicitly shows the `/24` entry being selected after checking the `/27` entry.

---

# 5. Subnet Mask `255.255.31.0`

### Question

The subnet mask is:

```text
255.255.31.0
```

Which pair of IP addresses can belong to the same network?

### Core idea

Convert the important part to binary.

```text
31 = 00011111
```

The mask determines which bits must be identical.

For every candidate:

```text
IP AND subnet mask
```

If the resulting network IDs are equal → same subnet.

The PPT checks the candidates using bitwise AND and concludes the valid pair is **option D**.

> [!important] GATE pattern  
> When asked:
> 
> **"Which IP addresses can belong to this network?"**
> 
> Don't compare the IPs directly.
> 
> Do:
> 
> ```text
> IP1 AND MASK
> IP2 AND MASK
> ```
> 
> Same result → same subnet.

---

# 6. GATE CSE 2005 — 64 Departments

### Question

An organization has a **Class B** network and wants to create subnets for **64 departments**.

What subnet mask is required?

### Answer

Need:

```text
64 = 2^6
```

So borrow:

```text
6 bits
```

Class B default:

```text
/16
```

New prefix:

```text
16 + 6 = /22
```

Mask:

```text
255.255.252.0
```

**Answer: `255.255.252.0`**

The PPT explicitly notes the 6 borrowed bits and answer `255.255.252.0`.

---

# 7. GATE CSE 2006 — Two Computers and Different Masks

### Question

```text
C1 = 203.197.2.53
Mask = 255.255.128.0

C2 = 203.197.75.201
Mask = 255.255.192.0
```

Determine whether the computers assume they are in the same network.

### Answer

For C1:

```text
203.197.2.53
AND 255.255.128.0
-----------------
203.197.0.0
```

For C2:

```text
203.197.75.201
AND 255.255.192.0
-----------------
203.197.64.0
```

The network IDs differ.

But the question is about **what each computer assumes**.

### C1's view

Using `/17`:

```text
203.197.0.0/17
```

C2's IP:

```text
203.197.75.201
```

lies inside:

```text
203.197.0.0 – 203.197.127.255
```

So C1 thinks:

```text
C2 is local
```

### C2's view

C2 uses `/18`:

```text
203.197.64.0/18
```

C1:

```text
203.197.2.53
```

is outside that range.

So C2 thinks:

```text
C1 is remote
```

### Answer

**C1 assumes C2 is on the same network, while C2 assumes C1 is on a different network.**

The PPT illustrates exactly this asymmetric view on pages 20–25.

> [!important]  
> This is a beautiful GATE concept:
> 
> **"Same network" can depend on which host's subnet mask you use.**

---

# 8. GATE CSE 2007 — Class B Split into Subnets

### Question

A Class B network is split into subnets with a **6-bit subnet number**.

Find:

1. Maximum number of subnets
    
2. Maximum hosts per subnet
    

### Answer

Class B:

```text
Network = 16 bits
Host = 16 bits
```

Borrow:

```text
6 bits
```

### Number of subnets

```text
2^6 = 64
```

### Remaining host bits

```text
16 - 6 = 10
```

Therefore:

```text
2^10 = 1024 total addresses
```

Usable hosts:

```text
1024 - 2
= 1022
```

### Answer

```text
64 subnets
1022 hosts/subnet
```

The question and explanation appear on pages 26–27.

---

# 9. GATE CSE 2008 — `255.255.248.0`

### Question

A Class B network has subnet mask:

```text
255.255.248.0
```

What is the maximum number of hosts per subnet?

### Answer

Convert:

```text
248 = 11111000
```

So:

```text
255.255.248.0
= /21
```

Host bits:

```text
32 - 21 = 11
```

Total addresses:

```text
2^11 = 2048
```

Usable hosts:

```text
2048 - 2
= 2046
```

### Answer

**2046 hosts**

The PPT reaches `2^11 - 2 = 2046`.

---

# 10. GATE CSE 2010 — Find the Mask

### Question

Two computers:

```text
A = 10.105.1.113
B = 10.105.1.91
```

Both use the same subnet mask.

Which mask **should NOT** be used if A and B must belong to the same network?

Options include:

```text
255.255.255.0
255.255.255.128
255.255.255.192
255.255.255.224
```

### Answer

Test `/27`:

```text
255.255.255.224
```

Subnet ranges are blocks of 32:

```text
0–31
32–63
64–95
96–127
128–159
...
```

A:

```text
113 → 96–127
```

B:

```text
91 → 64–95
```

Different subnet.

Therefore:

**Answer: `255.255.255.224`**

The PPT's AND-based explanation confirms this.

---

# 11. GATE CSE 2012 — CIDR Allocation

### Question

ISP has:

```text
245.248.128.0/20
```

It wants to allocate:

```text
A → half
B → quarter
```

while retaining the rest.

### Answer

Original `/20` contains:

```text
2^(32-20)
= 4096 addresses
```

### A gets half

Half of 4096:

```text
2048 addresses
```

Prefix:

```text
/21
```

So:

```text
A = 245.248.136.0/21
```

### B gets quarter

Quarter:

```text
1024 addresses
```

Prefix:

```text
/22
```

So:

```text
B = 245.248.128.0/22
```

The PPT's answer is **Option A**, with A using `/21` and B `/22`.

---

# 12. Supernetting — `205.16.32.0/21`

### Question

A supernet has:

```text
First address = 205.16.32.0
Mask = 255.255.248.0
```

How many blocks are in this supernet, and what is the address range of each block?

### Answer

Mask:

```text
255.255.248.0
```

Since:

```text
248 = 11111000
```

Prefix:

```text
/21
```

Compared with a `/24` Class C block, this combines:

```text
24 - 21 = 3 bits
```

Therefore:

```text
2^3 = 8 blocks
```

Each block contains:

```text
256 addresses
```

Ranges:

```text
205.16.32.0  – 205.16.32.255
205.16.33.0  – 205.16.33.255
205.16.34.0  – 205.16.34.255
205.16.35.0  – 205.16.35.255
205.16.36.0  – 205.16.36.255
205.16.37.0  – 205.16.37.255
205.16.38.0  – 205.16.38.255
205.16.39.0  – 205.16.39.255
```

**Answer: 8 blocks**

The PPT explicitly lists all eight ranges.

---

# 13. Forwarding Table — `201.10.7.17`

### Question

Destination:

```text
201.10.7.17
```

Forwarding table:

|Prefix|Outgoing link|
|---|--:|
|`192.0.0.0/4`|2|
|`4.83.128.0/17`|1|
|`201.10.0.0/21`|3|
|`201.10.6.0/23`|2|
|`126.255.103.0/24`|3|

Find outgoing link.

### Answer

Check:

```text
201.10.0.0/21
```

This covers:

```text
201.10.0.0 – 201.10.7.255
```

Therefore:

```text
201.10.7.17
```

matches it.

So:

```text
Outgoing link = 3
```

**Answer: 3**

The slide highlights the matching route and interface.

---

# 14. Longest Prefix Matching — `128.143.71.21`

### Question

Routing table contains:

```text
10.0.0.0/8          → R1
128.143.0.0/16      → R2
128.143.64.0/20     → R3
128.143.192.0/20    → R3
128.143.71.0/24     → R4
128.143.71.55/32    → R3
default              → R5
```

Destination:

```text
128.143.71.21
```

### Answer

Several entries can match.

But routing uses:

> **Longest Prefix Match**

Compare:

```text
/16
/20
/24
/32
```

Does `/32` match?

```text
128.143.71.55
```

No, because destination is `.21`.

Does:

```text
128.143.71.0/24
```

match?

Yes.

Therefore:

```text
Next hop = R4
```

**Answer: R4**

The PPT explicitly identifies `128.143.71.0/24` as the longest matching prefix.

---

# 15. Address Allocation for 15, 12 and 45 Hosts

### Question

A router has three subnets:

```text
A → 15 hosts
B → 12 hosts
C → 45 hosts
```

Assign addresses such that the three subnet ranges can be aggregated into a single advertised route.

### Answer

Largest requirement:

```text
45 hosts
```

Need:

```text
2^6 = 64 addresses
```

So:

```text
/26
```

For 15 and 12 hosts:

```text
2^5 = 32 addresses
```

so each requires:

```text
/27
```

The three blocks can be arranged contiguously so that their combined address space can be represented by a larger aggregate prefix.

The PPT's construction starts from the largest subnet and uses contiguous blocks specifically to minimize the aggregate advertised range.

> [!tip]  
> **Supernetting wants contiguous blocks.**
> 
> If the networks are scattered around the address space, aggregation becomes difficult or impossible.

---

# 16. GATE CSE 2004 — Two Destinations

### Question

A router has:

|Destination|Mask|Interface|
|---|---|---|
|`128.75.43.0`|`255.255.255.0`|Eth0|
|`128.75.43.60`|`255.255.255.128`|Eth1|
|`192.12.17.0`|`255.255.255.255`|Eth3|
|Default|—|Eth2|

Find interfaces for:

```text
128.75.43.16
192.12.17.10
```

### Answer

#### Destination 1

```text
128.75.43.16
```

Matches:

```text
128.75.43.0/24
```

and the `/25` route beginning at `.60` does not match.

Therefore:

```text
Eth0
```

#### Destination 2

```text
192.12.17.10
```

Doesn't match the specific `/32` entry:

```text
192.12.17.0/32
```

Therefore default route:

```text
Eth2
```

### Answer

```text
128.75.43.16 → Eth0
192.12.17.10 → Eth2
```

The PPT's solution uses ANDing and then the default route for the second packet.

---

# 17. GATE CSE 2014 — CIDR Routing

### Question

Destination:

```text
131.23.151.76
```

Routing table:

```text
131.16.0.0/12 → 3
131.28.0.0/14 → 5
131.19.0.0/16 → 2
131.22.0.0/15 → 1
```

Find output interface.

### Answer

Check each prefix.

```text
131.16.0.0/12
```

matches.

```text
131.28.0.0/14
```

doesn't.

```text
131.19.0.0/16
```

doesn't.

```text
131.22.0.0/15
```

matches.

Two matches:

```text
/12 → interface 3
/15 → interface 1
```

Longest prefix:

```text
/15
```

Therefore:

**Answer: Interface 1**

The PPT explicitly compares the matching prefixes and chooses `/15`.

---

# 18. GATE CSE 2015 — Routing Table

The PPT evaluates several destination addresses against:

```text
128.96.170.0/?
128.96.160.0/?
128.96.162.0/?
128.96.164.0/?
```

using subnet masks and bitwise AND.

The final mapping given by the solution is:

```text
128.96.171.92  → Interface 0
128.96.167.151 → R2
128.96.163.151 → R4
128.96.164.121 → R3
```

The important method is:

```text
Destination
     ↓
AND with mask
     ↓
Subnet ID
     ↓
Compare routing entries
     ↓
Longest matching prefix
```

The worked AND operations appear across pages 49–51.

---

# 19. GATE IT 2006 — `144.16.68.117`

### Question

Routing table:

|Destination|Mask|Interface|
|---|---|---|
|`144.16.0.0`|`255.255.0.0`|eth0|
|`144.16.64.0`|`255.255.192.0`|eth1|
|`144.16.68.0`|`255.255.255.0`|eth2|
|`144.16.68.64`|`255.255.255.224`|eth3|

Destination:

```text
144.16.68.117
```

### Answer

Try the most specific route:

```text
144.16.68.64/27
```

A `/27` block:

```text
64 – 95
```

But:

```text
117
```

is not inside it.

Try:

```text
144.16.68.0/24
```

117 is inside:

```text
0 – 255
```

Therefore:

```text
eth2
```

**Answer: eth2**

The PPT explicitly demonstrates checking the longest mask first and falling back to `/24`.

---

# 20. 8-bit Forwarding Table — Host Count

### Question

Using 8-bit addresses:

|Prefix|Interface|
|---|--:|
|`00`|0|
|`010`|1|
|`01`|2|
|`1`|3|
|`10`|4|

Determine number of hosts reachable through each interface.

### Answer

Remember:

```text
Number of addresses = 2^(remaining bits)
```

### Interface 0

Prefix:

```text
00
```

Remaining:

```text
8 - 2 = 6
```

Therefore:

```text
2^6 = 64
```

### Interface 1

```text
010
```

Remaining:

```text
8 - 3 = 5
```

```text
2^5 = 32
```

### Interface 2

```text
01
```

Remaining:

```text
6 bits
```

```text
64
```

### Interface 3

```text
1
```

Remaining:

```text
7 bits
```

```text
128
```

### Interface 4

```text
10
```

Remaining:

```text
6 bits
```

```text
64
```

### Answer

|Interface|Prefix|Address count|
|--:|---|--:|
|0|`00`|64|
|1|`010`|32|
|2|`01`|64|
|3|`1`|128|
|4|`10`|64|

The PPT then emphasizes that **only the destination address matters** when forwarding.

---

# 21. 8-bit Forwarding — Find Interface

Using the same table:

```text
00  → 0
010 → 1
01  → 2
1   → 3
10  → 4
```

### Destination 1

```text
0100010
```

Matches:

```text
010
01
```

Longest:

```text
010
```

Therefore:

```text
Interface 1
```

### Destination 2

```text
1111010
```

Matches:

```text
1
```

Not `10`.

Therefore:

```text
Interface 3
```

### Destination 3

```text
0111100
```

Matches:

```text
01
```

Therefore:

```text
Interface 2
```

### Answer

```text
1 → Interface 1
2 → Interface 3
3 → Interface 2
```

This is the same **longest-prefix-match pattern** used throughout the routing questions.

---

# 22. 8-bit Prefix Range Question

### Question

Routing table:

```text
00 → Interface 0
01 → Interface 1
10 → Interface 2
11 → Interface 3
```

Find the address range associated with each interface.

### Answer

Each prefix contains 2 fixed bits.

Therefore:

```text
Remaining bits = 8 - 2 = 6
```

Each prefix represents:

```text
2^6 = 64 addresses
```

Ranges:

|Interface|Prefix|Range|Addresses|
|--:|---|---|--:|
|0|`00`|`0x00 – 0x3F`|64|
|1|`01`|`0x40 – 0x7F`|64|
|2|`10`|`0x80 – 0xBF`|64|
|3|`11`|`0xC0 – 0xFF`|64|

The PPT gives exactly these ranges.

---

# 23. Routing Table — `142.150.71.132`

### Question

Routing table:

```text
142.150.64.0/20  → A
142.150.71.128/28 → B
142.150.71.128/30 → D
142.150.0.0/16 → C
```

Destination:

```text
142.150.71.132
```

Find next hop.

### Answer

Destination matches:

```text
142.150.0.0/16
142.150.64.0/20
142.150.71.128/28
```

Check `/30`:

```text
142.150.71.128/30
```

range:

```text
128 – 131
```

Destination:

```text
132
```

doesn't match.

So the longest valid prefix is:

```text
/28
```

Therefore:

**Answer: B**

The PPT marks B and explains the longest-prefix reasoning.

---

# 24. Modify Routing Table

Same table.

### (b) Question

Add an entry so that:

```text
142.150.71.132
```

goes to next hop:

```text
A
```

while all other destinations remain unchanged.

### Answer

We need a prefix matching only that address.

Use:

```text
142.150.71.132/32 → A
```

Because `/32` matches exactly one IPv4 address.

### (c) Question

Force all addresses that don't match existing entries to go to:

```text
C
```

### Answer

Use default route:

```text
0.0.0.0/0 → C
```

The PPT explicitly gives `/32` for part (b) and `0.0.0.0/0` for part (c).

> [!important]  
> Think of:
> 
> ```text
> /32 → one exact IP
> /0  → everything
> ```

---

# 25. Routing Table — Five Destination Addresses

The PPT gives:

```text
128.96.170.0
128.96.168.0
128.96.166.0
128.96.164.0
default
```

with different masks and next hops.

For each destination, perform:

```text
Destination AND mask
```

Then compare the result with the subnet number.

The PPT's worked answers demonstrate:

```text
128.96.171.92 → Interface 0
128.96.167.151 → R2
128.96.163.151 → default R4
128.96.169.192 → Interface 1
128.96.165.121 → R3
```

The key idea is **try the longest mask first** and move toward shorter masks only when a match fails.

---

# 26. GATE CSE 2022 — Route Aggregation

### Question

Routing table:

```text
12.20.164.0/22 → R1
12.20.170.0/23 → R2
12.20.168.0/23 → Interface 0
12.20.166.0/23 → Interface 1
default → R3
```

Find the CIDR prefix that can collectively aggregate the relevant subnet routes.

### Answer

The relevant ranges are contiguous around:

```text
12.20.164.0
through
12.20.171.255
```

This covers:

```text
8 × 256 = 2048 addresses
```

Therefore:

```text
2048 = 2^11
```

Prefix:

```text
32 - 11 = /21
```

The aggregate is:

```text
12.20.164.0/21
```

The PPT discusses why extra IPs may be included when choosing the aggregate prefix.

> [!important]  
> Route aggregation doesn't require the aggregate to contain **only** the original networks.
> 
> It may cover some additional addresses as long as the routing behavior remains correct.

---

# 27. CIDR Forwarding Table

### Question

Routing table:

|Prefix|Next Hop|
|---|---|
|`196.94.2.0/24`|A|
|`196.94.2.128/25`|B|
|`196.94.0.0/16`|C|
|`196.94.64.0/18`|D|
|`196.76.0.0/14`|E|
|`140.0.0.0/8`|F|
|`128.0.0.0/2`|G|
|`0.0.0.0/1`|H|

Find next hop for:

```text
(a) 139.1.1.1
(b) 196.94.2.100
(c) 196.94.2.200
(d) 196.94.3.100
```

### (a) `139.1.1.1`

Matches the broad:

```text
128.0.0.0/2
```

Therefore:

```text
G
```

### (b) `196.94.2.100`

Matches:

```text
196.94.0.0/16
196.94.2.0/24
196.94.2.128/25
```

But `.100` is not inside:

```text
128–255
```

So `/25` doesn't match.

Longest valid prefix:

```text
/24
```

Therefore:

```text
A
```

### (c) `196.94.2.200`

Matches:

```text
/16
/24
/25
```

`.200` belongs to:

```text
128–255
```

So `/25` matches.

Therefore:

```text
B
```

### (d) `196.94.3.100`

Matches:

```text
196.94.0.0/16
```

but not:

```text
196.94.2.0/24
```

Therefore:

```text
C
```

### Final

```text
(a) G
(b) A
(c) B
(d) C
```

The PPT's answer page explicitly gives these results.

---

# 28. GATE CSE 2019 — Three Machines

### Question

Machines:

```text
M = 100.10.5.2
N = 100.10.5.5
P = 100.10.5.6
```

Subnet mask:

```text
255.255.255.252
```

Which machines belong to the same subnet?

### Answer

Mask:

```text
255.255.255.252
```

Last octet:

```text
252 = 11111100
```

So:

```text
/30
```

Block size:

```text
256 - 252 = 4
```

Subnet ranges:

```text
0–3
4–7
8–11
...
```

All three:

```text
2
5
6
```

Wait:

```text
2 → subnet 0–3
5 → subnet 4–7
6 → subnet 4–7
```

Therefore:

```text
N and P
```

are in the same subnet.

### Answer

**Only N and P belong to the same subnet.**

The PPT confirms this after performing the AND operation.

---

# 29. GATE IT 2008 — Default Gateway

### Question

Host X:

```text
192.168.1.97
```

Host Y:

```text
192.168.1.80
```

Routers:

```text
R1:
192.168.1.135
192.168.1.110

R2:
192.168.1.67
192.168.1.155
```

Subnet mask:

```text
255.255.255.224
```

Which IP should X configure as its gateway?

### Answer

Mask:

```text
255.255.255.224
```

means:

```text
/27
```

Block size:

```text
256 - 224 = 32
```

Subnets:

```text
0–31
32–63
64–95
96–127
128–159
...
```

X:

```text
192.168.1.97
```

belongs to:

```text
192.168.1.96/27
```

Gateway must be in **the same subnet**.

R1:

```text
192.168.1.110
```

belongs to:

```text
96–127
```

So it is valid.

R1's:

```text
192.168.1.135
```

belongs to another subnet.

Therefore:

**Answer: `192.168.1.110`**

The PPT explicitly identifies option B and explains that X and the gateway must have the same subnet number.

---

# 30. GATE IT 2006 — Given Broadcast Address

### Question

A subnetted Class B network has broadcast address:

```text
144.16.90.255
```

Which subnet mask is possible?

Options:

```text
255.255.224.0
255.255.240.0
255.255.248.0
Any of the above
```

### Answer

Broadcast address has:

```text
host bits = 1
```

For each possible mask:

```text
255.255.224.0
```

leaves 5 bits in the third octet as host bits.

```text
255.255.240.0
```

leaves 4 host bits.

```text
255.255.248.0
```

leaves 3 host bits.

All can produce a broadcast address ending in:

```text
...90.255
```

Therefore:

**Answer: Any of the above**

The PPT's reasoning is that the broadcast address only requires the host portion to be all ones.

---

# 31. GATE IT 2008 — How Many Existing Subnets?

### Question

Host X:

```text
192.168.1.97
```

Host Y:

```text
192.168.1.80
```

Router interfaces:

```text
R1:
192.168.1.135
192.168.1.110

R2:
192.168.1.67
192.168.1.155
```

Mask:

```text
255.255.255.224
```

How many distinct subnets are guaranteed to already exist?

### Answer

Again:

```text
/27
```

Block size:

```text
32
```

Look at the last octets:

```text
97
80
135
110
67
155
```

Map them:

```text
67  → 64–95
80  → 64–95
97  → 96–127
110 → 96–127
135 → 128–159
155 → 128–159
```

Therefore three subnet IDs exist:

```text
192.168.1.64
192.168.1.96
192.168.1.128
```

### Answer

**3 subnets**

The PPT explicitly gives option C and lists these three subnet IDs.

---

# 32. GATE CSE 2022 — Number of Subnets in Enterprise

### Question

An enterprise network has:

- Two Ethernet segments
    
- A web server
    
- A firewall
    
- Three routers
    

The diagram asks:

> What is the number of subnets inside the enterprise network?

### Answer

Each **distinct Layer-3 network segment** is a subnet.

The three Ethernet networks are separate subnetworks.

The router-to-router connections also represent networks.

The PPT counts:

```text
2 + 3 + 1 = 6
```

where the router-to-router/common connection is counted appropriately.

### Answer

**6 subnets**

The answer slide explicitly states **C = 6**, explaining that each interface/network segment corresponds to a subnet while the common router-to-router connection counts as one.

---

# 33. Tricky Subnet Question — Gateway and Host

This is the continuation of the previous `192.168.1.x/27` family.

### Core question

For a host to communicate directly with a gateway, what must be true?

### Answer

The host and gateway must produce the **same subnet ID** using the subnet mask.

Example:

```text
Host:
192.168.1.97

Mask:
255.255.255.224
```

Calculate:

```text
192.168.1.97
AND
255.255.255.224

= 192.168.1.96
```

Gateway:

```text
192.168.1.110
```

AND mask:

```text
192.168.1.110
AND
255.255.255.224

= 192.168.1.96
```

Same subnet:

```text
192.168.1.96/27
```

Therefore the gateway is valid.

---

# 34. The Core Pattern Behind Almost Every Question

> [!important] Don't memorize individual questions

Almost the entire PPT reduces to these operations.

### Pattern 1 — Find network ID

```text
IP
AND
Subnet Mask
=
Network ID
```

---

### Pattern 2 — Find number of subnets

If you borrow `s` bits:

```text
Number of subnets = 2^s
```

---

### Pattern 3 — Find hosts

If `h` host bits remain:

```text
Total addresses = 2^h
```

Normally:

```text
Usable hosts = 2^h - 2
```

---

### Pattern 4 — Find prefix from mask

Example:

```text
255.255.255.224
```

Binary:

```text
11111111.11111111.11111111.11100000
```

Count ones:

```text
27
```

Therefore:

```text
/27
```

---

### Pattern 5 — Find block size

For the interesting octet:

```text
Block size = 256 - mask value
```

Example:

```text
Mask = 255.255.255.224

256 - 224 = 32
```

Subnets:

```text
0–31
32–63
64–95
96–127
...
```

---

### Pattern 6 — Longest Prefix Match

When routing:

```text
Destination
      ↓
Which prefixes match?
      ↓
Several may match
      ↓
Choose the longest prefix
      ↓
Forward
```

For example:

```text
/16 matches
/20 matches
/24 matches
```

Choose:

```text
/24
```

because:

```text
24 > 20 > 16
```

---

### Pattern 7 — `/32`

```text
/32
```

means:

```text
all 32 bits fixed
```

Therefore:

```text
exactly one IPv4 address
```

Example:

```text
192.168.1.132/32
```

matches only:

```text
192.168.1.132
```

---

### Pattern 8 — `/0`

```text
0.0.0.0/0
```

means:

```text
zero bits fixed
```

Therefore it matches:

```text
EVERY IPv4 address
```

That's why it is the:

```text
default route
```

---

# GATE Mistake Traps From This PPT

> [!danger] Trap 1 — Same IP range ≠ same subnet  
> You **must use the mask**.

---

> [!danger] Trap 2 — Multiple routes can match  
> Don't stop at the first matching route.
> 
> Always ask:
> 
> **"Is there a longer prefix that also matches?"**

---

> [!danger] Trap 3 — Host and gateway  
> Host and gateway must have the **same subnet ID** if they are directly connected.

---

> [!danger] Trap 4 — `/27` mental shortcut

```text
/27
↓
255.255.255.224
↓
block size = 32
↓
0, 32, 64, 96, 128, 160, 192, 224
```

This instantly solves many GATE questions.

---

> [!danger] Trap 5 — VLSM allocation  
> Allocate **largest requirement first**.

```text
120
↓
60
↓
20
↓
10
...
```

---

> [!danger] Trap 6 — Don't confuse total addresses and usable hosts

```text
2^h
```

is generally **total addresses**.

For traditional subnetting:

```text
2^h - 2
```

is usable hosts.

---

# One Mental Model to Remember

Think of an IP address as:

```text
+---------------- NETWORK ----------------+---- HOST ----+
|                                         |              |
|          fixed by subnet mask           |   variable   |
+-----------------------------------------+--------------+
```

The mask tells you:

> **"Which bits am I allowed to compare?"**

So when a router receives:

```text
Destination IP
```

it essentially does:

```text
Destination IP
       ↓
Look at prefix bits
       ↓
Which routes match?
       ↓
Pick the route with MOST fixed bits
       ↓
Next hop / interface
```

That is the intuition behind **CIDR + Longest Prefix Matching + subnetting**.

The later PPT questions repeatedly apply exactly this mechanism, including the 8-bit forwarding exercises and the `196.94.x.x` CIDR table.

---

## Quick GATE Formula Sheet

```text
Number of subnets
= 2^(borrowed bits)

Total addresses
= 2^(host bits)

Usable hosts
= 2^(host bits) - 2

Prefix length
= number of 1s in subnet mask

Block size
= 256 - mask value in interesting octet

Network ID
= IP AND subnet mask

Broadcast
= all host bits = 1

/32
= one exact IP

/0
= all IPs

Routing
= Longest Prefix Match

VLSM
= Allocate largest subnet first

Gateway
= Must belong to same subnet as host
```

**This is the version I'd keep in Obsidian.** The PDF itself is 87 pages, but a lot of those pages are continuation/solution slides rather than independent questions; the actual material boils down to the question patterns above. The final pages also include the tricky `/27` gateway/subnet questions and the enterprise subnet-count problem.