# 🐧 Linux Internals — Day 28: Virtual Memory & Demand Paging

> 🎯 **Goal:** Understand how Linux makes programs think they have lots of memory while only keeping the pages they actually need in RAM.

---

## 1. The Core Problem

Suppose your machine has:

```text
RAM = 8 GB

Browser     → 3 GB
Game        → 4 GB
VS Code     → 1 GB
Other apps  → 2 GB
```

That's potentially more memory than available RAM.

Linux does **not** simply load every byte of every program into RAM.

Instead, it uses:

> **Virtual Memory + Paging + Demand Paging**

---

# 2. Virtual Memory

Every process gets its own **virtual address space**.

For example:

```text
Process A                    Process B

0x1000 → Code                0x1000 → Code
0x2000 → Data                0x2000 → Data
0x3000 → Heap                0x3000 → Heap
...                          ...
```

Both processes might use virtual address `0x3000`, but those addresses can map to completely different locations in physical RAM.

```text
Virtual Memory                    Physical RAM

Process A: 0x3000 ───────────────→ Frame 50

Process B: 0x3000 ───────────────→ Frame 821
```

So:

> **Virtual address ≠ physical RAM address.**

---

# 3. Who Translates the Address?

The CPU contains hardware called the **MMU**.

**MMU = Memory Management Unit**

Its job is essentially:

```text
Virtual Address
      ↓
     MMU
      ↓
Page Table
      ↓
Physical Address
      ↓
RAM
```

The OS manages the page tables; the MMU uses them during address translation.

---

# 4. Pages and Frames

Linux doesn't normally manage process memory byte-by-byte.

Virtual memory is divided into:

> **Pages**

Physical RAM is divided into:

> **Frames**

Common page size on many systems:

```text
4 KB
```

Conceptually:

```text
Virtual Memory

Page 0
Page 1
Page 2
Page 3
Page 4
   │
   │ Page Table
   ▼
Physical RAM

Frame 91
Frame 12
Frame 700
...
```

A virtual page can be mapped to a physical frame.

---

# 5. Page Table

Each process has memory mappings represented through page-table structures managed by the kernel.

Think of a page table as a mapping:

```text
Virtual Page          Physical Frame

Page 0   ─────────→   Frame 42
Page 1   ─────────→   Frame 900
Page 2   ─────────→   Frame 71
Page 3   ─────────→   NOT PRESENT
```

This provides two huge benefits.

### Isolation

```text
Process A memory  ✗  Process B memory
```

One process normally can't simply access another process's memory.

### Flexibility

Virtual memory can appear continuous:

```text
Page 1
Page 2
Page 3
```

while physical RAM may be scattered:

```text
Frame 831
Frame 27
Frame 502
```

The mapping hides this from the application.

---

# 6. Demand Paging ⭐

This is today's main concept.

Suppose you start a large program.

Linux doesn't necessarily read every executable/library page into RAM immediately.

Instead:

> **Load/map pages as they're actually needed.**

```text
Program starts
      ↓
Virtual address space established
      ↓
Some pages aren't resident yet
      ↓
Program accesses one
      ↓
PAGE FAULT
      ↓
Kernel resolves it
      ↓
Program continues
```

This is **Demand Paging**.

---

# 7. What is a Page Fault?

Suppose the CPU accesses:

```text
Virtual Page 50
```

The translation/mapping says the required page isn't currently available in the way needed.

The CPU raises a **page fault exception**.

```text
CPU accesses address
        ↓
Can access it now?
        │
   NO ──┘
        ↓
    Page Fault
        ↓
Kernel handles it
```

The kernel determines **why** the fault occurred.

If it's valid, Linux fixes the situation and execution continues.

So:

> ⚠️ **Page fault does NOT automatically mean a crash/error.**

Page faults are a normal part of virtual memory.

---

# 8. Minor Page Fault

A **minor fault** can be resolved **without reading the required page contents from storage**.

For example, the data may already be present in memory/page cache and only need to be mapped appropriately.

```text
Process
   ↓
Page Fault
   ↓
Page already available in memory
   ↓
Update mapping/page table
   ↓
Continue
```

Generally relatively cheap.

---

# 9. Major Page Fault

A **major fault** requires storage I/O to obtain the page contents.

```text
Process
   ↓
Page Fault
   ↓
Page needs storage I/O
   ↓
SSD / Disk
   ↓
Load page into RAM
   ↓
Update mapping
   ↓
Continue
```

Storage is dramatically slower than RAM, so major faults can be expensive.

Remember:

```text
Minor → no required storage read

Major → storage I/O required
```

---

# 10. Where Does Disk Come Into This?

Pages may originate from executable files, shared libraries, memory-mapped files, or swap-backed memory.

Example:

```text
Program executable on SSD
          │
          │ page needed
          ▼
         RAM
          │
          ▼
         CPU
```

Linux only needs to bring relevant pages into RAM as they're accessed.

---

# 11. What Happens When RAM Gets Full?

Linux can reclaim memory.

For file-backed pages that haven't changed, Linux may simply discard them because they can be read again from the file later.

Anonymous memory may potentially be moved to **swap** if swap is enabled and the kernel decides to use it.

```text
RAM pressure
     ↓
Kernel chooses pages
     ↓
Reclaim
   /       \
file page   anonymous page
   ↓             ↓
discard       possibly swap
```

Later, if a swapped-out page is needed:

```text
Process accesses page
       ↓
Page fault
       ↓
Read from swap
       ↓
RAM
       ↓
Continue
```

---

# 12. Virtual Memory vs RAM Usage

Suppose a program reserves/maps:

```text
10 GB Virtual Memory
```

That does **not necessarily mean**:

```text
10 GB physical RAM used
```

Maybe only:

```text
1.2 GB
```

is currently resident.

Think:

```text
Virtual Address Space
████████████████████████████ 10 GB

Actually resident in RAM
████ 1.2 GB
```

This is one reason process tools distinguish virtual memory from resident memory.

---

# 13. VSZ vs RSS

You'll often see:

### VSZ / VIRT

Amount of **virtual address space** associated with the process.

### RSS / RES

**Resident Set Size** — memory pages currently resident in physical RAM for the process.

Example:

```text
Process

VIRT → 8 GB
RES  → 900 MB
```

This isn't inherently suspicious.

The process can have a large virtual address space while only part of it is physically resident.

---

# 14. `mmap()` Connection

You've seen `mmap()` before.

It creates memory mappings in a process's virtual address space.

```text
File
 │
 │ mmap()
 ▼
Process Virtual Memory
```

The contents don't necessarily all need to be read immediately.

A typical flow can be:

```text
mmap(file)
    ↓
Create virtual mapping
    ↓
Process accesses page
    ↓
Page fault if page isn't resident
    ↓
Kernel obtains/maps page
    ↓
Continue
```

That's one reason `mmap()` and demand paging fit together so naturally.

---

# 15. Shared Libraries

Imagine 20 programs all use the same shared library.

```text
Program A ─┐
Program B ─┤
Program C ─┼──→ libc code pages in RAM
Program D ─┘
```

Read-only library code pages can be shared between processes rather than requiring identical physical copies for every process.

Each process still has its own **virtual mapping**.

---

# 16. TLB — The Speed Trick

If every memory access required a full page-table walk, address translation could become expensive.

CPUs therefore have a cache called:

> **TLB = Translation Lookaside Buffer**

It caches recent virtual → physical translations.

```text
Virtual Address
      ↓
     TLB
      │
      ├── HIT → translation found quickly
      │
      └── MISS
            ↓
        Page-table walk
            ↓
        Cache translation
```

Think:

> **TLB = cache for address translations.**

---

# 17. Full Flow ⭐

Suppose your program executes:

```c
x = array[i];
```

At the hardware/OS level, conceptually:

```text
Program generates
Virtual Address
      ↓
     TLB
      ↓
Translation available?
   │           │
 YES           NO
   │           ↓
   │      Page-table walk
   │           ↓
   │      Mapping valid/present?
   │          │
   │      ┌───┴────┐
   │     YES       NO
   │      │         ↓
   │      │     Page Fault
   │      │         ↓
   │      │    Kernel handles
   │      │         ↓
   │      │    mapping/page ready
   │      │
   └──────┴─────────────→ Physical RAM
                            ↓
                           CPU
```

That's the picture worth understanding.

---

# 🔗 Connect It With Previous Topics

### Processes

```text
Process
   ↓
Own virtual address space
```

### System Calls

Calls such as:

```text
mmap()
brk()
```

interact with the process's virtual memory layout.

### Filesystem

Executable and library pages can be backed by files:

```text
Executable
     ↓
Virtual mapping
     ↓
Demand paging
     ↓
RAM when needed
```

### Performance

Lots of:

```text
Major Page Faults
       ↓
Storage I/O
       ↓
Application slowdown
```

---

# 🧠 Final Mental Model

Imagine **RAM is your study desk**.

Your SSD is your bookshelf.

You own 100 books, but you don't put all 100 books on the desk.

You bring over what you're currently using.

```text
📚 Storage
    │
    │ needed page
    ▼
🪑 RAM
    │
    ▼
🧠 CPU
```

Need something that isn't currently available?

```text
Need page
   ↓
Page Fault
   ↓
Kernel obtains/maps it
   ↓
Continue
```

That's **demand paging**.

---

# ⚡ 2-Minute Revision

**Virtual Memory** → each process gets its own virtual address space.

**Page** → fixed-size block of virtual memory.

**Frame** → corresponding block of physical RAM.

**Page Table** → maps virtual pages → physical frames.

**MMU** → CPU hardware performing virtual-address translation using page tables.

**TLB** → cache of recent address translations.

**Demand Paging** → pages are brought/mapped as needed instead of eagerly loading everything.

**Page Fault** → CPU exception when an attempted virtual-memory access requires OS handling.

**Minor Fault** → resolved without reading page contents from storage.

**Major Fault** → requires storage I/O.

**VIRT/VSZ** → virtual address-space usage.

**RES/RSS** → pages currently resident in RAM.

**Swap** → disk-backed area Linux can use for certain memory pages under memory-management decisions.

### ⭐ Remember this chain

```text
Process
   ↓
Virtual Address
   ↓
MMU + TLB
   ↓
Page Table
   ↓
Physical Frame
   ↓
RAM

If required page isn't available
        ↓
    Page Fault
        ↓
Kernel resolves it
        ↓
Program continues
```

If that flow is clear, you've got essentially everything Day 28 is trying to teach you.