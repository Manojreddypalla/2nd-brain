Absolutely. I’ll give you the **question set from the lecture itself**, with the answer and the calculation/reasoning you should use in an exam. I’ll keep the same order as the lecture rather than adding unrelated questions.

# More in Subnetting — Lecture Questions + Answers

## Q1. VLSM — University Departments

A university has the block:

**128.232.1.0/24**

Requirements:

- Department A → up to 60 hosts
    
- Department B → up to 120 hosts
    
- Department C → up to 30 hosts
    
- Department D → up to 30 hosts
    

Which allocation wastes the least address space?

### Answer: A

```text
B → /25
A → /26
C → /27
D → /27
```

### Calculation

Start with largest requirement.

**B = 120 hosts**

```text
2^7 - 2 = 126 ≥ 120

Host bits = 7
Prefix = 32 - 7 = /25
```

**A = 60 hosts**

```text
2^6 - 2 = 62 ≥ 60

Prefix = 32 - 6 = /26
```

**C = 30**

```text
2^5 - 2 = 30

Prefix = /27
```

Same for D.

Total address space:

```text
/25 = 128
/26 = 64
/27 = 32
/27 = 32

128 + 64 + 32 + 32
= 256
```

A `/24` contains exactly 256 addresses.

So it fits perfectly.

---

# Q2. VLSM Allocation

Given:

**200.1.1.0/24**

Requirements:

```text
Subnet A → 72 hosts
Subnet B → 35 hosts
Subnet C → 20 hosts
Subnet D → 18 hosts
```

Which arrangement satisfies the requirements with minimal wastage?

### Answer given in lecture: B

The lecture's answer slide marks **Option B**.

### Required subnet sizes

For A:

```text
72 hosts

/26 → 62 usable ❌
/25 → 126 usable ✅

A → /25
```

For B:

```text
35 hosts

/27 → 30 usable ❌
/26 → 62 usable ✅

B → /26
```

For C:

```text
20 hosts

/27 → 30 usable

C → /27
```

For D:

```text
18 hosts

/27 → 30 usable

D → /27
```

Therefore the sizes are:

```text
A → /25
B → /26
C → /27
D → /27
```

---

# Q3. How Many Subnets Are There?

The lecture gives a topology with three routers and three LAN regions.

### Answer: 6

Think of each independent network segment as one subnet.

```text
3 LAN networks
+
3 router-router links
=
6 subnets
```

### Shortcut

> Imagine cutting every router interface away. Every isolated network "island" is one subnet.

---

# Q4. Simple Two-Router Topology

```text
LAN1
 |
R1 ----- R2
          |
         LAN2
```

How many subnets?

### Answer: 3

```text
LAN1 ↔ R1 = 1

R1 ↔ R2 = 1

R2 ↔ LAN2 = 1
```

Therefore:

```text
Total = 3
```

---

# Q5. GATE CSE 2022 — Enterprise Network

The lecture shows the GATE CSE 2022 question with:

- Two Ethernet segments
    
- One web server
    
- Firewall
    
- Three routers
    

Question:

**What is the number of subnets inside the enterprise network?**

Options:

```text
A. 3
B. 12
C. 6
D. 8
```

### Answer: C — 6

The lecture's method initially counts router interfaces:

```text
2 + 3 + 2 = 7
```

But one network segment is common/shared.

Therefore:

```text
7 - 1 = 6
```

### Exam intuition

Don't blindly count interfaces.

Two interfaces connected by the **same network segment** belong to the same subnet.

---

# Q6. Assign IP Addresses Using VLSM

Given:

```text
192.168.0.0/22
```

Topology requires:

```text
LAN A → 300 hosts
LAN B → 120 hosts
R1 ↔ R2 point-to-point link
```

Find suitable subnets.

### Step 1 — LAN A

Need 300 hosts.

```text
/24 → 254 usable ❌

/23 → 510 usable ✅
```

Therefore:

```text
LAN A = 192.168.0.0/23
```

Range covered:

```text
192.168.0.0
       ↓
192.168.1.255
```

---

### Step 2 — LAN B

Need 120 hosts.

```text
/25 → 126 usable
```

Therefore:

```text
LAN B = 192.168.2.0/25
```

---

### Step 3 — Router link

The lecture allocates:

```text
192.168.2.128/30
```

Usable addresses:

```text
192.168.2.129
192.168.2.130
```

So:

```text
R1 = 192.168.2.129
R2 = 192.168.2.130
```

Final:

```text
LAN A:
192.168.0.0/23

LAN B:
192.168.2.0/25

R1 ↔ R2:
192.168.2.128/30
```

---

# Q7. Router Interface Address Question

The lecture gives a LAN where hosts have addresses beginning with:

```text
111.111.111.xxx
```

and subnet masks are `/24`.

You must assign an address to the router's left interface.

### Answer

Any **valid host address in the same `/24` subnet**, excluding addresses already used and the reserved network/broadcast addresses.

Concept:

```text
Hosts:
111.111.111.x/24

Router interface MUST also be:

111.111.111.x/24
```

### Key rule

```text
Host -------- Router interface
```

If directly connected:

> Both must belong to the same subnet.

---

# Q8. Most Efficient Point-to-Point Mask

A link between two routers needs only **2 IP addresses**, one for each endpoint.

Which subnet mask is most efficient?

Options:

```text
A. /29
B. /28
C. /30
D. /31
```

### Lecture answer: D — `/31`

```text
/31
→ 2 addresses
```

The lecture notes that `/31` can be used for point-to-point links with exactly two IPs.

---

# Q9. Four Routers in Full Mesh

Four routers are connected in a **full mesh**.

Each router also connects to one LAN.

How many subnets?

### Step 1 — Router links

For a full mesh:

```text
n(n-1)/2
```

For `n = 4`:

```text
4(3)/2
= 6
```

### Step 2 — LANs

```text
4 routers
→ 4 LANs
```

Therefore:

```text
6 + 4
= 10
```

### Answer: 10 subnets

---

# Q10. Binary Tree of 7 Routers

There are 7 routers arranged as a binary tree.

The leaves connect to 4 LANs.

How many subnets?

### Router links

A tree with `n` nodes has:

```text
n - 1
```

edges.

Therefore:

```text
7 - 1 = 6
```

router-router links.

Add LANs:

```text
6 + 4
= 10
```

### Answer: 10 subnets

---

# Q11. Star-Like Router Topology

The lecture shows:

```text
        LAN1
         |
         R1
          \
LAN2--R2--R0--LAN0
          /
         R3
         |
        LAN3
```

How many subnets?

### Router links

```text
R0-R1
R0-R2
R0-R3

= 3
```

LANs:

```text
LAN0
LAN1
LAN2
LAN3

= 4
```

Therefore:

```text
3 + 4
= 7
```

### Answer: 7 subnets

---

# Q12. Four Routers + Four LANs

The lecture gives another four-router topology containing **6 router-router links** and **4 LANs**.

How many subnets?

```text
Router links = 6
LANs = 4

Total = 10
```

### Answer: 10 subnets

---

# Q13. Router Connected to 3 LANs + 2 Routers

A router connects to:

```text
3 LANs
+
2 other routers using point-to-point links
```

How many subnets are required?

Options:

```text
3
4
5
6
```

### Answer: 5

Each LAN:

```text
3 LANs → 3 subnets
```

Each P2P link:

```text
2 links → 2 subnets
```

Therefore:

```text
3 + 2 = 5
```

---

# Q14. Can These Two Hosts Communicate?

```text
Host A = 192.168.4.10/24

Host B = 192.168.5.20/24
```

They are directly connected using Ethernet.

Can they communicate normally?

### A's network

```text
192.168.4.10/24

→ 192.168.4.0/24
```

### B's network

```text
192.168.5.20/24

→ 192.168.5.0/24
```

Different subnets.

Therefore A thinks:

```text
B is remote.
```

B thinks:

```text
A is remote.
```

No router/default gateway exists between them.

### Answer: NO

The lecture explicitly shows this result.

---

# Q15. One-Way Communication

Given:

```text
Host A = 192.168.4.10/23
Host B = 192.168.5.20/24
```

Direct connection, no router.

What happens?

### From A's perspective

A has `/23`.

```text
A's network:

192.168.4.0/23
```

Range:

```text
192.168.4.0
to
192.168.5.255
```

B = `192.168.5.20`

So B lies inside A's perceived subnet.

Therefore:

```text
A thinks B is local.
```

A sends directly.

---

### From B's perspective

B has `/24`.

```text
B's network:

192.168.5.0/24
```

Range:

```text
192.168.5.0
to
192.168.5.255
```

A = `192.168.4.10`

Not inside the range.

Therefore:

```text
B thinks A is remote.
```

With no gateway, B cannot return the packet normally.

### Answer

```text
A → B : direct
B → A : not direct
```

This is the lecture's one-way communication example.

---

# Q16. Same Idea as MCQ

Hosts:

```text
Host A:
192.168.4.10
255.255.254.0 (/23)

Host B:
192.168.5.20
255.255.255.0 (/24)
```

Which is true?

```text
A. Both can send directly

B. A can send to B directly,
   but B cannot send to A directly

C. B can send to A directly,
   but A cannot send to B directly

D. Neither can send directly
```

### Answer: B

Because:

```text
A's /23 includes B

B's /24 does NOT include A
```

---

# Q17. Different Masks — 203.197.x.x

Given:

```text
C1:
203.197.2.53
255.255.128.0

C2:
203.197.75.201
255.255.192.0
```

The lecture concludes:

```text
C1 thinks C2 is inside its subnet.

C2 thinks C1 is outside its subnet.
```

Therefore communication becomes asymmetric:

```text
C1 → C2 : direct

C2 → C1 : does not consider C1 local
```

### Key lesson

**Never use only one mask when two hosts have different masks.**

Calculate:

```text
C1's view of C2

AND

C2's view of C1
```

separately.

---

# Q18. Two Computers — Same or Different Networks?

The lecture gives:

```text
A:
IP = 203.197.17.157
Mask = 255.255.128.0

B:
IP = 203.197.192.201
Mask = 255.255.192.0
```

Which statement is true?

The lecture marks:

### Answer: D

```text
A and B both assume
they are on different networks.
```

---

# Q19. GATE IT 2008 — Number of Subnets

The lecture gives:

```text
Host X
 |
R1
 |
R2
 |
Host Y
```

and asks:

**How many distinct subnets are guaranteed to already exist?**

Options:

```text
A. 1
B. 2
C. 3
D. 6
```

### Lecture answer: C — 3

Think structurally:

```text
X ↔ R1
    ↓
 subnet 1

R1 ↔ R2
    ↓
 subnet 2

R2 ↔ Y
    ↓
 subnet 3
```

### Answer: 3

---

# Q20. Forwarding Table — 192.168.1.10

Routing table:

```text
Destination       Interface

192.168.1.0/24    eth0
10.0.0.0/8        eth1
172.16.0.0/12     eth2
0.0.0.0/0         eth3
```

Destination packet:

```text
192.168.1.10
```

Which interface?

It matches:

```text
192.168.1.0/24
```

### Answer: `eth0`

---

# Q21. Forwarding Table — 10.55.33.8

Same routing table.

Destination:

```text
10.55.33.8
```

Matches:

```text
10.0.0.0/8
```

### Answer: `eth1`

---

# Q22. Forwarding Table — 8.8.8.8

Destination:

```text
8.8.8.8
```

Check:

```text
192.168.1.0/24 ❌

10.0.0.0/8 ❌

172.16.0.0/12 ❌
```

So use:

```text
0.0.0.0/0
```

which is the **default route**.

### Answer: `eth3`

---

# ⭐ The Question Patterns From This Lecture

You basically need to recognize these patterns:

|Question says...|Immediately think...|
|---|---|
|X hosts required|`2^h - 2 ≥ X`|
|Multiple host requirements|VLSM, largest first|
|Count subnets in diagram|LANs + router links|
|Full mesh|`n(n-1)/2` links|
|Tree|`n-1` links|
|Two directly connected interfaces|Must share a subnet|
|Two hosts with different masks|Check **A→B and B→A separately**|
|Destination outside local network|Default gateway|
|Routing/forwarding table|Longest prefix match|
|Nothing matches|`0.0.0.0/0`|
|P2P, exactly 2 addresses|`/31` in the lecture|

The **highest-value questions to redo without seeing the answer are Q1, Q5, Q6, Q15/Q16, Q17, Q19, and Q20–22**. Those cover almost every conceptual trap the lecture is trying to teach.