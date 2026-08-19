# 1. First: What is "fragmentation"?

Think of physical memory as a long parking lot.

Processes arrive and leave:

```text
Physical Memory

+---------+
|   P1    |
+---------+
|         |
|  FREE   |
|         |
+---------+
|   P2    |
+---------+
|         |
|  FREE   |
|         |
+---------+
|   P3    |
+---------+
|         |
|  FREE   |
|         |
+---------+
```

The memory isn't necessarily full.

But the **free space is broken into pieces**.

That's the key idea.

---

# 2. Why is this called EXTERNAL fragmentation?

Because the fragmented/free space is **outside the allocated processes**.

```text
+---------+
|   P1    |
+---------+
| FREE    | ← external fragment
+---------+
|   P2    |
+---------+
| FREE    | ← external fragment
+---------+
|   P3    |
+---------+
| FREE    | ← external fragment
+---------+
```

The `FREE` regions are scattered around the allocated regions.

Hence:

> **External fragmentation = free memory exists, but it is split into multiple non-contiguous holes.**

---

# 3. Here's the important part

Suppose we have:

```text
FREE = 20 KB
FREE = 30 KB
FREE = 40 KB
```

Total free memory:

```text
20 + 30 + 40
= 90 KB
```

Now a new process needs:

```text
60 KB
```

You might say:

> "Easy! We have 90 KB free."

But look at the actual memory:

```text
+---------+
|   P1    |
+---------+
| FREE    | 20 KB
+---------+
|   P2    |
+---------+
| FREE    | 30 KB
+---------+
|   P3    |
+---------+
| FREE    | 40 KB
+---------+
```

There is **no contiguous 60 KB block**.

So the process cannot be allocated.

🔥 This is external fragmentation.

---

# 4. The most important distinction

Don't think:

> "External fragmentation means wasted memory."

That's too vague.

Instead think:

> **"Enough total free memory exists, but it isn't available as one sufficiently large contiguous block."**

That's the GATE mental model.

---

# 5. Why does Base + Bound suffer from it?

Remember Base + Bound?

A process gets:

```text
Base
+
Bound
```

And the entire process must occupy one contiguous physical region.

For example:

```text
Process P1
+-------------------+
|                   |
|       50 KB       |
|                   |
+-------------------+
```

It cannot be:

```text
+---------+
| P1 20KB |
+---------+
| P2      |
+---------+
| P1 30KB |
+---------+
```

because Base + Bound assumes one contiguous region for the process.

The lecture explicitly connects external fragmentation to this contiguous-allocation requirement.

---

# 6. Segmentation also suffers from it

This is a **GATE trap**.

You might think:

> "Segmentation solves the contiguous allocation problem."

Not completely.

It solves:

> **The entire process doesn't have to be contiguous.**

But each individual segment still needs contiguous physical memory.

Suppose:

```text
Process

Code  = 20 KB
Heap  = 40 KB
Stack = 10 KB
```

They can be placed separately:

```text
Physical Memory

+----------------+
| Code 20 KB     |
+----------------+
| Process B      |
+----------------+
| Heap 40 KB     |
+----------------+
| Process C      |
+----------------+
| Stack 10 KB    |
+----------------+
```

That's good.

But now suppose the heap needs to grow from:

```text
40 KB → 60 KB
```

and there isn't another 20 KB contiguous space next to it.

We may need to move the segment.

So segmentation **reduces some contiguity requirements**, but does **not eliminate external fragmentation**.

The lecture explicitly says segments are allocated contiguously and identifies external fragmentation as a remaining problem.

---

# 7. External vs Internal Fragmentation

This distinction is SUPER important.

## External fragmentation

Free space is outside allocated blocks and scattered.

```text
P1
FREE 10
P2
FREE 20
P3
FREE 15
```

Problem:

```text
FREE = scattered
```

---

## Internal fragmentation

Wasted space is **inside an allocated block**.

Example:

Process needs:

```text
70 KB
```

but allocator gives:

```text
100 KB
```

Then:

```text
+----------------------+
| Process = 70 KB      |
|----------------------|
| UNUSED = 30 KB       |
+----------------------+
```

That 30 KB is **inside the allocated region**.

That's internal fragmentation.

---

# 8. Visual Difference

### External

```text
+---------+
| Process |
+---------+
|  FREE   | ← outside allocation
+---------+
| Process |
+---------+
|  FREE   | ← outside allocation
+---------+
```

### Internal

```text
+----------------+
| Process        |
|----------------|
| UNUSED         | ← inside allocation
+----------------+
```

So remember:

> **External → outside allocated blocks**

> **Internal → inside allocated blocks**

---

# 9. A GATE-style question

Suppose physical memory is:

```text
100 KB
```

Currently allocated:

```text
P1 = 20 KB
P2 = 30 KB
P3 = 20 KB
```

Free memory is:

```text
30 KB
```

But suppose that 30 KB is split as:

```text
10 KB
+
8 KB
+
12 KB
```

Now a process requests:

```text
15 KB
```

Can it be allocated?

### Total free memory:

```text
10 + 8 + 12
= 30 KB
```

Looks sufficient.

But largest free block:

```text
12 KB
```

And:

```text
12 < 15
```

Therefore:

❌ Cannot allocate.

That's external fragmentation.

---

# 10. Here's the killer GATE insight

Suppose:

```text
Total free memory = 100 KB
```

and a process needs:

```text
80 KB
```

You **cannot determine whether allocation is possible** from total free memory alone.

You need to know:

> **How is that free memory distributed?**

For example:

### Case A

```text
80 KB FREE
20 KB FREE
```

Allocation succeeds.

### Case B

```text
40 KB FREE
30 KB FREE
30 KB FREE
```

Total:

```text
100 KB
```

But:

```text
largest hole = 40 KB
```

So an 80 KB contiguous request fails.

🔥 **Total free memory ≠ largest available contiguous block.**

---

# 11. Why does fragmentation happen?

Imagine processes arriving and leaving.

Initially:

```text
+-----------------------------+
| P1 | P2 | P3 | P4 | P5     |
+-----------------------------+
```

Now P2 terminates:

```text
+-----------------------------+
| P1 | FREE | P3 | P4 | P5   |
+-----------------------------+
```

Then P4 terminates:

```text
+-----------------------------+
| P1 | FREE | P3 | FREE | P5 |
+-----------------------------+
```

Then another process arrives that needs a block.

Over time:

```text
+-----------------------------+
| P1 | F | P3 | F | P5 | F  |
+-----------------------------+
```

The holes become scattered.

That's how external fragmentation naturally develops.

---

# 12. What is a "hole"?

In memory allocation terminology:

> A **hole** is a free contiguous region of physical memory.

Example:

```text
+-----------+
| Process A |
+-----------+
|  Hole     | ← 25 KB
+-----------+
| Process B |
+-----------+
|  Hole     | ← 40 KB
+-----------+
```

So if a question says:

> "There are holes of sizes 10 KB, 20 KB and 30 KB..."

They're talking about free memory regions.

---

# 13. The deeper connection to the lecture

The lecture's progression is actually beautiful:

### Base + Bound

Entire process must be contiguous.

```text
+-----------------------+
|       PROCESS         |
+-----------------------+
```

Problem:

```text
External fragmentation
```

### Segmentation

Break process into pieces.

```text
Code
Heap
Stack
```

Now the **whole process** doesn't need to be contiguous.

But:

```text
Each segment → contiguous
```

So external fragmentation still exists.

That's why the lecture ends by saying segmentation is "okay... but" and then introduces the problems of relocation and external fragmentation.

---

# 14. ⭐ The sentence I want you to remember

If GATE asks:

> **What is external fragmentation?**

Your brain should immediately say:

> **Free memory is available, but it is divided into separate non-contiguous holes, so a request requiring a sufficiently large contiguous block may fail.**

And the classic example:

```text
Free:
20 KB + 30 KB + 40 KB = 90 KB

Request:
60 KB

Total free = 90 KB ✔

Largest contiguous block = 40 KB ❌

Therefore → External Fragmentation
```

---

## One final connection

This is also why **paging** becomes such a big deal later.

Base + Bound:

```text
Process
████████████████
      ONE BLOCK
```

Segmentation:

```text
Code ███
Heap █████
Stack ██
```

Paging takes the idea much further by allowing memory to be broken into **fixed-size pieces**, so the physical placement doesn't require a large contiguous hole.

So when you eventually study paging, don't treat it as a random new topic. Think:

> **"We are trying to solve the contiguous-allocation/external-fragmentation problem that Base + Bound and segmentation still have."**

That's the conceptual bridge.