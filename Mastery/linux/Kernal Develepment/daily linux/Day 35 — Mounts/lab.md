# 🧪 Linux Day 35 — Mounts Lab

> 🎯 **Goal:** See that Linux's directory tree can contain multiple mounted filesystems, and understand what a mount actually changes.

## 1. Inspect Your Real Mount Tree

Start with:

```bash
findmnt
```

Don't read every line. Look at the **tree structure**.

Now:

```bash
findmnt /
findmnt /proc
findmnt /sys
findmnt /dev
```

Pay attention to:

```text
TARGET   → where mounted
SOURCE   → where it comes from
FSTYPE   → filesystem type
OPTIONS  → mount options
```

### Short note

```text
/      → root filesystem
/proc  → usually procfs
/sys   → sysfs
/dev   → device-related filesystem
```

Linux's `/` tree is **not necessarily one filesystem**.

---

# 2. See Different Filesystem Types

```bash
df -T
```

You may see:

```text
ext4
xfs
tmpfs
...
```

Also try:

```bash
findmnt -t proc,sysfs,tmpfs
```

Mental model:

```text
ext4   → storage filesystem
tmpfs  → memory-backed
procfs → kernel/process information
sysfs  → kernel/device information
```

So:

> **filesystem ≠ physical disk**

---

# 3. Create Our Playground

```bash
mkdir -p /tmp/mount-lab/source
mkdir -p /tmp/mount-lab/view
```

Create files:

```bash
echo "Hello from source" > /tmp/mount-lab/source/file.txt
echo "Original view file" > /tmp/mount-lab/view/hidden.txt
```

Check:

```bash
ls /tmp/mount-lab/source
ls /tmp/mount-lab/view
```

You have:

```text
source/
└── file.txt

view/
└── hidden.txt
```

---

# 4. Create a Bind Mount ⭐

Run:

```bash
sudo mount --bind /tmp/mount-lab/source /tmp/mount-lab/view
```

Now:

```bash
ls /tmp/mount-lab/view
```

You should see:

```text
file.txt
```

Wait—where did:

```text
hidden.txt
```

go?

It wasn't deleted.

The mount changed what pathname resolution sees at `/tmp/mount-lab/view`.

```text
BEFORE

view/
└── hidden.txt


AFTER MOUNT

view/  ← mount point
  │
  ▼
source/
└── file.txt
```

---

# 5. Prove It's Not a Copy

Run:

```bash
cat /tmp/mount-lab/view/file.txt
```

Now modify through `view`:

```bash
echo "Modified through view" >> /tmp/mount-lab/view/file.txt
```

Check the original path:

```bash
cat /tmp/mount-lab/source/file.txt
```

You'll see the modification.

Why?

```text
source/file.txt ───┐
                   │
                   ▼
             same underlying
                 object
                   ▲
                   │
view/file.txt ─────┘
```

> **Bind mount does not copy the data.**

It gives you another path to the existing subtree.

---

# 6. Inspect the Mount

Run:

```bash
findmnt /tmp/mount-lab/view
```

You should see information about the bind mount.

Also try:

```bash
mount | grep mount-lab
```

Now you're seeing the actual mount relationship Linux is tracking.

---

# 7. Look at Kernel Mount Information

Run:

```bash
grep mount-lab /proc/self/mountinfo
```

You'll get an ugly-looking line. That's okay.

The important point is:

```text
/proc/self/mountinfo
        ↓
kernel-visible mount information
        ↓
for this process's mount namespace
```

`findmnt` gives you the nicer human-readable view.

---

# 8. Watch Path Resolution Cross the Mount ⭐

Run:

```bash
namei /tmp/mount-lab/view/file.txt
```

From your perspective:

```text
/
↓
tmp
↓
mount-lab
↓
view
↓
──────────── MOUNT ────────────
↓
source subtree
↓
file.txt
```

The pathname still looks completely normal:

```text
/tmp/mount-lab/view/file.txt
```

But internally the walk crossed a **mount boundary**.

This is the main idea of Day 35.

---

# 9. Unmount It 🔥

Run:

```bash
sudo umount /tmp/mount-lab/view
```

Now:

```bash
ls /tmp/mount-lab/view
```

What comes back?

```text
hidden.txt
```

🔥 That's the experiment worth remembering.

Nothing was deleted.

```text
Before mount:

view/
└── hidden.txt


During mount:

view/
└── file.txt


After umount:

view/
└── hidden.txt
```

The mount simply changed what pathname lookup saw at that point.

---

# 10. Verify the Source Still Exists

```bash
cat /tmp/mount-lab/source/file.txt
```

You'll still see:

```text
Hello from source
Modified through view
```

So:

```text
mount
→ didn't copy

umount
→ didn't delete

bind mount
→ exposed the same subtree elsewhere
```

---

# 11. Bonus — Mount a RAM Filesystem

If you want one extra experiment:

```bash
mkdir -p /tmp/mount-lab/ram
```

Mount `tmpfs`:

```bash
sudo mount -t tmpfs tmpfs /tmp/mount-lab/ram
```

Check:

```bash
findmnt /tmp/mount-lab/ram
```

You'll see something like:

```text
TARGET              SOURCE FSTYPE
/tmp/mount-lab/ram  tmpfs  tmpfs
```

Create:

```bash
echo "I live in tmpfs" > /tmp/mount-lab/ram/test.txt
```

Read:

```bash
cat /tmp/mount-lab/ram/test.txt
```

Here you've mounted a **memory-backed filesystem** into the same Linux tree.

```text
Linux tree
    │
    ├── ext4
    │
    ├── procfs
    │
    ├── sysfs
    │
    └── tmpfs ← your experiment
```

Unmount:

```bash
sudo umount /tmp/mount-lab/ram
```

Because this was a temporary `tmpfs`, the data stored in that mounted instance is no longer available through that mount after unmounting.

---

# 🧹 Cleanup

Make sure the mounts are gone:

```bash
findmnt | grep mount-lab
```

Then:

```bash
rm -rf /tmp/mount-lab
```

---

# 📝 Lab Short Notes

```text
findmnt
→ view mount tree

df -T
→ filesystem types + space

/proc/self/mountinfo
→ kernel mount information visible to process


Mount
→ attaches filesystem/tree to a directory


Bind mount
mount --bind SOURCE TARGET

→ exposes existing subtree at another path
→ does NOT copy data


Mounting over a directory:
→ hides/obscures its existing contents
→ data is NOT deleted

umount:
→ removes mount
→ underlying directory becomes visible again


tmpfs:
→ memory-backed filesystem
→ can be mounted into normal Linux tree
```

## ⭐ What This Lab Actually Proved

The key experiment was:

```text
                 /
                 │
                tmp
                 │
             mount-lab
              /      \
             /        \
         source       view
            │           │
            │   bind mount
            └──────────►│
                        │
                     file.txt
```

So don't imagine Linux as:

> "`/` = one giant filesystem."

Think:

> **Linux presents one unified namespace, and mounts stitch filesystem trees/subtrees into that namespace.**

That makes the Day 33 → 35 chain:

```text
VFS
 ↓
How objects are represented

Pathname Resolution
 ↓
How Linux walks the tree

Mounts
 ↓
How that walk can cross into another filesystem
```

That's the foundation you'll need for **Page Cache** next.