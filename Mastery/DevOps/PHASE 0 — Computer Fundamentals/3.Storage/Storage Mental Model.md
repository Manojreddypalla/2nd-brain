# Storage Fundamentals — DevOps

## Storage Mental Model

Before learning individual technologies, separate them into layers:

```text
Physical Storage
├── HDD
└── SSD

Combining Physical Disks
└── RAID

Network Storage
├── NAS
└── SAN

Storage Models
├── Block Storage
├── File Storage
└── Object Storage
```

> **HDD/SSD = where bits physically live**  
> **RAID = how multiple disks work together**  
> **NAS/SAN = how storage is provided over a network**  
> **Block/File/Object = how applications/systems access the data**

---

# 1. HDD — Hard Disk Drive

## What is HDD?

**HDD (Hard Disk Drive)** stores data magnetically on rotating physical disks called **platters**.

```text
        Actuator Arm
             ↓
      ┌─────────────┐
      │   Platter   │  ↻
      │      ●      │
      └─────────────┘
             ↑
          Spindle
```

A mechanical read/write head moves across the platter.

---

## How HDD Works

Data is organized into areas such as:

```text
Platter
 ↓
Tracks
 ↓
Sectors
 ↓
Data
```

To read data:

```text
Move head
   ↓
Wait for platter rotation
   ↓
Find sector
   ↓
Read data
```

Because physical movement is involved, HDDs have relatively high latency.

---

## HDD Characteristics

### Advantages

- Cheap per GB
    
- Large capacities
    
- Good for bulk storage
    
- Good for backups
    
- Good for archival data
    

### Disadvantages

- Mechanical parts
    
- Slower random access
    
- Higher latency
    
- More vulnerable to physical shock
    
- Usually more power/noise than SSDs
    

---

## DevOps Use Cases

Common for:

- Backup servers
    
- NAS
    
- Large archives
    
- Log archives
    
- Media storage
    
- Cold/warm data
    

---

# 2. SSD — Solid State Drive

## What is SSD?

**SSD (Solid State Drive)** stores data using flash memory.

There are no moving mechanical parts.

```text
HDD

Motor → Platter → Head → Data


SSD

Controller → Flash Memory → Data
```

This drastically reduces access latency.

---

# SSD Interfaces

## SATA SSD

Uses the SATA interface.

```text
CPU
 ↓
Storage Controller
 ↓
SATA
 ↓
SSD
```

Common practical maximum:

```text
~500–550 MB/s
```

---

## NVMe SSD

**NVMe = Non-Volatile Memory Express**

NVMe SSDs commonly communicate over **PCI Express (PCIe)**.

```text
CPU
 ↓
PCIe
 ↓
NVMe SSD
```

Designed specifically for high-speed solid-state storage.

Can provide:

- Higher throughput
    
- Lower latency
    
- More parallel I/O
    
- Higher IOPS
    

than traditional SATA SSDs.

---

# IOPS

**IOPS = Input/Output Operations Per Second**

Measures how many storage operations can be performed per second.

Important for workloads with many small/random operations.

Examples:

```text
Databases
Virtual Machines
Containers
High-traffic applications
```

---

# Throughput vs IOPS vs Latency

These are different.

## Throughput

How much data can be transferred per second.

```text
MB/s
GB/s
```

Think:

> How wide is the highway?

---

## IOPS

How many individual I/O operations happen per second.

Think:

> How many cars can pass?

---

## Latency

How long one operation takes.

```text
Request → [delay] → Response
```

Think:

> How long does one car take to reach the destination?

---

# HDD vs SSD

|Feature|HDD|SSD|
|---|---|---|
|Technology|Magnetic/mechanical|Flash memory|
|Moving parts|Yes|No|
|Random access|Slower|Faster|
|Latency|Higher|Lower|
|IOPS|Lower|Higher|
|Cost/GB|Lower|Higher|
|Noise|Possible|Silent|
|Large bulk storage|Excellent|More expensive|
|Databases/VMs|Less ideal|Excellent|

---

# 3. RAID

## What is RAID?

**RAID = Redundant Array of Independent Disks**

RAID combines multiple physical disks into a logical storage arrangement.

Goals may include:

- Performance
    
- Redundancy
    
- Capacity
    
- Availability
    

depending on RAID level.

```text
Disk 1 ─┐
Disk 2 ─┤
Disk 3 ─┼── RAID ──> Logical Storage
Disk 4 ─┘
```

---

# Important Concepts

## Striping

Data is distributed across multiple disks.

```text
Data:

A B C D

Disk 1 → A C
Disk 2 → B D
```

Benefit:

> Performance

But striping alone provides no redundancy.

---

## Mirroring

The same data exists on multiple disks.

```text
Disk 1 → A B C
Disk 2 → A B C
```

Benefit:

> Redundancy

---

## Parity

Extra calculated information is stored so lost data can be reconstructed after certain disk failures.

Conceptually:

```text
Data + Data → Parity Information
```

Parity provides redundancy without storing a complete second copy of everything.

---

# RAID 0 — Striping

Minimum:

```text
2 disks
```

Example:

```text
        Data
         ↓

Disk 1      Disk 2

  A           B
  C           D
  E           F
```

### Advantages

- High performance
    
- Uses total disk capacity
    

### Disadvantage

❌ No redundancy

If one disk fails:

```text
Array → FAILED / data lost
```

### Capacity

For equal-sized disks:

```text
Usable = N × Disk Size
```

Example:

```text
2 × 1 TB = 2 TB
```

---

# RAID 1 — Mirroring

Minimum:

```text
2 disks
```

```text
Disk 1        Disk 2

A B C         A B C
```

Same data stored on both.

### Advantages

- Simple redundancy
    
- Can survive failure of one disk in a two-disk mirror
    
- Reads may benefit from multiple copies
    

### Disadvantages

- Significant capacity cost
    

Example:

```text
2 × 1 TB disks

Raw = 2 TB
Usable ≈ 1 TB
```

---

# RAID 5 — Striping + Distributed Parity

Minimum:

```text
3 disks
```

Conceptually:

```text
Disk 1     Disk 2     Disk 3

Data       Data       Parity
Data       Parity     Data
Parity     Data       Data
```

Can tolerate:

```text
1 disk failure
```

### Capacity

For equal-sized disks:

```text
Usable = (N - 1) × Disk Size
```

Example:

```text
4 × 2 TB

Usable = 6 TB
```

### Advantages

- Better capacity efficiency than mirroring
    
- Fault tolerance
    

### Disadvantages

- Parity calculation overhead
    
- Writes can be slower
    
- Rebuilds can be slow/risky on large arrays
    

---

# RAID 6

Similar to RAID 5 but stores enough parity information to tolerate:

```text
2 disk failures
```

Minimum:

```text
4 disks
```

Capacity:

```text
Usable = (N - 2) × Disk Size
```

Example:

```text
6 × 2 TB

Usable = 8 TB
```

Better failure tolerance than RAID 5, with additional capacity/write overhead.

---

# RAID 10

Also called:

```text
RAID 1+0
```

Combines:

```text
Mirroring + Striping
```

Example:

```text
        RAID 0
       /      \
    Mirror    Mirror

   D1   D2    D3   D4
    ↕          ↕
 RAID 1      RAID 1
```

Minimum:

```text
4 disks
```

### Advantages

- High performance
    
- Good redundancy
    
- Fast rebuild characteristics compared with parity RAID
    

### Disadvantages

- About 50% capacity efficiency with standard mirrors
    
- More disks required
    

Commonly useful for:

- Databases
    
- Virtualization
    
- High-I/O workloads
    

---

# RAID Comparison

|RAID|Minimum Disks|Redundancy|Performance|Capacity Efficiency|
|---|--:|---|---|---|
|RAID 0|2|❌|Very High|100%|
|RAID 1|2|✅|Good|~50%|
|RAID 5|3|1 disk|Good|`(N-1)/N`|
|RAID 6|4|2 disks|Good|`(N-2)/N`|
|RAID 10|4|✅|Very High|~50%|

> **RAID is NOT a backup.**

RAID may protect against disk failure.

It does not inherently protect against:

- Accidental deletion
    
- Ransomware
    
- File corruption
    
- Application mistakes
    
- Entire server loss
    
- Fire/theft
    
- Administrative mistakes
    

You still need backups.

---

# 4. NAS — Network Attached Storage

## What is NAS?

**NAS = Network Attached Storage**

A NAS provides **file-level storage over a network**.

```text
Laptop ─────┐
Server ─────┼── LAN ──> NAS
Desktop ────┘
```

Clients access:

```text
Files
Folders
Shares
```

rather than directly controlling raw disks.

---

# NAS Protocols

Common protocols:

```text
SMB
NFS
```

## SMB

**Server Message Block**

Common in Windows environments.

Example:

```text
\\nas-server\shared
```

## NFS

**Network File System**

Common in Linux/Unix environments.

Example:

```text
/mnt/shared
```

---

# NAS Architecture

```text
Client
  ↓
File Protocol
(SMB / NFS)
  ↓
Network
  ↓
NAS Server
  ↓
Filesystem
  ↓
Storage Disks
```

The NAS itself manages its filesystem.

The client says:

> Give me `/projects/app.txt`.

---

# NAS Use Cases

- Shared team files
    
- Backups
    
- Media storage
    
- Home labs
    
- Shared Linux directories
    
- Centralized file storage
    

---

# 5. SAN — Storage Area Network

## What is SAN?

**SAN = Storage Area Network**

A SAN provides **block-level storage over a dedicated or specialized storage network**.

The server sees storage roughly like a locally attached disk.

```text
Server
  ↓
SAN Network
  ↓
Storage Array
```

The server may receive a block device/LUN and create its own filesystem on top.

---

# SAN Protocols

Common technologies:

```text
Fibre Channel
iSCSI
```

## iSCSI

**Internet Small Computer Systems Interface**

Carries SCSI storage commands over IP networks.

Conceptually:

```text
Server
 ↓
iSCSI
 ↓
IP Network
 ↓
Storage
```

---

# NAS vs SAN

This difference is extremely important.

## NAS

Provides:

```text
FILES
```

Client:

> Give me `/photos/image.jpg`.

## SAN

Provides:

```text
BLOCKS / virtual disk
```

Server:

> Give me blocks from this storage device.

Then the server manages the filesystem.

---

# NAS vs SAN Comparison

|Feature|NAS|SAN|
|---|---|---|
|Storage level|File|Block|
|Typical protocols|SMB, NFS|iSCSI, Fibre Channel|
|Client sees|Files/folders|Disk/block device|
|Complexity|Lower|Higher|
|Cost|Usually lower|Often higher|
|Common use|File sharing|Enterprise storage, DB/VM workloads|

---

# 6. Block Storage

## What is Block Storage?

Block storage divides storage into fixed-size **blocks**.

```text
Storage

[Block 1]
[Block 2]
[Block 3]
[Block 4]
[Block 5]
```

The operating system sees something resembling a disk.

Example Linux device:

```text
/dev/sdb
```

You can then create:

```text
Block Device
    ↓
Partition
    ↓
Filesystem
    ↓
Directories
    ↓
Files
```

For example:

```bash
mkfs.ext4 /dev/sdb
```

The OS creates an `ext4` filesystem on the block device.

---

# Block Storage Characteristics

- Low latency
    
- High performance
    
- Random read/write support
    
- OS can choose filesystem
    
- Good for transactional workloads
    

Common use cases:

- Databases
    
- Virtual machines
    
- Boot disks
    
- Kubernetes persistent volumes
    
- Enterprise applications
    

---

# Cloud Example

Cloud virtual machine disks are commonly block storage.

Conceptually:

```text
EC2 / VM
   ↓
Virtual Disk
   ↓
Block Storage
```

For example, [Amazon EBS](https://aws.amazon.com/ebs/?utm_source=chatgpt.com) provides block storage for AWS workloads.

---

# 7. Object Storage

## What is Object Storage?

Object storage stores data as independent **objects** instead of exposing a traditional disk/block device.

Each object generally contains:

```text
Object
├── Data
├── Metadata
└── Unique Key / Identifier
```

Example:

```text
Bucket
│
├── images/cat.jpg
├── videos/demo.mp4
└── backups/db-backup.tar.gz
```

---

# How Object Storage is Accessed

Usually through:

```text
HTTP/API
```

rather than mounting a traditional disk and manipulating raw blocks.

Conceptually:

```text
Application
    ↓
HTTP/API
    ↓
Object Storage
    ↓
Bucket
    ↓
Object
```

---

# Object Storage Mental Model

Block storage says:

> Here is a disk. Build a filesystem on it.

Object storage says:

> Give me the object whose key is `images/logo.png`.

---

# Object Storage Advantages

- Massive scalability
    
- Rich metadata
    
- High durability designs
    
- Good for unstructured data
    
- Easy HTTP/API access
    
- Excellent for distributed/cloud systems
    

---

# Object Storage Use Cases

Excellent for:

- Images
    
- Videos
    
- Backups
    
- Logs
    
- Static website assets
    
- Data lakes
    
- ML datasets
    
- Software artifacts
    

Examples include [Amazon S3](https://aws.amazon.com/s3/?utm_source=chatgpt.com), Azure Blob Storage, and Google Cloud Storage.

---

# Object Storage Limitation

Object storage is generally **not a direct replacement for a normal local filesystem or database disk**.

For example, a relational database usually wants:

```text
Low-latency random block access
```

rather than repeatedly treating database pages as independent remote objects.

So:

```text
Database disk → Block Storage ✅

Database backups → Object Storage ✅
```

---

# 8. File Storage

Even though it wasn't in the original list, understand this because it connects NAS, block and object storage.

File storage organizes data hierarchically:

```text
/
├── home/
│   └── user/
│       └── notes.txt
│
├── var/
└── etc/
```

Structure:

```text
Directories
    ↓
Subdirectories
    ↓
Files
```

Common filesystems:

```text
ext4
XFS
NTFS
ZFS
```

NAS commonly exposes file storage over a network.

---

# Block vs File vs Object Storage

This is the most important storage comparison.

## Block Storage

```text
Disk
 ↓
Blocks
 ↓
Filesystem
 ↓
Files
```

Think:

> Virtual hard drive.

---

## File Storage

```text
Directory
 ↓
Folder
 ↓
File
```

Think:

> Shared folder.

---

## Object Storage

```text
Bucket
 ↓
Object Key
 ↓
Object + Metadata
```

Think:

> Massive API-accessed object collection.

---

# Comparison

|Feature|Block|File|Object|
|---|---|---|---|
|Access|Blocks|Files|API/Objects|
|Structure|Raw blocks|Hierarchical|Flat-ish key namespace|
|Filesystem|Client creates/manages|Storage system manages|Not traditional|
|Performance|Very high|Good|Optimized for scale/durability|
|Scalability|Good|Good|Massive|
|Common workload|DB/VM|Shared files|Media/backups/data lakes|

---

# Real DevOps Architecture

Imagine a web application:

```text
                    Application
                         │
          ┌──────────────┼──────────────┐
          ↓              ↓              ↓

       Database       Shared Files     Images
          ↓              ↓              ↓

        Block           NAS           Object
       Storage        NFS/SMB         Storage
          ↓                             ↓
      SSD / RAID                     S3-like
```

Why?

### Database

Needs:

```text
Low latency
Random reads/writes
High IOPS
```

→ Block storage

### Shared Files

Multiple systems need filesystem-style access:

```text
/team/file.txt
```

→ NAS/File storage

### Images / Videos / Backups

Need:

```text
Huge scalability
Durability
API access
```

→ Object storage

---

# Storage Stack — Full Picture

Now connect everything:

```text
Application
     ↓
Storage Model
     │
     ├── Block
     ├── File
     └── Object
     ↓
Storage System
     │
     ├── Local Disk
     ├── NAS
     ├── SAN
     └── Cloud Storage
     ↓
Disk Organization
     ↓
    RAID
     ↓
Physical Devices
     │
     ├── HDD
     └── SSD
```

This is the key abstraction.

---

# DevOps Commands

## View disks

```bash
lsblk
```

`lsblk` = **List Block Devices**

Shows:

```text
Disk
Partitions
Mount points
```

---

## Check disk space

```bash
df -h
```

- `df` → Disk Free
    
- `-h` → Human-readable sizes
    

---

## Check directory size

```bash
du -sh /var/log
```

- `du` → Disk Usage
    
- `-s` → Summary
    
- `-h` → Human readable
    

---

## View mounted filesystems

```bash
mount
```

or:

```bash
findmnt
```

---

## Check filesystem/device information

```bash
lsblk -f
```

Shows information such as:

```text
Filesystem
UUID
Mount point
```

---

# DevOps Troubleshooting Mental Model

If application reports:

```text
No space left on device
```

Think:

```text
Application
    ↓
Filesystem full?
    ↓
df -h
```

But if `df` looks okay, investigate other limits such as **inodes**:

```bash
df -i
```

---

If storage is slow:

```text
Application Slow
      ↓
Storage latency?
      ↓
High I/O?
      ↓
Disk overloaded?
      ↓
Network storage slow?
```

Important metrics:

```text
Latency
IOPS
Throughput
Disk utilization
Queue depth
Free capacity
```

---

# Important DevOps Distinctions

## HDD vs SSD

Question:

> What physical storage technology?

```text
HDD / SSD
```

---

## RAID

Question:

> How are multiple disks organized?

```text
RAID
```

---

## NAS vs SAN

Question:

> How is remote storage exposed?

```text
NAS → Files

SAN → Blocks
```

---

## Block vs Object

Question:

> How does the application/system interact with storage?

```text
Block → Disk-like

Object → API/object-like
```

---

# Quick Revision

```text
HDD
→ Magnetic mechanical storage
→ Cheap, large capacity
→ Higher latency


SSD
→ Flash storage
→ Fast, low latency
→ High IOPS


RAID
→ Multiple disks combined
→ Performance and/or redundancy


NAS
→ Network file storage
→ SMB / NFS
→ Files and folders


SAN
→ Network block storage
→ iSCSI / Fibre Channel
→ Server sees disk-like storage


Block Storage
→ Raw blocks / virtual disk
→ DB, VM, boot disks


File Storage
→ Files + directories
→ Shared files


Object Storage
→ Objects + metadata + key
→ HTTP/API
→ Images, backups, logs, data lakes
```

---

# Interview Cheat Sheet

**What is RAID?**

RAID combines multiple physical disks to provide performance, redundancy, capacity, or a combination depending on the RAID level.

**Is RAID a backup?**

No. RAID primarily helps with disk availability/failure scenarios; backups protect against broader forms of data loss.

**NAS vs SAN?**

```text
NAS → File-level storage
SAN → Block-level storage
```

**Block vs Object Storage?**

```text
Block
→ Disk-like
→ Low-latency random I/O
→ Databases/VMs

Object
→ API-accessed objects
→ Massive scalability
→ Backups/media/logs
```

**Why SSD for databases?**

Databases frequently perform random I/O and benefit heavily from low latency and high IOPS.

**Why object storage for backups?**

Backups are usually large objects that benefit from scalable, durable, cost-efficient storage rather than low-latency random block access.

---

# One-Line Mental Model

> **HDD/SSD = hardware → RAID = combine disks → NAS/SAN = provide storage over network → Block/File/Object = how storage is exposed and accessed.**

#DevOps #Storage #Linux #RAID #NAS #SAN #SSD #HDD #ObjectStorage #BlockStorage