# 🐧 Linux Internals — Day 36: Page Cache

## 1. What is Page Cache?

**Page Cache** = RAM used by the Linux kernel to cache **file data**.

Instead of repeatedly reading the same data from SSD/HDD:

```text
Storage → RAM(Page Cache) → Process
```

Linux keeps recently accessed file data in RAM.

---

# 2. Read Path

When a process does:

```bash
cat file.txt
```

internally:

```text
Process
   ↓
read()
   ↓
VFS / Filesystem
   ↓
Page Cache
   ↓
Is page cached?
   │
   ├── YES → Cache Hit → RAM → Process
   │
   └── NO  → Cache Miss
                ↓
             Storage
                ↓
          Load into Page Cache
                ↓
             Process
```

### First read

```text
Process
   ↓
Page Cache MISS
   ↓
SSD
   ↓
RAM
   ↓
Process
```

### Second read

If data is still cached:

```text
Process
   ↓
Page Cache HIT
   ↓
RAM
   ↓
Process
```

So the second read may require **little or no physical storage I/O**.

---

# 3. What is a Page?

Linux manages memory in fixed-size chunks called **pages**.

Check:

```bash
getconf PAGE_SIZE
```

Common result:

```text
4096 bytes = 4 KiB
```

A file can conceptually be viewed as:

```text
File
┌────────────┐
│ Page 0     │
├────────────┤
│ Page 1     │
├────────────┤
│ Page 2     │
├────────────┤
│ Page 3     │
└────────────┘
```

Linux can cache the relevant pages rather than thinking of the whole file as one object.

---

# 4. Why Linux Uses RAM for Cache

Think:

```text
Empty RAM
→ doing nothing

Cached RAM
→ speeding up file access
```

Therefore:

> **Low `free` RAM does NOT automatically mean you're running out of memory.**

Check:

```bash
free -h
```

Important fields:

```text
free
buff/cache
available
```

### Key distinction

```text
free
→ completely unused RAM

buff/cache
→ RAM being used for caching

available
→ estimated RAM available for applications
   without causing serious memory pressure
```

For normal system inspection:

> **`available` is usually more useful than just looking at `free`.**

---

# 5. Cache is Reclaimable

Suppose:

```text
16 GB RAM

Applications → 6 GB
Cache        → 8 GB
Free         → 2 GB
```

That doesn't mean applications only have 2 GB left.

If a program needs more memory:

```text
Application requests RAM
        ↓
Kernel needs memory
        ↓
Reclaim suitable cached pages
        ↓
RAM becomes available
```

So cache is useful **but reclaimable when appropriate**.

---

# 6. Inspect Page-Cache Related Memory

```bash
grep -E '^(Cached|Buffers|MemFree|MemAvailable):' /proc/meminfo
```

Remember the connection:

```text
/proc
 ↓
kernel exposes runtime information
 ↓
/proc/meminfo
 ↓
memory state
```

---

# 7. Read Experiment

Create a 256 MiB file:

```bash
dd if=/dev/zero of=/tmp/cache-test.bin bs=1M count=256 status=progress
```

Meaning:

```text
dd
→ copy/convert data

if
→ input file

/dev/zero
→ generates zero bytes

of
→ output file

bs=1M
→ block size = 1 MiB

count=256
→ 256 blocks
```

Then:

```bash
time cat /tmp/cache-test.bin > /dev/null
```

`/dev/null` discards the output.

So:

```text
file
 ↓
read()
 ↓
kernel
 ↓
/dev/null
```

Read again:

```bash
time cat /tmp/cache-test.bin > /dev/null
```

Conceptually:

```text
First read
Storage → Page Cache → Process

Second read
Page Cache → Process
```

Don't rely purely on timing as proof; modern SSDs and other caching layers can make tiny benchmarks noisy.

---

# 8. Observe Storage I/O

Use:

```bash
vmstat 1
```

Important columns:

```text
bi → blocks received from block devices
bo → blocks sent to block devices
```

Mental connection:

```text
Program
   ↓
read/write
   ↓
Kernel
   ↓
Page Cache
   ↓
Block-device I/O
```

---

# 9. Writes and Dirty Pages

This is the second major concept.

A normal buffered:

```c
write(fd, data, size);
```

doesn't necessarily mean:

```text
data is physically on SSD
```

Instead, simplified:

```text
Process
   ↓
write()
   ↓
Page Cache
   ↓
Cached page modified
   ↓
DIRTY PAGE
   ↓
Process can continue
   .
   .
   .
Kernel writeback
   ↓
Storage
   ↓
Page becomes clean
```

## Dirty Page

A **dirty page** is:

> Cached file data that has been modified in RAM but has not yet been persisted to backing storage.

---

# 10. Writeback

**Writeback** is the kernel's process of eventually writing dirty cached data to storage.

```text
Dirty Page
    ↓
Writeback
    ↓
SSD/HDD
    ↓
Clean Page
```

Inspect:

```bash
grep -E '^(Dirty|Writeback):' /proc/meminfo
```

```text
Dirty
→ modified cached data waiting for persistence

Writeback
→ data currently being written out
```

---

# 11. `write()` ≠ Durability

Very important systems concept:

```text
write()
```

and

```text
physically persisted on storage
```

are **not necessarily the same event**.

Think of three states:

```text
Application data
      ↓
   write()
      ↓
Page Cache
   DIRTY
      ↓
 writeback
      ↓
Storage
```

This distinction becomes crucial later for:

**databases → filesystems → crashes → durability → `fsync()` → journaling**

---

# 12. `sync`

Command:

```bash
sync
```

High-level idea:

```text
Dirty filesystem data
        ↓
      sync
        ↓
Kernel performs synchronization/writeback
        ↓
Backing storage
```

Don't reduce this to "`sync` instantly guarantees every physical layer has completed everything"; storage durability has more nuance. For Day 36, understand it as **requesting synchronization of pending filesystem writes**.

---

# 🧠 Core Mental Model

Put the whole day together:

```text
                  READ
                   │
                   ▼
Process → read() → VFS
                   │
                   ▼
               Page Cache
               /        \
             HIT        MISS
              │           │
             RAM       Storage
                          │
                          ▼
                      Page Cache
                          │
                          ▼
                       Process
```

And writes:

```text
                 WRITE

Process
   ↓
write()
   ↓
VFS / Filesystem
   ↓
Page Cache
   ↓
Dirty Page
   ↓
Writeback
   ↓
Storage
```

---

# 🔗 Connection to Previous Days

Your filesystem chain now makes much more sense:

```text
Process
   ↓
System Call
   ↓
File Descriptor
   ↓
VFS
   ↓
dentry / inode
   ↓
Filesystem
   ↓
Page Cache
   ↓
Block I/O
   ↓
Storage
```

The important separation is:

```text
inode
→ metadata / identity of file

dentry
→ pathname component → inode relationship

page cache
→ actual cached FILE DATA
```

So don't mentally merge **inode** and **file contents**.

---

# 🔬 Commands to Remember

```bash
# Page size
getconf PAGE_SIZE

# Memory overview
free -h

# Detailed kernel memory information
cat /proc/meminfo

# Relevant cache information
grep -E '^(Cached|Dirty|Writeback|MemAvailable):' /proc/meminfo

# Observe system activity
vmstat 1

# Request filesystem synchronization
sync
```

---

# 📝 Short Revision Notes

```text
PAGE CACHE
==========

Page Cache
→ caches file data in RAM
→ avoids repeated storage I/O

READ:

Process
 ↓
read()
 ↓
VFS / filesystem
 ↓
Page Cache
 ├─ HIT  → RAM → Process
 └─ MISS → Storage → Cache → Process


PAGE:

Linux memory is divided into pages.

Typical:
4096 bytes = 4 KiB

Check:
getconf PAGE_SIZE


WHY CACHE?

RAM >> SSD/HDD

Unused RAM can be used for useful caching.

Cache can be reclaimed when applications
need memory.

Therefore:

free RAM ≠ available RAM

Check:
free -h

MemAvailable is usually more useful.


WRITE:

Process
 ↓
write()
 ↓
Page Cache
 ↓
Dirty Page
 ↓
Writeback
 ↓
Storage


Dirty Page
→ modified cached file data
→ not yet persisted

Writeback
→ dirty data being written to storage


IMPORTANT:

write()
≠ necessarily physically persisted


Useful commands:

getconf PAGE_SIZE
free -h
cat /proc/meminfo
vmstat 1
sync
```

## The one thing to remember

If Day 36 fades from memory later, reconstruct it from this:

**Reads:**  
`Storage → Page Cache → Process`

**Repeated reads:**  
`Page Cache → Process`

**Writes:**  
`Process → Page Cache (dirty) → Writeback → Storage`

And **Day 37 `mmap()` is where this gets really interesting**: instead of thinking mainly in terms of "`read()` copies file data to my buffer," you'll connect **virtual addresses → page faults → page cache → file pages**. That's the bridge between your earlier process/memory work and these filesystem internals.