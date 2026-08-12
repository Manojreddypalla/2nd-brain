Absolutely. I went through the **full 96-page PDF**, including the diagrams, handwritten explanations, route-aggregation examples, longest-prefix matching examples, and the GATE/practice questions at the end. Since this is your lecture reference, I’ll keep the notes aligned with **Go Classes' flow and terminology**, rather than mixing in unrelated theory.

# 🌐 Computer Networks — Lecture 3: Supernetting / Route Aggregation

---

# 1. CIDR Prefix Basics

An IPv4 address has **32 bits**.

If a network is written as:

```text
x.y.z.w/n
```

then:

- `n` = number of **initial/network bits**
    
- Remaining `32 − n` bits = **host bits**
    

### Number of IP addresses

$$  
\boxed{2^{32-n}}  
$$ 

Example:

```text
192.168.1.0/24
```

Network bits:

```text
24
```

Host bits:

```text
32 - 24 = 8
```

Therefore:

$$  
2^8 = 256  
$$ 

IP addresses.

The lecture also shows that if a `/n` network is divided into **4 equal subnets**, each subnet has:

$$ 
2^{32-n-2}  
$$ 

addresses because 2 bits are used to distinguish the four subnets.

---

# 2. What Is Supernetting?

The lecture calls this:

> **Supernetting = Route Aggregation**

The basic idea:

```text
Many smaller networks
        ↓
   combine them
        ↓
One larger prefix
```

Why?

Because routers don't want unnecessarily huge forwarding tables.

Instead of storing:

```text
Network A → same interface
Network B → same interface
Network C → same interface
Network D → same interface
```

we can potentially store:

```text
Combined Network → same interface
```

Supernetting is therefore primarily a **logical forwarding-table concept** in this lecture.

---

# 3. First Understand IP Packet Forwarding

An IP packet contains:

```text
Source IP
Destination IP
Data
```

The router mainly cares about the **destination IP** when deciding where to send the packet.

Conceptually:

```text
Packet
   ↓
Router
   ↓
Look at destination IP
   ↓
Search forwarding table
   ↓
Determine outgoing interface
   ↓
Forward packet
```

The lecture's first example shows a router receiving a packet destined for:

```text
201.143.7.0
```

and consulting its forwarding table.

---

# 4. Forwarding Table

A forwarding table contains something like:

|Prefix|Port|
|---|---|
|`201.143.0.0/22`|Port 1|
|`201.143.4.0/24`|Port 2|
|`201.143.5.0/24`|Port 3|
|`201.143.6.0/23`|Port 4|

The router asks:

> **Which prefix matches the destination IP?**

For destination:

```text
201.143.7.0
```

the matching route is:

```text
201.143.6.0/23
```

so:

```text
→ Port 4
```

The lecture demonstrates this by converting the relevant octets to binary and comparing the prefix bits.

---

# 5. How Does Prefix Matching Work?

Suppose:

```text
Destination = 201.143.7.0
```

The router checks the prefix.

For:

```text
201.143.6.0/23
```

the important bits are:

```text
201.143.6
```

plus the appropriate prefix bits from the next octet.

The host bits are **not relevant for determining whether the address belongs to that network**.

The lecture explicitly demonstrates this by writing the destination and each forwarding-table prefix in binary and comparing their initial bits.

---

# 6. Forwarding Table of R1

The lecture gives an example with four organizations:

```text
Organization 1 → 140.24.7.0/26
Organization 2 → 140.24.7.64/26
Organization 3 → 140.24.7.128/26
Organization 4 → 140.24.7.192/26
```

For R1, each organization is reached through a **different interface**:

```text
140.24.7.0/26      → m0
140.24.7.64/26     → m1
140.24.7.128/26    → m2
140.24.7.192/26    → m3
default            → m4
```

The default route sends traffic elsewhere, such as toward the Internet.

---

# 7. Forwarding Table of R2

R2 is different.

All four organizations are reachable through the **same interface `m0`**:

```text
140.24.7.0/26      → m0
140.24.7.64/26     → m0
140.24.7.128/26    → m0
140.24.7.192/26    → m0

default            → m1
```

This is where the question arises:

> **Can we shorten this table?**

Yes. 😎

---

# 8. Why Can We Aggregate These Four Networks?

The four networks are:

```text
140.24.7.0/26
140.24.7.64/26
140.24.7.128/26
140.24.7.192/26
```

A `/26` has:

$$ 
2^{32-26}=2^6=64  
$$ 

addresses.

So the four blocks are:

```text
0   – 63
64  – 127
128 – 191
192 – 255
```

Together:

```text
0 – 255
```

That's the entire fourth octet.

Therefore:

```text
140.24.7.0/24
```

covers all four.

So:

```text
4 × /26
       ↓
1 × /24
```

and R2's table becomes:

```text
140.24.7.0/24 → m0
default        → m1
```

This is **route aggregation / supernetting**.

---

# 9. The Binary Intuition Behind Aggregation

This is probably the most important part to understand.

The fourth octet starts at:

```text
0   = 00000000
64  = 01000000
128 = 10000000
192 = 11000000
```

Notice the first two bits:

```text
00
01
10
11
```

All four possible combinations occur.

So the four `/26` networks are effectively:

```text
140.24.7.00xxxxxx
140.24.7.01xxxxxx
140.24.7.10xxxxxx
140.24.7.11xxxxxx
```

Since those two bits vary across all four networks, we can no longer use them as part of the common prefix.

Therefore:

```text
/26 → /24
```

and:

```text
140.24.7.0/24
```

represents all four.

The lecture uses exactly this bit-level reasoning in its aggregation explanation.

---

# 10. Critical Condition for Aggregation

This is **VERY important for GATE**.

You cannot simply combine networks because their IP ranges look related.

The networks need to be reachable through the **same next hop/interface**.

Example:

```text
A → m0
B → m0
C → m0
D → m0
```

Potentially:

```text
A+B+C+D → m0
```

But:

```text
A → m0
B → m1
C → m2
D → m3
```

You **cannot** aggregate them into one forwarding-table entry because the router needs different forwarding decisions.

The lecture explicitly demonstrates this with R1: although the four networks are adjacent, R1 cannot aggregate them because they use different interfaces.

### Mental rule

> **Same destination direction + compatible prefix structure → aggregation may be possible.**

---

# 11. Supernetting vs Subnetting

This distinction is important.

### Subnetting

One larger network:

```text
          /24
           ↓
   ┌────┬────┬────┬────┐
  /26  /26  /26  /26
```

You're **splitting** a network.

### Supernetting

Multiple smaller networks:

```text
/26 + /26 + /26 + /26
             ↓
            /24
```

You're **combining** networks.

The lecture makes this conceptual distinction explicitly: subnetting is treated as a physical allocation concept, whereas route aggregation is a logical forwarding-table concept.

---

# 12. Route Aggregation Is a Logical Concept

This is worth remembering.

Supernetting doesn't necessarily mean that the physical networks suddenly become one physical network.

For example:

```text
Organization A
Organization B
Organization C
Organization D
```

may still be separate networks.

The router is simply saying:

> "Whenever the destination falls inside this larger range, send it through the same next hop."

So aggregation changes the **representation in the forwarding table**.

It reduces the number of entries the router needs to maintain.

---

# 13. What If One Organization Is Somewhere Else?

The lecture then changes the topology.

Suppose:

```text
Organization 1 ─┐
Organization 2 ─┤
Organization 3 ─┘
                 R1 ─── R2 ─── Internet

Organization 4 ─────────────── R2
```

Now Organization 4 is somewhere else.

R1 can aggregate the first three:

```text
140.24.7.0/24 → m0
```

but R2 has to distinguish Organization 4 separately.

So R2 could have:

```text
140.24.7.0/24     → m0
140.24.7.192/26   → m1
default            → m2
```

Why?

Because `140.24.7.192/26` is more specific and needs a different destination.

The lecture demonstrates this exact situation.

---

# 14. Aggregation Can Cover Extra IP Addresses

This is a subtle but **important** concept.

Suppose you aggregate:

```text
140.24.7.0/26
140.24.7.64/26
140.24.7.128/26
```

You might want:

```text
140.24.7.0/24
```

But `/24` covers:

```text
0 – 255
```

while the original networks only covered:

```text
0 – 191
```

So aggregation would also cover:

```text
192 – 255
```

These are **extra IP addresses**.

The lecture's rule:

> If aggregation covers extra IPs, mention the extra entry separately and let **longest prefix matching** handle it.

---

# 15. Example of Extra-IP Handling

Suppose:

```text
140.24.7.0/24 → m0
140.24.7.192/26 → m1
default → m2
```

The `/24` covers:

```text
0 – 255
```

The `/26` covers:

```text
192 – 255
```

So addresses in:

```text
192 – 255
```

match **both**.

But that's okay.

Because:

```text
/26
```

is more specific than:

```text
/24
```

Therefore:

```text
140.24.7.200
```

uses:

```text
140.24.7.192/26 → m1
```

while:

```text
140.24.7.100
```

uses:

```text
140.24.7.0/24 → m0
```

This leads directly to the most important forwarding rule in this lecture.

---

# 16. Longest Prefix Match (LPM)

### Definition

When multiple prefixes match a destination address:

> **Choose the matching prefix with the greatest number of bits.**

In other words:

```text
/32 beats /24
/24 beats /16
/16 beats /8
```

provided they all match.

The lecture explicitly defines longest-prefix matching as selecting the **longest address prefix that matches the destination address**.

---

# 17. Why Longest Prefix Match Exists

Imagine:

```text
140.24.7.0/24 → m0
140.24.7.192/26 → m1
```

Destination:

```text
140.24.7.200
```

Both match.

But `/24` is broad:

```text
0 – 255
```

while `/26` is specific:

```text
192 – 255
```

The router chooses:

```text
/26 → m1
```

because it is the **longest matching prefix**.

So LPM allows us to safely use aggregation even when the aggregated prefix covers some addresses that require a different route.

---

# 18. Important LPM Example

Forwarding table:

|Prefix|Interface|
|---|--:|
|`198.15.0.0/16`|1|
|`198.15.7.0/24`|7|
|`198.15.7.3/32`|4|

Destination:

```text
198.15.7.3
```

All three match.

Lengths:

```text
/16
/24
/32
```

Longest:

```text
/32
```

Therefore:

```text
→ Interface 4
```

Destination:

```text
198.15.7.4
```

Now `/32` does not match.

So the longest matching prefix becomes:

```text
/24
```

Therefore:

```text
→ Interface 7
```

The lecture works through this exact example.

---

# 19. Another LPM Example

Destination:

```text
128.143.71.21
```

Forwarding table includes:

```text
128.143.0.0/16
128.143.64.0/20
128.143.192.0/20
128.143.71.0/24
128.143.71.55/32
default
```

The destination matches:

```text
128.143.0.0/16
128.143.64.0/20
128.143.71.0/24
```

but does **not** match:

```text
128.143.71.55/32
```

because destination is `.21`, not `.55`.

Therefore:

```text
/24
```

is the longest matching prefix.

So:

```text
→ R4
```

The lecture explicitly identifies `/24` as the longest match.

---

# 20. Hop-by-Hop Packet Forwarding

Each router has its own forwarding table.

When a packet arrives:

```text
1. Router receives packet
        ↓
2. Inspect destination IP
        ↓
3. Search forwarding table
        ↓
4. Find longest prefix match
        ↓
5. Select associated outgoing interface
        ↓
6. Forward packet
```

Then the next router repeats the process.

Hence:

> **Hop-by-hop forwarding**

The forwarding table itself may be created using:

- Routing algorithms
    
- Static configuration
    

The lecture summarizes this process explicitly.

---

# 21. Route Aggregation and Routing Table Size

This is the main motivation.

Suppose:

```text
10.1.0.0/24 → R3
10.1.2.0/24 → eth1
10.2.1.0/24 → eth0
10.3.1.0/24 → R3
20.2.0.0/16 → R2
30.1.0.0/28 → R2
```

If multiple routes share the same next hop and can be represented by a valid aggregate, they can potentially be combined.

The lecture's main point:

> Route aggregation significantly reduces the size of Internet routing tables.

---

# 22. Route Aggregation Rules

From the lecture, your checklist should be:

### Step 1

Look at the networks you want to aggregate.

### Step 2

Check their binary prefixes.

### Step 3

Find the bits that are common.

### Step 4

Determine the **most specific common prefix**.

### Step 5

Check whether all those networks use the **same next hop/interface**.

### Step 6

Check whether the aggregate covers extra IP addresses.

### Step 7

If it does, preserve the more specific route separately.

### Step 8

Remember that **Longest Prefix Match** resolves overlapping routes.

This procedure is summarized in the lecture's aggregation discussion.

---

# 23. Very Important Misconception

The lecture explicitly calls out common misconceptions.

### Misconception 1

> "Addresses must always be contiguous."

Not necessarily in the simplistic way people often assume.

What matters for aggregation is whether the prefixes can be represented by a **common valid prefix**.

### Misconception 2

> "The number of subnets being combined must always be (2^k)."

This is a common misconception, but aggregation can involve more nuanced cases depending on which routes are being represented and whether extra addresses are covered.

The lecture flags these as misconceptions and then emphasizes checking the actual matching bits/prefix structure.

---

# 24. Aggregation and Extra Addresses

The lecture gives an especially useful rule:

> If aggregation causes you to cover extra IP addresses, mention the original/specific entry separately and let **longest-prefix matching** take care of it.

But if the extra addresses are **not allocated to anyone**, there's no need to worry about the overlap in the same way.

This is a nice GATE-level trap.

---

# 25. Example: Three Networks Aggregated

The lecture gives:

```text
192.24.0.0/21
192.24.16.0/20
192.24.8.0/22
```

These represent different-sized ranges.

The lecture aggregates them into:

```text
192.24.0.0/19
```

This aggregate contains more addresses than the three original networks individually cover.

The lecture calculates:

### Separate networks

$$ 
2^{11}+2^{12}+2^{10}  
$$ 

$$   
=2^{11}+2^{12}+2^{10}  
$$ 

### Aggregate

A `/19` has:

$$ 
2^{32-19}=2^{13}  
$$ 

addresses.

Therefore the aggregate contains:

$$ 
2^{13}  
$$ ]

while the original networks together contain:

$$ 
2^{11}+2^{12}+2^{10}  
$$ 

The difference represents the **extra IPs covered by aggregation**.

---

# 26. Aggregation Doesn't Need to Mean "Adjacent Looking Numbers"

Another example in the lecture shows that the networks may not simply appear as:

```text
0
1
2
3
```

in the last octet.

The important thing is:

> **Check the bits.**

Find the **most specific prefix that all required networks share**.

The lecture literally says to check "till what point things are matching" and then select the most specific option.

---

# 27. The "Most Specific Option" Idea

Suppose the binary prefixes look like:

```text
140.24.7.00--------
140.24.7.01--------
140.24.7.10--------
140.24.7.11--------
```

All four have:

```text
140.24.7
```

in common.

The first two bits of the fourth octet vary:

```text
00
01
10
11
```

Therefore the common prefix ends before those two bits.

So the aggregate is:

```text
140.24.7.0/24
```

This is exactly how you should **derive** an aggregate rather than memorizing formulas.

---

# 28. Forwarding vs Routing

Keep this distinction clear:

### Routing

Determines:

> **What routes should exist?**

### Forwarding

Actually uses the forwarding table:

> **Given this destination IP, which interface should I send it through?**

This lecture is heavily focused on **forwarding tables and route aggregation**.

The forwarding table maps:

```text
Destination prefix
        ↓
Outgoing interface / next hop
```

---

# 29. Default Route

The default route is represented as:

```text
0.0.0.0/0
```

It means:

> Match everything that doesn't have a more specific matching route.

So conceptually:

```text
Specific route
      ↓
More specific route
      ↓
Even more specific route
      ↓
Default route
```

The default route is effectively the fallback.

The lecture uses it repeatedly in forwarding-table examples.

---

# 30. LPM + Default Route

Suppose:

```text
140.24.7.0/24 → m0
default        → m1
```

Destination:

```text
140.24.7.100
```

Matches `/24`.

Therefore:

```text
→ m0
```

Destination:

```text
8.8.8.8
```

Doesn't match `/24`.

Therefore:

```text
→ default → m1
```

So default is effectively the **least-specific fallback route**.

---

# 31. Important GATE Pattern: Don't Do Unnecessary Work

The lecture's later questions repeatedly use:

```text
Destination IP
+
Subnet mask / prefix
```

and ask which forwarding-table entry matches.

The general operation is:

$$ 
\boxed{\text{Destination IP AND Subnet Mask} = \text{Network Address}}  
$$ 

Then compare the resulting network address with the table entry.

But when the question asks **only for the subnet mask**, don't automatically calculate the network ID.

That's a useful exam-discipline rule.

---

# 32. GATE Example: Same Prefix, Different Specificity

The PDF includes a GATE question with:

```text
128.75.43.0     255.255.255.0
128.75.43.60    255.255.255.128
192.12.17.0     255.255.255.255
Default
```

The important skill is:

> Determine which route matches the destination, then choose the **longest/specific matching prefix**.

The solution demonstrates using bitwise AND to determine whether a destination belongs to a route.

---

# 33. GATE Example: CIDR + Longest Prefix

The PDF contains a GATE CSE 2014 question where the routing table contains:

```text
131.16.0.0/12
131.28.0.0/14
131.19.0.0/16
131.22.0.0/15
```

For the destination:

```text
131.23.151.76
```

multiple prefixes may appear relevant.

The correct procedure is:

```text
Check /12
Check /14
Check /16
Check /15
        ↓
Find all matching prefixes
        ↓
Choose longest matching one
```

The lecture's solution concludes that the `/15` route is the longest matching prefix.

---

# 34. Another Important GATE Pattern: Bitwise AND

For a destination:

```text
IP address
```

and mask:

```text
Subnet mask
```

perform:

```text
IP
AND
MASK
----
Network ID
```

Example conceptually:

```text
IP:       128.96.171.92
Mask:     255.255.254.0
          ----------------
Network:  128.96.170.0
```

Then compare that network ID against the forwarding table.

The PDF repeatedly uses this method in its GATE examples.

---

# 35. Another LPM Shortcut

If you already know the prefix lengths, you don't always need to calculate everything.

Suppose:

```text
/8
/16
/20
/24
/32
```

and the destination matches:

```text
/8
/16
/24
```

Immediately:

```text
/24 wins
```

because:

$$  
24>16>8  
$$ 

The lecture's examples repeatedly demonstrate this idea.

---

# 36. Subnetting Questions Included in the PDF

The last part of the lecture shifts into **subnetting-related practice**.

For example, the PDF includes:

- subnet address allocation
    
- network ID calculation
    
- subnet mask identification
    
- gateway selection
    
- number of existing subnets
    
- broadcast-address-based subnet mask questions
    
- GATE subnet questions
    

These are included as practice/revision material around the forwarding and prefix concepts.

---

# 37. One Very Important Mental Model

Don't memorize:

> "Supernetting means decreasing the prefix by 2."

That's only true in a particular example.

Instead think:

```text
What networks do I have?
        ↓
Which bits are common?
        ↓
Where do they stop matching?
        ↓
That is my aggregate prefix.
```

For example:

```text
00xxxxxx
01xxxxxx
10xxxxxx
11xxxxxx
```

Common bits:

```text
[common][00/01/10/11][host]
```

The varying bits can't remain in the common prefix.

Therefore:

```text
/26 → /24
```

That's the actual reasoning.

---

# 38. The Entire Lecture in One Flow

This lecture is actually one connected story:

```text
IP Address
    ↓
Prefix / Mask
    ↓
Network range
    ↓
Forwarding table
    ↓
Destination IP matching
    ↓
Multiple routes may match
    ↓
Longest Prefix Match
    ↓
Multiple networks may use same next hop
    ↓
Route Aggregation / Supernetting
    ↓
Fewer forwarding-table entries
    ↓
Aggregation may cover extra addresses
    ↓
Longest Prefix Match handles exceptions
```

That's the **big picture**.

---

# 39. What You Should Be Able to Do After This Lecture

For GATE, you should be comfortable with these:

### Prefix basics

- Interpret `/n`
    
- Calculate host bits
    
- Calculate number of IP addresses
    
- Convert prefix ↔ mask
    

### Forwarding

- Read a forwarding table
    
- Check whether destination matches a prefix
    
- Determine outgoing interface
    

### Longest Prefix Match

- Identify all matching entries
    
- Compare prefix lengths
    
- Select the longest one
    

### Route aggregation

- Identify networks that can potentially be aggregated
    
- Find common prefix bits
    
- Calculate aggregate prefix
    
- Check whether same next hop/interface is used
    
- Identify extra IP addresses
    
- Preserve more-specific routes where necessary
    

### Subnetting

- Calculate network ID
    
- Determine subnet mask
    
- Determine subnet ranges
    
- Determine whether hosts belong to same subnet
    
- Solve gateway/subnet questions
    

These are exactly the skills reinforced by the practice and GATE questions in the latter portion of the PDF.

---

# 🧠 The 5 Things I Want You to Actually Understand

If you forget everything else, keep these five:

### 1. Prefix

```text
/n = first n bits identify the network
```

### 2. Forwarding

```text
Destination IP
      ↓
Find matching prefix
      ↓
Choose interface
```

### 3. Longest Prefix Match

```text
Multiple matches?
       ↓
Choose largest /n
```

### 4. Supernetting

```text
Multiple smaller networks
          ↓
   common prefix
          ↓
One larger aggregate
```

### 5. Aggregation condition

```text
Can aggregate only when the
combined route can safely use
the same forwarding decision.
```

And if the aggregate accidentally covers a more-specific network:

```text
Broad aggregate → default/general path
Specific prefix → special path

        ↓

Longest Prefix Match
handles it.
```

That is the **core of Lecture 3**.

---

## What we'll do next

Now that you've got the **full notes**, the best sequence is:

**1. Short notes** → only the things worth writing in your actual GATE notebook  
**2. Questions + answers** → first basic LPM, then aggregation, then GATE-level traps  
**3. Mistake patterns** → what to recognize instantly in an exam

So yes — **next we can compress this into your short GATE notes**, and then I'll make the question set from this exact PDF.