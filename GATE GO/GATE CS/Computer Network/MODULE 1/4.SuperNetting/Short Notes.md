# 🌐 GATE CN — Lecture 3: Supernetting / Route Aggregation — Short Notes

> **These are the notes worth actually writing in your notebook.** I’m keeping them compact and based on the lecture, not the full explanation.

---

## 1. Prefix / CIDR

IPv4 = **32 bits**

```text
IP/n
```

- `/n` → first `n` bits = **network/prefix**
    
- Host bits = `32 − n`
    
- Number of addresses:
    

$$  
\boxed{2^{32-n}}  
$$

---

## 2. IP Forwarding

Router receives packet → checks **destination IP** → searches forwarding table → selects outgoing interface.

```text
Destination IP
      ↓
Match prefix
      ↓
Outgoing interface
```

---

## 3. Prefix Matching

A destination IP **matches** a prefix if its first `n` bits are the same.

Example:

```text
140.24.7.0/24
```

means first **24 bits** must match.

---

## 4. Longest Prefix Match (LPM) ⭐

If destination matches multiple entries:

$$  
\boxed{\text{Choose the longest matching prefix}}  
$$

Example:

```text
140.24.7.0/24  → m0
140.24.7.192/26 → m1
```

For `140.24.7.200`:

Both match, but:

```text
/26 > /24
```

Therefore:

```text
→ m1
```

### Remember:

```text
/32 > /24 > /16 > /8
```

More `/n` → **more specific route**.

---

# 5. Route Aggregation / Supernetting ⭐

### Idea:

Combine multiple smaller networks into **one larger prefix**.

```text
Multiple routes
      ↓
Common prefix
      ↓
One aggregate route
```

### Purpose:

**Reduce forwarding-table size.**

---

## 6. Example of Aggregation

Four `/26` networks:

```text
140.24.7.0/26
140.24.7.64/26
140.24.7.128/26
140.24.7.192/26
```

Each `/26`:

$$  
2^{32-26}=64  
$$

Together cover:

```text
0 – 255
```

Therefore:

$$  
\boxed{140.24.7.0/24}  
$$

can represent all four.

```text
4 × /26
   ↓
1 × /24
```

---

## 7. Binary Idea Behind Aggregation ⭐

```text
0   = 00000000
64  = 01000000
128 = 10000000
192 = 11000000
```

First 2 bits:

```text
00
01
10
11
```

They vary across all four networks.

Therefore those bits **cannot remain part of the common prefix**:

```text
/26 → /24
```

### Pattern to recognize:

```text
Common bits → prefix
Varying bits → host portion
```

---

# 8. Condition for Aggregation ⭐

Aggregation is possible when the routes can safely be represented by **the same forwarding decision / next hop**.

Example:

```text
A → m0
B → m0
C → m0
D → m0
```

Can potentially become:

```text
A+B+C+D → m0
```

But:

```text
A → m0
B → m1
C → m2
```

cannot simply be replaced by one route.

---

# 9. Aggregation May Cover Extra IPs

An aggregate can sometimes cover addresses that weren't part of the original networks.

If those extra addresses require another route:

```text
Broad aggregate → general path
Specific route  → special path
```

Keep the specific route.

Then:

$$  
\boxed{\text{Longest Prefix Match handles the overlap}}  
$$

Example:

```text
140.24.7.0/24      → m0
140.24.7.192/26    → m1
```

For `.200`:

```text
/24 matches
/26 matches

/26 wins → m1
```

---

# 10. Subnetting vs Supernetting

### Subnetting

```text
One large network
       ↓
Multiple smaller networks
```

### Supernetting

```text
Multiple smaller networks
       ↓
One larger aggregate
```

---

# 11. Default Route

```text
0.0.0.0/0
```

Matches everything that doesn't have a more specific matching route.

So:

```text
Specific route → preferred
Default route  → fallback
```

---

# 12. Forwarding Process ⭐

```text
Packet arrives
      ↓
Check destination IP
      ↓
Find matching prefixes
      ↓
Longest Prefix Match
      ↓
Select interface
      ↓
Forward packet
```

This happens **hop-by-hop** at each router.

---

# 13. GATE Calculation

To determine whether an IP belongs to a network:

$$  
\boxed{\text{IP Address AND Subnet Mask = Network Address}}  
$$

Then compare with the forwarding-table network.

---

# 🧠 Supernetting Pattern to Recognize

When you see:

```text
Several networks
      +
Same next hop/interface
      +
Question says "aggregate", "summarize",
"shorten routing table"
```

Think:

> **ROUTE AGGREGATION**

Then:

```text
1. Convert relevant part to binary
2. Find common prefix bits
3. Stop where bits start differing
4. That gives aggregate prefix
5. Check for extra addresses
6. Use LPM for exceptions
```

### ⭐ One-line memory

$$  
\boxed{\text{Supernetting = multiple networks → common prefix → fewer routing entries}}  
$$

These are the **actual notebook-worthy points**. Don't copy the full lecture into your notes — the diagrams/examples can stay in the lecture/PDF, while these rules and patterns are what you need for revision.