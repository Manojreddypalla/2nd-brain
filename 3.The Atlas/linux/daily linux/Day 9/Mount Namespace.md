# Linux Internals — Day 9 Notes

# Mount Namespace (Deep Dive)

> **Category:** Linux Internals / Containers / Docker / DevOps _(Development topic, not GATE)_

---

# First Intuition — Why do we need Mount Namespaces?

Imagine there is **one huge library**.

Everyone enters the library through the same entrance.

They all see

```
/
├── home
├── etc
├── usr
├── var
├── tmp
└── opt
```

Now imagine you want to run an Ubuntu container and an Alpine container on the same machine.

Ubuntu expects

```
/etc/apt
```

while Alpine expects

```
/etc/apk
```

If both shared the exact same filesystem, they'd overwrite each other's files.

Linux solves this by saying:

> "Instead of giving every process the same filesystem view, I'll let each process have its own view."

That "view" is a **Mount Namespace**.

---

# One-Line Definition

> **A Mount Namespace isolates the filesystem hierarchy (mount points) visible to a process.**

Notice the wording carefully.

It **doesn't create another hard drive.**

It creates another **view** of mounted filesystems.

---

# Before Understanding Mount Namespace...

We need to understand **Mounting**.

Without this concept, mount namespaces don't make sense.

---

# What is a Mount?

Think about Windows.

You insert a USB.

Immediately

```
D:\
```

appears.

The USB already existed.

Windows simply **attached** it to

```
D:
```

Linux works similarly.

Suppose you have a hard disk

```
/dev/sdb1
```

It is just a block device.

You cannot access its files until Linux attaches it somewhere.

Example

```
mount /dev/sdb1 /mnt
```

Now

```
/mnt
```

shows the contents of that disk.

---

## Visual

Before mount

```
Hard Disk

/dev/sdb1
```

Filesystem

```
/

├── home

├── etc

├── tmp
```

No connection.

---

After mount

```
/

├── home

├── etc

├── tmp

└── mnt
      │
      ▼
   /dev/sdb1
```

Now

```
ls /mnt
```

shows files from the disk.

---

# What is a Mount Point?

A mount point is simply

> **A directory where another filesystem is attached.**

Example

```
/

├── home

├── boot

├── media

├── mnt
```

Any of these can become mount points.

Example

```
mount /dev/sdb1 /mnt
```

Now

```
/mnt
```

becomes a mount point.

---

# Linux Filesystem is Like a Tree

Everything begins at

```
/
```

```
/

├── home

├── etc

├── usr

├── proc

├── dev

└── tmp
```

Every mounted filesystem gets inserted somewhere in this tree.

That's why Linux is often called a **single unified filesystem**.

Unlike Windows

```
C:
D:
E:
```

Linux has one tree.

---

# Problem Without Mount Namespace

Imagine Docker didn't exist.

Host

```
/

├── etc

├── home

├── usr

├── var
```

Container starts.

It sees

```
/

├── etc

├── home

├── usr

├── var
```

Now inside container

```
rm -rf /etc
```

Oops.

Host destroyed.

Not acceptable.

---

# Solution

Give container another filesystem view.

Host

```
/

├── etc

├── home

├── var
```

Container

```
/

├── etc
├── app
├── tmp
└── usr
```

Notice

The container isn't changing the host.

It is simply looking through another "window."

---

# Mental Model

Imagine wearing VR goggles.

Reality

```
Chair
Table
Computer
```

VR Headset A

```
Castle
Dragon
River
```

VR Headset B

```
Mars
Rocket
Alien
```

Same room.

Different view.

Mount Namespace is basically VR for filesystems.

---

# What Exactly Gets Isolated?

Not files.

**Mount table.**

This is very important.

Linux maintains a list

```
Device

↓

Mounted At
```

Example

```
/dev/sda2

↓

/
```

```
tmpfs

↓

/tmp
```

```
proc

↓

/proc
```

That table is isolated.

Each namespace gets its own copy.

---

# What is tmpfs?

Today's lab uses

```
tmpfs
```

Many beginners think tmpfs is another disk.

It isn't.

It is a filesystem stored in RAM.

Think

```
RAM

↓

Filesystem
```

Instead of

```
SSD

↓

Filesystem
```

Advantages

- Very fast
    
- Temporary
    
- Automatically disappears after unmount or reboot
    

---

# Understanding the Command

```
mount -t tmpfs tmpfs /tmp/linux-lab
```

Let's decode it.

---

## mount

Attach a filesystem.

---

## -t

Means

```
Type
```

Filesystem type.

Examples

```
ext4
xfs
btrfs
tmpfs
```

---

## tmpfs

Filesystem stored in RAM.

---

## /tmp/linux-lab

Mount point.

---

Internally Linux does

```
Create tmpfs

↓

Attach it

↓

at

↓

/tmp/linux-lab
```

---

# Mount Namespace

Suppose host

```
Mount Table

/dev/sda2

↓

/
```

```
tmpfs

↓

/tmp
```

Now

```
sudo unshare --mount bash
```

creates

Another copy

```
Mount Table

/dev/sda2

↓

/
```

```
tmpfs

↓

/tmp
```

Initially identical.

Now you add

```
tmpfs

↓

/tmp/linux-lab
```

Only inside namespace.

Host table

```
/

tmp

proc
```

Namespace

```
/

tmp

proc

tmp/linux-lab
```

Different mount tables.

---

# Visual Representation

Host

```
/

├── home

├── etc

├── tmp
```

Namespace

```
/

├── home

├── etc

├── tmp

└── linux-lab
```

Host doesn't know this mount exists.

---

# What Happens Internally?

```
unshare

↓

Clone mount namespace

↓

Copy mount table

↓

Launch bash

↓

Modify copied table
```

Original untouched.

---

# Why Docker Needs Mount Namespace

Suppose container image contains

```
/

├── bin

├── etc

├── usr

├── app
```

Docker mounts this filesystem.

Host

```
/

├── home

├── etc
```

Container

```
/

├── app

├── etc

├── usr
```

The container believes

```
/
```

is its own filesystem.

Actually

It's only a different mount namespace (combined with other technologies like OverlayFS and `pivot_root`/`chroot`).

---

# Why Doesn't It Affect Host?

Because

The host and container use

different

```
Mount Tables
```

Changing one table

doesn't modify

the other.

Think

Two Google Maps tabs.

Adding a pin in one

doesn't add it to the other.

---

# Important Commands

---

## mount

Shows mounted filesystems.

Example

```bash
mount
```

Output

```
proc on /proc

tmpfs on /tmp

/dev/sda2 on /
```

---

## findmnt

Modern tool.

Displays mounts as a tree.

```
findmnt
```

Much easier to read.

---

## unshare

```
sudo unshare --mount bash
```

Create mount namespace.

Launch bash.

---

## mkdir

```
mkdir /tmp/linux-lab
```

Creates mount point directory.

---

## Mount tmpfs

```
mount -t tmpfs tmpfs /tmp/linux-lab
```

Attach RAM filesystem.

---

## touch

```
touch file.txt
```

Creates empty file.

---

# Mini Experiment

Inside namespace

```
touch /tmp/linux-lab/test.txt
```

```
ls /tmp/linux-lab
```

Shows

```
test.txt
```

Exit namespace

```
exit
```

Host

```
ls /tmp/linux-lab
```

You don't see the mounted tmpfs because that mount existed only in the namespace. The original directory on the host is still there, but it wasn't overlaid by the tmpfs mount.

---

# Mount Namespace vs PID Namespace

|PID Namespace|Mount Namespace|
|---|---|
|Isolates process tree|Isolates filesystem view|
|Own PID 1|Own mount table|
|`ps` changes|`mount` / `findmnt` changes|
|Used for process isolation|Used for filesystem isolation|

---

# Where is this Used?

- Docker
    
- Kubernetes
    
- Podman
    
- LXC
    
- systemd services
    
- Sandboxing tools
    
- Rootless containers
    

Anywhere you need isolated filesystem views without running a full virtual machine.

---

# Interview Corner ⭐

### Why doesn't a container modify the host filesystem?

Because it operates in a separate **mount namespace** with its own mount table and typically its own root filesystem.

---

### What exactly is isolated?

**The mount table (filesystem hierarchy), not the physical disks.**

---

### Is a new hard drive created?

**No.**

Only another view of mounted filesystems.

---

### Can two namespaces mount the same disk differently?

**Yes.**

Each namespace can mount the same underlying device at different mount points or with different mount options, independently.

---

### Does a mount namespace copy files?

**No.**

It copies the **mount table**, not the file contents.

---

# Common Mistakes

❌ "A mount namespace creates another filesystem."

No. It creates another **view** of filesystems.

---

❌ "tmpfs is a hard disk."

No.

It is a filesystem stored in RAM.

---

❌ "Containers have their own SSD."

No.

Containers usually share the host's storage but see it through isolated mount namespaces and layered filesystems.

---

# Commands Learned Today

```bash
mount
```

Lists mounted filesystems.

```bash
findmnt
```

Displays the mount hierarchy in a tree.

```bash
sudo unshare --mount bash
```

Creates a new mount namespace and launches a shell.

```bash
mkdir /tmp/linux-lab
```

Creates a directory that will act as a mount point.

```bash
mount -t tmpfs tmpfs /tmp/linux-lab
```

Mounts a temporary in-memory filesystem.

```bash
touch /tmp/linux-lab/test.txt
```

Creates a file inside the mounted filesystem.

```bash
ls /tmp/linux-lab
```

Lists files in the mounted directory.

```bash
exit
```

Leaves the mount namespace.

---

# Day 9 Summary

- A **mount namespace** gives a process its own view of the filesystem hierarchy.
    
- A **mount point** is a directory where another filesystem is attached.
    
- Each mount namespace has its own **mount table**, so mounting or unmounting filesystems in one namespace doesn't affect another.
    
- **`tmpfs`** is a temporary filesystem that stores data in RAM, making it fast and ephemeral.
    
- Docker uses **mount namespaces**, together with technologies like **OverlayFS**, to provide each container with what appears to be its own root filesystem while sharing the same Linux kernel.
    
- Unlike a **PID namespace**, which isolates processes, a **mount namespace** isolates how filesystems are presented to a process.
    

## Connection to Previous Days

You now know two core namespace types:

```
Linux Kernel
│
├── PID Namespace
│   └── "Which processes can I see?"
│
└── Mount Namespace
    └── "Which filesystem can I see?"
```

Tomorrow, when you study **Network Namespaces**, you'll complete another major piece of the container puzzle by seeing how each container can have its own network interfaces, IP addresses, routing table, and even listen on the same port number independently.