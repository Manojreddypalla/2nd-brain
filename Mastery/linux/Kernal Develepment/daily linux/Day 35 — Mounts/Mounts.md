# 🐧 Linux Day 35 — Mounts | Mini Notes

## 1. Core Idea

Linux presents **one filesystem tree**:

```text
/
├── home
├── proc
├── sys
├── dev
└── mnt
```

But this tree can contain **many different filesystems**.

> **Mount = attach a filesystem/subtree to a directory in the existing filesystem tree.**

```text
Filesystem A
    /
    └── mnt
         │
         │ mount
         ▼
    Filesystem B
         ├── a.txt
         └── b.txt
```

Path lookup crossing `/mnt` now continues inside **Filesystem B**.

---

## 2. Filesystem ≠ Disk

A filesystem doesn't have to live on physical storage.

```text
ext4    → usually disk/SSD
XFS     → usually disk/SSD

tmpfs   → RAM-backed

procfs  → kernel/process information
          exposed through /proc

sysfs   → devices/drivers/kernel objects
          exposed through /sys
```

This is why:

```bash
cat /proc/cpuinfo
```

looks like reading a normal file even though the information is provided by the kernel.

---

## 3. Mount Point

A **mount point** is the directory where another filesystem/tree is attached.

```text
/
└── mnt
     │
     │ ← mount point
     ▼
   USB filesystem
     ├── photo.jpg
     └── notes.txt
```

From userspace it looks like one continuous tree:

```text
/mnt/photo.jpg
```

---

## 4. Mounts + Path Resolution

From Day 34:

```text
/a/b/c/file
```

Linux walks each component.

Now add mounts:

```text
/
↓
mnt
↓
[MOUNT BOUNDARY]
↓
mounted filesystem
↓
folder
↓
file
```

So pathname resolution can **cross filesystem boundaries automatically**.

---

## 5. Mounts Can Hide Existing Files

Suppose:

```text
/mnt/test
├── secret.txt
└── notes.txt
```

Mount another filesystem at `/mnt/test`:

```text
/mnt/test
├── a.txt
└── b.txt
```

`secret.txt` wasn't deleted.

The mounted filesystem simply **obscures the original directory contents**.

Unmount it and the old contents become visible again.

---

## 6. Bind Mount

A bind mount exposes an **existing directory/subtree at another path**.

```bash
sudo mount --bind SOURCE TARGET
```

Example:

```text
/tmp/source
    └── file.txt
         ▲
         │
    same subtree
         │
         ▼
/tmp/view
    └── file.txt
```

> **Bind mount ≠ copy.**

Unmount:

```bash
sudo umount TARGET
```

---

## 7. Mount Namespace

Different processes can have **different views of the mount tree**.

```text
             Linux Kernel
              /        \
             /          \
      Process A        Process B
          │                │
     Mount NS A        Mount NS B
          │                │
          /                /
       ├─home           ├─app
       └─proc           └─proc
```

This is a major building block of **containers**.

```text
Same kernel
+
different mount namespaces
=
different filesystem views
```

---

# 🔧 Important Commands

```bash
findmnt
```

→ Show mount tree.

```bash
findmnt /proc
```

→ Find which filesystem provides a path.

```bash
df -T
```

→ Filesystems + types + disk usage.

```bash
cat /proc/self/mountinfo
```

→ Kernel's mount information visible to the current process.

```bash
sudo mount --bind SOURCE TARGET
```

→ Expose a subtree at another location.

```bash
sudo umount TARGET
```

→ Remove mount.

---

# 🧠 30-Second Revision

```text
Mount
→ attach filesystem/tree to directory

Mount point
→ directory where it's attached

Linux tree
→ can contain multiple filesystems

procfs
→ process/kernel info

sysfs
→ devices/drivers/kernel objects

tmpfs
→ RAM-backed

Bind mount
→ same subtree visible at another path

Mount namespace
→ different processes can see different mount trees
```

### ⭐ Mental Model

```text
PATH
 ↓
VFS pathname walk
 ↓
directory
 ↓
MOUNT POINT
 ↓
cross filesystem boundary
 ↓
mounted filesystem
 ↓
continue pathname walk
 ↓
final inode
```

So Days **33–35** fit together as:

```text
VFS
 ↓
How Linux represents files

Path Resolution
 ↓
How Linux finds files

Mounts
 ↓
How that path can cross different filesystems
```

**Next: Page Cache → how file data actually moves between storage and RAM.**