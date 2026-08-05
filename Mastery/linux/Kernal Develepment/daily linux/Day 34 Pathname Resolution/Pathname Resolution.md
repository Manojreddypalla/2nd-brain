# 🐧 Linux Internals — Day 34: Pathname Resolution

> 🎯 **Core idea:** A path like `/home/retr0/file.txt` isn't one giant filename to Linux. It's a **route that the kernel walks component by component** until it reaches the final filesystem object.

---

# 1. The Main Idea — A Path is a Traversal ⭐

Suppose:

```bash
cat /home/retr0/notes.txt
```

We humans see:

```text
/home/retr0/notes.txt
```

Linux effectively sees components:

```text
/
↓
home
↓
retr0
↓
notes.txt
```

Conceptually:

```text
Start at /
    │
    ▼
Find "home"
    │
    ▼
Is it traversable directory?
    │
    ▼
Find "retr0"
    │
    ▼
Is it traversable directory?
    │
    ▼
Find "notes.txt"
    │
    ▼
Final filesystem object
```

This process is:

> **Pathname resolution / pathname lookup**

---

# 2. Connection to Yesterday's VFS

Day 33 gave us:

```text
superblock  → filesystem instance
inode       → filesystem object
dentry      → pathname/name lookup object
struct file → opened-file instance
```

Now imagine:

```text
/home/retr0/notes.txt
```

Linux needs to find the objects associated with each component:

```text
"/"
 │
 ▼
dentry/inode
 │
 ▼
"home"
 │
 ▼
dentry/inode
 │
 ▼
"retr0"
 │
 ▼
dentry/inode
 │
 ▼
"notes.txt"
 │
 ▼
dentry/inode
```

So Day 34 is basically:

> **How does Linux reach the final dentry/inode?**

---

# 3. Absolute Paths

An **absolute path** starts with:

```text
/
```

Example:

```text
/home/retr0/file.txt
```

So lookup starts from the process's filesystem root:

```text
ROOT
 │
 ▼
home
 │
 ▼
retr0
 │
 ▼
file.txt
```

Think:

```text
/ at beginning
      ↓
absolute path
      ↓
start from root
```

---

# 4. Relative Paths

Consider:

```bash
cat notes/file.txt
```

There's no `/` at the beginning.

So where does Linux start?

From the process's:

> **CWD — Current Working Directory**

Suppose:

```bash
pwd
```

returns:

```text
/home/retr0
```

Then:

```text
notes/file.txt
```

is resolved starting there:

```text
/home/retr0
     │
     ▼
   notes
     │
     ▼
  file.txt
```

---

# 5. Every Process Has a CWD

Your shell has a current working directory.

Check:

```bash
readlink /proc/$$/cwd
```

Remember:

```text
$$ → current shell PID
```

So:

```text
/proc/$$/cwd
```

exposes the shell's current working directory.

Conceptually:

```text
Process
   │
   ├── FD table
   ├── memory
   ├── credentials
   └── CWD
         │
         ▼
    /home/retr0
```

This is why two processes can resolve the same relative pathname differently.

---

# 6. Example — Same Relative Path, Different Result

Imagine:

```text
Process A
CWD = /home/retr0

Process B
CWD = /tmp
```

Both do:

```c
open("data.txt", ...)
```

Process A starts lookup from:

```text
/home/retr0
     ↓
data.txt
```

Process B starts from:

```text
/tmp
 ↓
data.txt
```

Same string:

```text
data.txt
```

Different starting point → potentially different object.

---

# 7. `.` — Current Directory

`.` represents the current directory in pathname semantics.

Suppose:

```text
CWD = /home/retr0
```

Then:

```bash
cat ./notes.txt
```

means conceptually:

```text
CWD
 ↓
 .
 ↓
notes.txt
```

So:

```text
./notes.txt
```

resolves from the current directory.

---

# 8. `..` — Parent Directory

`..` refers to the parent in pathname traversal semantics.

Suppose:

```text
/home/retr0/projects
```

You run:

```bash
cd ..
```

Conceptually:

```text
projects
    │
    │ ..
    ▼
 retr0
```

Then:

```bash
pwd
```

becomes:

```text
/home/retr0
```

So:

```text
/home/retr0/projects/../notes.txt
```

involves walking:

```text
/
↓
home
↓
retr0
↓
projects
↓
..
↓
retr0
↓
notes.txt
```

There are subtleties around mounts and symlinks, but this traversal model is the right starting intuition.

---

# 9. Why Directories Matter

Suppose:

```text
/home/retr0/projects/kernel/file.c
```

To reach `file.c`, Linux must successfully traverse:

```text
/
↓
home
↓
retr0
↓
projects
↓
kernel
↓
file.c
```

It can't simply jump directly to `file.c`.

This is why permissions on **parent directories** matter.

---

# 10. Directory `x` Permission ⭐

For a regular file:

```text
x → execute
```

For a directory, `x` means roughly:

> **Search/traverse the directory.**

Imagine:

```text
/home/retr0/private/file.txt
```

Even if:

```text
file.txt

-rw-r--r--
```

looks readable, access may still fail if your process cannot traverse:

```text
private/
```

Conceptually:

```text
/
↓
home          ✅
↓
retr0         ✅
↓
private       ❌ no traversal permission
↓
STOP

file.txt never reached
```

So:

> **Final-file permissions aren't the whole story. Path traversal matters too.**

---

# 11. `namei` — See the Path Components ⭐

Run:

```bash
namei /tmp/path-lab/a/b/c/file.txt
```

You might see:

```text
f: /tmp/path-lab/a/b/c/file.txt
 d /
 d tmp
 d path-lab
 d a
 d b
 d c
 - file.txt
```

That's almost exactly your mental model:

```text
/
↓
tmp
↓
path-lab
↓
a
↓
b
↓
c
↓
file.txt
```

`namei` is a great tool for visualizing pathname traversal.

---

# 12. `namei -l` — Add Permissions

Run:

```bash
namei -l /tmp/path-lab/a/b/c/file.txt
```

`-l` → long listing.

Now you can inspect permissions at **every level**.

Conceptually:

```text
/           drwxr-xr-x  ✅
tmp         drwxrwxrwt  ✅
path-lab    drwxr-xr-x  ✅
a           drwxr-xr-x  ✅
b           drwx------  ❌ maybe blocked
c
file.txt
```

This is incredibly useful for debugging:

```text
Permission denied
```

because the problem might not be the final file.

It might be somewhere **earlier in the path**.

---

# 13. Dentries

Yesterday:

```text
"notes.txt"
     ↓
   dentry
     ↓
   inode
```

Today we see why dentries are so important.

Pathname resolution constantly deals with names/components:

```text
/etc/passwd
```

Conceptually:

```text
/
↓
"etc"
↓
"passwd"
```

The VFS uses dentries as part of its pathname lookup machinery.

---

# 14. Dentry Cache — `dcache`

Imagine thousands of programs repeatedly access:

```text
/etc/passwd
/etc/hosts
/usr/lib/...
/proc/...
```

It would be expensive if every pathname lookup always had to repeat all filesystem-specific lookup work from scratch.

Linux therefore caches dentries.

This is called:

> **Dentry Cache — dcache**

Conceptually:

```text
Application asks:

/etc/passwd
      ↓
VFS pathname lookup
      ↓
Check cached dentries
      ↓
Known component?
   │
 YES
   ↓
reuse cached lookup information
```

So:

> **dcache accelerates pathname lookup.**

---

# 15. Negative Dentries — Cool Detail

Linux can even cache that a name **doesn't exist**.

Suppose programs repeatedly try:

```text
/tmp/does-not-exist
```

Instead of repeatedly doing expensive underlying lookup work, VFS may retain a **negative dentry** representing:

```text
"does-not-exist"
       ↓
No inode associated
```

So dcache can help with:

```text
Things that exist
+
Things known not to exist
```

This is a nice example of how kernel caching goes deeper than simply caching file contents.

---

# 16. Symlinks Change the Walk ⭐

Create:

```bash
ln -s /tmp/path-lab/a/b/c/file.txt /tmp/my-link
```

Now:

```text
/tmp/my-link
```

is not another hard-link name for the target inode.

It's a **symbolic-link filesystem object** whose contents represent another pathname.

Conceptually:

```text
/tmp/my-link
     │
     ▼
symlink inode/object
     │
     │ contains path
     ▼
"/tmp/path-lab/a/b/c/file.txt"
     │
     ▼
Continue pathname resolution
     │
     ▼
file.txt
```

So symlink resolution can redirect the pathname walk.

---

# 17. Hard Link vs Symlink ⭐⭐⭐

This distinction is extremely important.

## Hard Link

```bash
ln file.txt hard.txt
```

Conceptually:

```text
file.txt ───┐
            │
            ▼
         inode 500
            ▲
            │
hard.txt ───┘
```

Two directory entries/names reference the same inode.

No pathname redirection is required after resolving either name.

---

## Symbolic Link

```bash
ln -s file.txt soft.txt
```

Conceptually:

```text
soft.txt
   ↓
symlink object
   ↓
contains "file.txt"
   ↓
pathname resolution
   ↓
file.txt
   ↓
target inode
```

Therefore:

```text
Hard link
    ↓
another name referencing same inode


Symlink
    ↓
separate object referring to another pathname
```

---

# 18. Why Symlinks Can Break

Suppose:

```text
soft.txt
   ↓
/home/retr0/file.txt
```

Then the target gets removed:

```bash
rm /home/retr0/file.txt
```

Now:

```text
soft.txt
   ↓
contains path
   ↓
/home/retr0/file.txt
   ↓
❌ doesn't exist
```

You get a **dangling/broken symlink**.

A hard link behaves differently because it directly adds another link to the same inode.

---

# 19. Mounts Can Also Change the Walk

Suppose:

```text
/
└── home
    └── retr0
        └── usb
```

Then another filesystem is mounted at:

```text
/home/retr0/usb
```

During pathname resolution:

```text
/
↓
home
↓
retr0
↓
usb
```

Linux reaches the mount point and then continues into the **mounted filesystem**.

Conceptually:

```text
Filesystem A

/
└── home
    └── retr0
        └── usb
             │
             │ mount boundary
             ▼

Filesystem B
/
├── photo.jpg
└── notes.txt
```

Yet from user space you see one continuous tree:

```text
/home/retr0/usb/photo.jpg
```

This leads directly into Day 35.

---

# 20. What Happens During `open()`?

Suppose:

```c
open("/home/retr0/notes.txt", ...)
```

High-level mental model:

```text
Application
     │
     │ open()
     ▼
System Call
     │
     ▼
VFS
     │
     ▼
Choose starting point
     │
     ├── /   if absolute
     │
     └── CWD if relative
     │
     ▼
Walk pathname components
     │
     ├── dentry lookup/cache
     ├── directory traversal
     ├── permission checks
     ├── symlink handling
     └── mount handling
     │
     ▼
Final filesystem object
     │
     ▼
Create/reference open-file state
     │
     ▼
Install FD
     │
     ▼
Return FD to process
```

Now Day 33 + Day 34 connect beautifully.

---

# 21. Connect With `strace`

Run:

```bash
strace -e trace=%file cat /tmp/path-lab/a/b/c/file.txt
```

You may see:

```text
openat(...)
newfstatat(...)
...
```

`strace` doesn't simply print every internal dentry lookup.

It's showing the **system-call boundary**.

Think:

```text
strace sees:

openat("/tmp/path-lab/a/b/c/file.txt", ...)
                 │
                 ▼

Inside kernel:

VFS
 ↓
/
 ↓
tmp
 ↓
path-lab
 ↓
a
 ↓
b
 ↓
c
 ↓
file.txt
```

That's an important distinction:

> `strace` shows you the kernel-facing request; VFS performs the deeper pathname resolution internally.

---

# 22. `openat()` — Why the “at”?

You'll frequently see:

```text
openat()
```

instead of older `open()`.

`openat()` can resolve a relative pathname relative to a **directory file descriptor**.

Conceptually:

```text
Directory FD
     │
     ▼
relative/path.txt
```

Or it can use:

```text
AT_FDCWD
```

meaning:

> Resolve relative to the current working directory.

So if `strace` shows:

```text
openat(AT_FDCWD, "notes.txt", ...)
```

read it as roughly:

```text
Start from CWD
      ↓
resolve notes.txt
      ↓
open it
```

This directly connects to today's absolute-vs-relative-path concept.

---

# 23. Why Pathname ≠ Object Identity

Suppose:

```text
/home/retr0/file.txt
```

gets renamed:

```text
/home/retr0/newname.txt
```

The filesystem object doesn't necessarily become a brand-new object.

The namespace/name relationship changed.

Similarly, hard links mean:

```text
name A ─┐
        ├──→ same inode
name B ─┘
```

So Linux cannot treat:

```text
"/home/retr0/file.txt"
```

as the permanent identity of the inode.

Think:

```text
Path
 ↓
How we FIND the object


inode
 ↓
Filesystem object
```

That's the key abstraction.

---

# 🔗 Day 33 + Day 34 Together

Yesterday:

```text
Process
   ↓
FD
   ↓
struct file
   ↓
dentry/inode
   ↓
Filesystem
```

Today's missing piece:

```text
"/home/retr0/file.txt"
          ↓
Pathname Resolution
          ↓
dentry/inode
```

Combine them:

```text
Process calls open()
        │
        ▼
"/home/retr0/file.txt"
        │
        ▼
Pathname Resolution
        │
        ├── /
        ├── home
        ├── retr0
        └── file.txt
              │
              ▼
         dentry/inode
              │
              ▼
         struct file
              │
              ▼
             FD
              │
              ▼
       returned to process
```

---

# ⚡ Quick Revision

### Pathname Resolution

> Linux walks a pathname **component by component**.

```text
/home/retr0/file

/
↓
home
↓
retr0
↓
file
```

### Absolute path

```text
/home/file
```

→ starts from `/` (the process's root).

### Relative path

```text
docs/file
```

→ normally starts from the process's CWD.

### `.`

→ current directory.

### `..`

→ parent-directory traversal semantics.

### Dentry

→ important VFS object for pathname/name lookup.

### dcache

→ caches dentries to accelerate repeated lookups.

### Directory `x`

→ search/traverse permission.

### Hard link

```text
name A ─┐
        ├→ same inode
name B ─┘
```

### Symlink

```text
name
 ↓
symlink object
 ↓
another pathname
 ↓
pathname resolution
 ↓
target
```

### Useful commands

```bash
namei PATH
```

→ show pathname components.

```bash
namei -l PATH
```

→ components + permissions.

```bash
readlink PATH
```

→ show symlink target.

```bash
readlink /proc/$$/cwd
```

→ current shell's CWD.

```bash
strace -e trace=%file command
```

→ observe file/path-related system calls.

---

# 🧠 One Mental Model to Keep

When you see:

```text
/home/retr0/project/file.c
```

don't think:

> **"That's the file."**

Think:

```text
             Starting Root
                  │
                  ▼
                 home
                  │
                  ▼
                retr0
                  │
                  ▼
               project
                  │
                  ▼
                file.c
                  │
                  ▼
            dentry / inode
                  │
                  ▼
          filesystem object
```

And then if the process opens it:

```text
Path
 ↓
Pathname Resolution
 ↓
inode/dentry
 ↓
struct file
 ↓
FD
```

That's the bridge between **a human-readable pathname** and **the kernel objects you learned yesterday**.