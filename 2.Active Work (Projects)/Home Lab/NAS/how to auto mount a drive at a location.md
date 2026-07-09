# Linux Practical: Auto-Mounting a Drive with `/etc/fstab`

## 🎯 Goal

Automatically mount a storage drive every time Linux boots.

Example:

```text
/media/manoj/ZORO   → 2TB Drive
/media/manoj/SANJI  → 1TB Drive
```

Instead of manually mounting after every reboot.

---

# Why is this needed?

Normally, Linux detects a drive but **doesn't always mount it permanently**.

Without `fstab`:

```
Boot
   │
   ▼
Drive detected
   │
   ▼
Need to mount manually
```

With `fstab`:

```
Boot
   │
   ▼
Reads /etc/fstab
   │
   ▼
Automatically mounts drive
```

---

# What is `/etc/fstab`?

`fstab` stands for

> **File System Table**

It is a configuration file that tells Linux:

- Which drives to mount
    
- Where to mount them
    
- Which filesystem they use
    
- What mount options to use
    

Location:

```bash
/etc/fstab
```

View it:

```bash
cat /etc/fstab
```

Edit it:

```bash
sudo nano /etc/fstab
```

---

# Step 1 — Find the Drive

List disks:

```bash
lsblk
```

Example:

```
sda
├── sda1

sdb
└── sdb1

sdc
└── sdc5
```

---

# Step 2 — Find UUID

Linux should use **UUID**, not `/dev/sdX`, because device names can change after reboot.

Get UUID:

```bash
sudo blkid /dev/sdc5
```

Example:

```
UUID="926B-24A0"
TYPE="exfat"
LABEL="manoj"
```

---

# Why UUID?

Bad:

```
/dev/sdc5
```

After plugging in another drive:

```
/dev/sdd5
```

Everything breaks.

Good:

```
UUID=926B-24A0
```

UUID never changes (unless the filesystem is recreated).

---

# Step 3 — Create a Mount Point

A mount point is simply an empty directory where the filesystem will appear.

Example:

```
Drive
   │
   ▼
Filesystem
   │
   ▼
Mounted at
/media/manoj/SANJI
```

Create it:

```bash
sudo mkdir -p /media/manoj/SANJI
```

---

# Step 4 — Edit `fstab`

Open:

```bash
sudo nano /etc/fstab
```

Add:

```fstab
UUID=926B-24A0  /media/manoj/SANJI  exfat  defaults,uid=1000,gid=1000,umask=022,nofail  0  0
```

---

# Understanding Each Field

```
UUID=926B-24A0
```

Which drive?

↓

```
/media/manoj/SANJI
```

Where should Linux mount it?

↓

```
exfat
```

Filesystem type.

↓

```
defaults
```

Default mount options.

↓

```
uid=1000
```

Owner User ID.

Usually your first user.

Check:

```bash
id
```

---

```
gid=1000
```

Owner Group ID.

---

```
umask=022
```

Default permissions.

Result:

```
Folders

755

Files

644
```

Meaning:

Owner:

```
Read
Write
Execute
```

Others:

```
Read
Execute
```

---

```
nofail
```

If the drive is missing,

Linux still boots.

Without this:

```
Missing drive
↓

Boot failure
```

---

Last two numbers:

```
0 0
```

First:

```
Dump backup utility
```

Usually:

```
0
```

Second:

Filesystem check (`fsck`).

For exFAT:

```
0
```

For ext4:

Usually:

```
2
```

---

# Step 5 — Test Before Reboot

Never reboot immediately.

Instead:

```bash
sudo mount -a
```

Linux attempts to mount everything in `fstab`.

No output usually means success.

---

# Step 6 — Reload systemd

Sometimes you'll see:

```
fstab has been modified
systemd still uses the old version
```

Reload:

```bash
sudo systemctl daemon-reload
```

---

# Step 7 — Verify

See mounted filesystems:

```bash
df -h
```

or

```bash
mount
```

or

```bash
lsblk -f
```

Expected:

```
sdc5
└── /media/manoj/SANJI
```

---

# Useful Commands

See disks:

```bash
lsblk
```

Filesystem info:

```bash
sudo blkid
```

Mounted filesystems:

```bash
df -h
```

Create mount point:

```bash
sudo mkdir -p /media/manoj/SANJI
```

Edit `fstab`:

```bash
sudo nano /etc/fstab
```

Test mounts:

```bash
sudo mount -a
```

Reload systemd:

```bash
sudo systemctl daemon-reload
```

Unmount manually:

```bash
sudo umount /media/manoj/SANJI
```

Mount manually:

```bash
sudo mount /media/manoj/SANJI
```

---

# Common Mistakes

- Using `/dev/sdc5` instead of the drive's UUID.
    
- Forgetting to create the mount point directory.
    
- Specifying the wrong filesystem type (e.g., `ntfs` instead of `exfat`).
    
- Rebooting without testing the `fstab` entry using `sudo mount -a`.
    
- Omitting `nofail`, which can delay or prevent boot if the drive is unavailable.
    
- Editing `fstab` without keeping a backup.
    

---

# Mental Model

Think of mounting like attaching a folder to a storage device:

```
Physical Disk
      │
      ▼
 Partition (/dev/sdc5)
      │
      ▼
 Filesystem (exFAT)
      │
      ▼
 Mount
      │
      ▼
 /media/manoj/SANJI
```

Applications don't access the disk directly—they access the mounted directory, and the Linux kernel transparently reads and writes data on the underlying filesystem.


```bash 

sudo nano /etc/fstab

  GNU nano 8.7.1                                          /etc/fstab
# /etc/fstab: static file system information.
#
# Use 'blkid' to print the universally unique identifier for a
# device; this may be used with UUID= as a more robust way to name devices
# that works even if disks are added and removed. See fstab(5).
#
# <file system> <mount point>   <type>  <options>       <dump>  <pass>
# / was on /dev/sda2 during curtin installation
/dev/disk/by-uuid/2f7bb065-157c-4a8c-9819-04995fa555ab / ext4 defaults 0 1
/swap.img       none    swap    sw      0       0
UUID=DA7CFE1A7CFDF15D  /media/manoj/ZORO  ntfs-3g  defaults,uid=1000,gid=1000  0  0
UUID=926B-24A0  /media/manoj/SANJI  exfat  defaults,uid=1000,gid=1000,umask=022,nofail  0  0



```