# 🐧 Linux Internals — Day 33: VFS (Virtual Filesystem)

> 🎯 **Core idea:** VFS gives applications **one common filesystem interface**, regardless of what actual filesystem is underneath.

---

# 1. The Problem VFS Solves

Your program wants to read:

```text
/home/retr0/notes.txt
```

It could live on:

```text
ext4
XFS
Btrfs
tmpfs
NFS
procfs
```

But imagine if applications had to do:

```c
ext4_open(...)
xfs_open(...)
nfs_open(...)
tmpfs_open(...)
```

That would be a nightmare.

Instead, applications use the same system-call interface:

```c
open(...)
read(...)
write(...)
close(...)
```

Linux handles the filesystem differences underneath.

That's the job of the:

> **VFS — Virtual Filesystem (or Virtual File System) layer**

---

# 2. Mental Model

Think of VFS as a **universal adapter**.

```text
                 USER SPACE

              Application
                  │
          open/read/write/close
                  │
                  ▼
══════════ System Call Boundary ══════════
                  │
                  ▼
                 VFS
                  │
       ┌──────────┼──────────┐
       ▼          ▼          ▼
      ext4       XFS       tmpfs
       │          │          │
       ▼          ▼          ▼
     Disk       Disk        RAM
```

Application:

> "Open `/home/retr0/notes.txt`."

VFS/filesystem code:

> "I'll deal with what filesystem actually implements that path."

---

# 3. Why This Abstraction is Powerful

Consider:

```bash
cat file1
cat /tmp/file2
cat /proc/cpuinfo
```

These paths could involve very different filesystem implementations.

Yet `cat` doesn't need completely different code for each one.

From the application's perspective:

```text
open()
  ↓
read()
  ↓
write()
  ↓
close()
```

The VFS layer provides the common kernel model.

---

# 4. The Four Important VFS Objects ⭐

For today, understand these:

```text
superblock
inode
dentry
struct file
```

Don't memorize kernel fields. Understand **why each concept exists**.

---

# 5. Superblock — The Filesystem

Think:

> **Superblock = mounted filesystem-level information**

Suppose:

```text
/dev/nvme0n1p2
        │
        ▼
       ext4
        │
      mounted
        │
        ▼
   VFS superblock
```

It represents information about the filesystem as a whole.

Conceptually:

```text
Superblock
   │
   ├── filesystem type
   ├── filesystem state/info
   ├── filesystem operations
   └── associated filesystem objects
```

Don't think:

> "superblock = one file"

Think:

> **"superblock represents a filesystem instance."**

---

# 6. Inode — The Actual Filesystem Object ⭐

An **inode** represents a filesystem object and its metadata.

Conceptually:

```text
inode
 │
 ├── object type
 ├── permissions
 ├── owner
 ├── size
 ├── timestamps
 └── filesystem-specific information
```

For example:

```text
inode 12345

Type        → regular file
Permissions → rw-r--r--
Owner       → retr0
Size        → 5 KB
Links       → 2
...
```

But here's today's most important idea:

# **The inode is not the filename.**

---

# 7. Filename ≠ Inode ⭐⭐⭐

Suppose you have:

```text
notes.txt
```

It's tempting to imagine:

```text
inode
 ├── filename = notes.txt
 ├── size
 └── permissions
```

That's the wrong mental model.

Instead, separate:

```text
NAME

from

OBJECT
```

Conceptually:

```text
"notes.txt"
     │
     ▼
directory entry
     │
     ▼
   inode
```

This distinction explains **hard links**.

---

# 8. Dentry — The Name/Lookup Object

**dentry = directory entry**

A dentry represents a component in pathname lookup and connects the VFS pathname/name world to filesystem objects.

Mental model:

```text
"notes.txt"
     │
     ▼
   dentry
     │
     ▼
   inode
```

Think:

> **Dentry = name/path lookup object**

while:

> **Inode = underlying filesystem object**

This distinction becomes very important tomorrow when you study pathname resolution.

---

# 9. Why Hard Links Make Sense Now

Create:

```bash
echo "Linux" > original.txt
```

Conceptually:

```text
original.txt
     │
   dentry
     │
     ▼
  inode 500
```

Now:

```bash
ln original.txt second.txt
```

You did **not copy the file data**.

Instead, you created another directory entry/name referring to the same underlying inode.

```text
original.txt ──┐
               │
               ▼
            inode 500
               ▲
               │
second.txt ────┘
```

Therefore:

```bash
echo "hello" >> second.txt
```

followed by:

```bash
cat original.txt
```

shows the modification.

Because:

```text
Two names
   ↓
Same inode
   ↓
Same underlying file object/data
```

---

# 10. Link Count

Run:

```bash
ls -li
```

You might see something conceptually like:

```text
12345 ... 2 ... original.txt
12345 ... 2 ... second.txt
```

Same:

```text
inode = 12345
```

and link count:

```text
2
```

because there are two hard-link directory entries pointing to that inode.

```text
original.txt ─┐
              ├──→ inode 12345
second.txt ───┘

link count = 2
```

---

# 11. `struct file` — An Open File Instance ⭐

Now we need another abstraction.

Suppose:

```text
inode = the underlying filesystem object
```

Why isn't that enough?

Imagine two processes:

```text
Process A → opens notes.txt

Process B → opens notes.txt
```

They're accessing the **same underlying file**.

But:

```text
Process A reading at byte 100

Process B reading at byte 500
```

Where should that per-open state live?

Not simply in the inode.

That's one reason Linux has an open-file object represented by:

> **`struct file`**

---

# 12. Mental Model of `struct file`

```text
Process
   │
   ▼
FD 3
   │
   ▼
struct file
   │
   ├── current file position
   ├── open flags/state
   └── references filesystem path/object
              │
              ▼
          dentry/inode
```

So:

```text
inode
 ↓
"What filesystem object is this?"


struct file
 ↓
"What is this open instance/state?"
```

That's the distinction.

---

# 13. Two Processes Opening the Same File

Imagine:

```text
Process A                      Process B

FD 3                           FD 7
 │                              │
 ▼                              ▼
struct file A                 struct file B
 │                              │
 position = 100                 position = 500
 │                              │
 └─────────────┬────────────────┘
               ▼
            dentry
               │
               ▼
           same inode
```

So they can share the same underlying filesystem object while maintaining independent open-file state.

---

# 14. File Descriptor Connection

You've already studied FDs.

Suppose:

```c
fd = open("notes.txt", ...);
```

Linux might return:

```text
3
```

The mental model now becomes deeper:

```text
Process
   │
   ▼
FD Table
   │
FD 3
   │
   ▼
struct file
   │
   ▼
Path / dentry
   │
   ▼
inode
   │
   ▼
Filesystem
```

The **FD is just a number in the process's descriptor table**.

It ultimately leads to the kernel's open-file object.

---

# 15. Connect With `strace`

Remember:

```bash
strace cat notes.txt
```

You saw something like:

```text
openat(... "notes.txt" ...) = 3
```

Now you understand more of what happened conceptually:

```text
cat
 │
 │ openat()
 ▼
Kernel
 │
 ▼
Path lookup through VFS
 │
 ▼
Find filesystem object
 │
 ▼
Create/reference open-file state
 │
 ▼
Install FD in process
 │
 ▼
return 3
```

Then:

```text
read(3)
```

means Linux uses FD `3` to find the corresponding open-file state and perform the read.

---

# 16. `/proc/<PID>/fd` Connection

You previously used:

```bash
ls -l /proc/$$/fd
```

Suppose:

```bash
exec 3<original.txt
```

Now:

```bash
ls -l /proc/$$/fd
```

may show:

```text
3 -> /tmp/vfs-lab/original.txt
```

Conceptually:

```text
Shell Process
     │
FD table
     │
FD 3
     │
     ▼
struct file
     │
     ▼
VFS/path
     │
     ▼
inode
```

Close it:

```bash
exec 3<&-
```

Now FD 3 disappears from the process's FD table.

---

# 17. `stat` — Look at the Object Metadata

Run:

```bash
stat original.txt
```

Pay attention to:

```text
Size
Blocks
Inode
Links
Access
Modify
Change
```

Especially:

```text
Inode
Links
```

After creating a hard link:

```bash
stat second-name.txt
```

you should see the same inode number.

```text
original.txt
inode = 12345

second-name.txt
inode = 12345
```

---

# 18. Which Filesystem Is Actually Underneath?

Run:

```bash
df -T /tmp/vfs-lab
```

You might discover `/tmp` is backed by something like:

```text
ext4
XFS
tmpfs
...
```

Yet your program still uses:

```text
open()
read()
write()
close()
```

That's VFS doing its job.

```text
Application
     ↓
open()
     ↓
VFS
     ↓
actual filesystem
```

---

# 19. VFS + Different Filesystems

Imagine:

```text
/home/file.txt       → ext4

/data/file.txt       → XFS

/tmp/file.txt        → tmpfs

/proc/cpuinfo        → procfs
```

From user space:

```text
open("/home/file.txt")

open("/data/file.txt")

open("/tmp/file.txt")

open("/proc/cpuinfo")
```

Same system-call style.

Underneath:

```text
                   VFS
                    │
        ┌───────────┼───────────┐
        ▼           ▼           ▼
      ext4         XFS        tmpfs
                               
                    │
                    └────→ procfs
```

Different filesystem implementations plug into the VFS interfaces.

---

# 20. VFS Doesn't Mean Everything Goes to Disk

This is important.

Consider:

```text
ext4
 ↓
usually block storage
```

But:

```text
tmpfs
 ↓
memory-backed
```

And:

```text
procfs
 ↓
kernel-generated virtual filesystem information
```

Yet applications still interact through familiar file/path operations.

That's the beauty of the abstraction:

```text
Application doesn't need to care
          ↓
         VFS
          ↓
Filesystem implementation decides
how the operation actually works
```

---

# 21. Superblock vs Inode vs Dentry vs `struct file`

Here's the easiest way to remember them:

### Superblock

> **Which filesystem instance?**

```text
Mounted ext4 filesystem
```

### Inode

> **Which filesystem object?**

```text
notes.txt's underlying object/metadata
```

### Dentry

> **Which name/path component?**

```text
"notes.txt" → inode
```

### `struct file`

> **Which open instance?**

```text
Process opened it with certain state
```

---

# 🧠 The Full Mental Model ⭐

Suppose:

```bash
cat /home/retr0/notes.txt
```

Conceptually:

```text
                  cat
                   │
                   │ open()
                   ▼
             System Call
                   │
═══════════════════▼════════════════
                  VFS
                   │
            Path Resolution
                   │
       / → home → retr0 → notes.txt
                   │
                dentry
                   │
                   ▼
                 inode
                   │
                   ▼
           Actual Filesystem
                (ext4)
                   │
                   ▼
                Storage
```

Once opened:

```text
Process
   │
FD 3
   │
   ▼
struct file
   │
   ▼
dentry/path
   │
   ▼
inode
   │
   ▼
Filesystem
```

---

# 🔗 Connect Your Previous Linux Topics

You can now connect several days together.

### `strace`

```text
openat("notes.txt") = 3
```

Shows the **system call**.

### `lsof`

```text
Process → FD 3 → notes.txt
```

Shows the process's **open resource**.

### VFS

Explains what happens deeper inside:

```text
FD
 ↓
struct file
 ↓
path/dentry
 ↓
inode
 ↓
filesystem
```

### `/proc`

```text
/proc/PID/fd
```

lets you inspect the process's descriptors.

So:

```text
strace
   ↓
What operations are happening?

lsof
   ↓
What is currently open?

/proc/PID/fd
   ↓
What descriptors does this process have?

VFS
   ↓
How does the kernel represent and route
filesystem operations internally?
```

---

# ⚡ Quick Revision

**VFS**

> Kernel abstraction providing a common interface across different filesystem implementations.

```text
Application
     ↓
open/read/write
     ↓
VFS
     ↓
ext4 / XFS / tmpfs / NFS / procfs...
```

### Four objects

```text
superblock
    ↓
mounted filesystem instance


inode
    ↓
filesystem object + metadata


dentry
    ↓
name/path lookup object associated with inode


struct file
    ↓
open-file instance/state
```

### Hard Links

```text
original.txt ───┐
                │
                ▼
             inode X
                ▲
                │
second.txt ─────┘
```

Therefore:

> **filename ≠ inode**

Multiple names can refer to the same inode.

---

# ⭐ One Diagram to Remember

```text
                    PROCESS
                       │
                       ▼
               File Descriptor
                       │
                       ▼
                  struct file
                       │
                       ▼
                  path/dentry
                       │
                       ▼
                     inode
                       │
                       ▼
                      VFS
                       │
            ┌──────────┼──────────┐
            ▼          ▼          ▼
          ext4        XFS       tmpfs
            │          │
            ▼          ▼
          Storage    Storage
```

And compress Day 33 into four questions:

```text
superblock  → Which filesystem?

inode       → Which filesystem object?

dentry      → Which name/path component?

struct file → Which open instance?
```

If **`filename ≠ inode`** and **`FD → struct file → filesystem object`** make sense, Day 33 is done. Day 34's pathname resolution is basically going to zoom into the **“how do we get from `/home/retr0/notes.txt` to the final dentry/inode?”** part of this diagram.