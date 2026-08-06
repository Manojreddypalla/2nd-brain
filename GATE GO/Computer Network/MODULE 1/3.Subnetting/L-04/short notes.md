Yep. For **L-4: More in Subnetting**, I wouldn't write all the solved questions again. Write only the concepts/rules that you'll need while solving GATE questions. This is based on the lecture you uploaded.

# L-4 — More in Subnetting

## 1. VLSM

**VLSM = Variable Length Subnet Masking**

Used to create **different-sized subnets** according to host requirements → reduces IP wastage.

### Host requirement

For `H` hosts:

```text
2^h - 2 ≥ H

Prefix = 32 - h
```

Allocate subnets in:

```text
Largest requirement → Smallest requirement
```

Quick table:

```text
/24 → 256 addresses → 254 hosts
/25 → 128           → 126
/26 → 64            → 62
/27 → 32            → 30
/28 → 16            → 14
/29 → 8             → 6
/30 → 4             → 2
```

The lecture uses this VLSM idea to fit `/25 + /26 + /27 + /27` exactly inside a `/24`.

---

## 2. Counting Number of Subnets

### Golden rule

> **Each isolated network segment = 1 subnet.**

Easy counting:

```text
Each LAN               → 1 subnet
Each router-router link → 1 subnet
```

A router connects **different networks**.

### Useful formulas

```text
Full mesh of n routers:
Links = n(n-1)/2

Tree of n routers:
Links = n-1

Total subnets
= Router-router links + LANs
```

---

## 3. Directly Connected Interfaces

Interfaces on the **same link must belong to the same subnet**.

```text
LAN 190.3.5.0/24
       |
     Router

Router's connected interface
must also ∈ 190.3.5.0/24
```

Same idea for:

```text
R1 -------- R2

R1 interface and R2 interface
must belong to the same subnet.
```

This interface rule is emphasized in the lecture diagrams.

---

## 4. Point-to-Point Link

For a router-to-router P2P link needing exactly 2 IPs:

```text
/31 → 2 addresses
```

The lecture treats `/31` as the most address-efficient choice for a P2P link.

---

# 5. Host Configuration

Every IP host needs:

```text
IP Address
Subnet Mask
Default Gateway
```

**Subnet Mask:** determines whether destination is local or remote.

**Default Gateway:** router used to reach networks outside the local subnet.

---

# 6. Local vs Remote Destination ⭐

A host always uses **its OWN subnet mask** to decide whether another IP is local.

```text
My IP & My Mask
        ↓
My Network

Destination IP & My Mask
        ↓
Destination Network
```

If:

```text
Same network
→ Direct delivery

Different network
→ Send to Default Gateway
```

---

## 7. Different Masks → Asymmetric Communication ⭐

If two hosts have different masks, **check both directions separately**.

Example:

```text
A = 192.168.4.10/23
B = 192.168.5.20/24
```

A's `/23`:

```text
192.168.4.0 – 192.168.5.255

→ A thinks B is LOCAL
```

B's `/24`:

```text
192.168.5.0 – 192.168.5.255

→ B thinks A is REMOTE
```

Therefore:

```text
A → B : Direct possible
B → A : Needs gateway
```

### ⭐ GATE rule

```text
Never ask only:
"Are A and B in same subnet?"

Ask:
Does A think B is local?
Does B think A is local?
```

The lecture demonstrates exactly this one-way communication case.

---

# 8. Forwarding / Routing Table

Router uses the **destination IP** to select where to forward a packet.

```text
Destination Network     Interface

192.168.1.0/24          eth0
10.0.0.0/8              eth1
172.16.0.0/12           eth2
0.0.0.0/0               eth3
```

### Longest Prefix Match ⭐

If multiple entries match:

> **Choose the route having the largest prefix length.**

```text
/24 beats /16
/16 beats /8
/8  beats /0
```

Because larger prefix = **more specific network**.

---

# 9. Default Route

```text
0.0.0.0/0
```

Matches every destination.

Used when **no more specific route matches**.

Example:

```text
Destination = 8.8.8.8

No specific entry
        ↓
0.0.0.0/0
        ↓
Default route
```

---

# ⭐ L-4 Ultra-Short Revision

Write this box at the end:

```text
VLSM:
2^h - 2 ≥ Hosts
Prefix = 32-h
Allocate Largest → Smallest

Subnet Counting:
Each LAN = 1
Each router-router link = 1

Full Mesh links = n(n-1)/2
Tree links = n-1

Same physical/link network
→ interfaces must be in same subnet

Host checks destination using ITS OWN mask.

Local → Direct
Remote → Default Gateway

Different masks
→ Check A→B and B→A separately.

Forwarding:
Longest Prefix Match

Default Route:
0.0.0.0/0

P2P:
Lecture uses /31 for exactly 2 IPs.
```

**That is what I'd actually put in the handwritten L-4 notes.** The solved VLSM/subnet-counting questions can stay in your practice section rather than bloating the theory notes. 