# Linux Filesystem Internals — Day 4 (10–15 min)

This is one of the most important Linux topics. Once you understand **inodes**, you'll understand why hard links work, why deleting a file doesn't always free disk space immediately, and how the Linux filesystem is organized internally.

---

# The Big Picture

When you think of a file, you probably imagine this:

```text
report.txt
```

But Linux sees it differently.

```text
Filename
    │
    ▼
Inode
    │
    ▼
Data Blocks
```

The **filename is not the file**.

The **inode is the file's identity**.

The **data blocks contain the actual contents**. ([Wikipedia](https://en.wikipedia.org/wiki/Unix_filesystem?utm_source=chatgpt.com "Unix filesystem"))

---

# What is a Filesystem?

A **filesystem** is how an operating system organizes and manages data on storage devices like SSDs and HDDs.

Without a filesystem, a disk is just millions or billions of bytes.

The filesystem answers questions like:

- Where is a file stored?
    
- Who owns it?
    
- How large is it?
    
- Who can read it?
    
- Which disk blocks belong to it?
    

Think of a filesystem as a **database for your disk**.

```
SSD
│
├── File A
├── File B
├── Directory C
└── File D
```

Popular Linux filesystems include:

- ext4 (most common)
    
- XFS
    
- Btrfs
    
- F2FS
    

Each uses different internal data structures, but the inode concept is common in Unix-like systems. ([Wikipedia](https://en.wikipedia.org/wiki/Unix_filesystem?utm_source=chatgpt.com "Unix filesystem"))

---

# What Actually Happens When You Save a File?

Suppose you create:

```bash
echo "Hello Linux" > report.txt
```

Internally Linux performs something like this:

```
Create inode

↓

Allocate disk blocks

↓

Store "Hello Linux"

↓

Create directory entry

↓

report.txt → inode 3512
```

Notice something interesting.

The filename is created **last**.

The filename is simply an entry inside a directory that points to an inode. ([Wikipedia](https://en.wikipedia.org/wiki/Unix_filesystem?utm_source=chatgpt.com "Unix filesystem"))

---

# Understanding Directories

Many beginners think directories contain files.

Not exactly.

Directories contain **mappings**.

```
Directory

report.txt  → inode 3512

notes.pdf   → inode 4901

photo.png   → inode 7831
```

The directory stores:

```
Filename

↓

Inode Number
```

It does **not** store the file's contents.

---

# What is an Inode?

An **inode (Index Node)** is a data structure that describes a file.

It **does not store the filename**.

It stores everything else.

Imagine a library.

```
Book Label

↓

Library Record

↓

Actual Book
```

The library record contains:

- Author
    
- Shelf number
    
- Number of pages
    
- Borrow status
    

The book label simply helps you find that record.

Linux works almost exactly the same way.

```
report.txt

↓

inode 3512

↓

Disk blocks
```

([Wikipedia](https://en.wikipedia.org/wiki/Inode?utm_source=chatgpt.com "Inode"))

---

# What Does an Inode Store?

An inode stores metadata such as:

- Owner (UID)
    
- Group (GID)
    
- File permissions
    
- File size
    
- Creation/modify/access timestamps
    
- Link count
    
- File type
    
- Pointers to the file's data blocks
    

Notice what is **missing**.

```
Filename
```

The filename lives in the directory, not in the inode. ([Wikipedia](https://en.wikipedia.org/wiki/Inode?utm_source=chatgpt.com "Inode"))

---

# File Name vs Inode vs Data

Suppose we have:

```text
resume.pdf
```

Internally:

```
resume.pdf
      │
      ▼
inode 9120
      │
      ▼
Block 50
Block 51
Block 52
```

### Filename

Just a human-readable label.

```
resume.pdf
```

### Inode

Stores metadata.

```
Owner

Permissions

Size

Dates

Pointers
```

### Data Blocks

Actual bytes.

```
PDF Contents

Page 1

Page 2

Images

Fonts
```

---

# File Metadata

Metadata means

> **Data about data**

Example

```
report.txt
```

Metadata:

```
Owner : retr0

Size : 4 KB

Permissions : rw-r--r--

Modified :
Yesterday
```

Content:

```
Hello Linux!
```

Metadata describes the file.

Content is the file itself.

---

# Visual Summary

```
Filename

report.txt

↓

Inode

Owner
Permissions
Size
Timestamps
Pointers

↓

Disk Blocks

Hello Linux
```

---

# What Happens When You Rename a File?

```bash
mv report.txt notes.txt
```

Most beginners think Linux changes the file.

Actually:

```
Old directory entry removed

↓

New directory entry created

↓

Same inode

↓

Same data
```

Nothing changes inside the inode.

Only the directory entry changes. ([Wikipedia](https://en.wikipedia.org/wiki/Unix_filesystem?utm_source=chatgpt.com "Unix filesystem"))

---

# Hard Links

Now comes the magic.

Suppose:

```
report.txt
```

creates

```
inode 100

↓

Data
```

Create a hard link:

```bash
ln report.txt backup.txt
```

Now:

```
report.txt ──┐
             │
             ▼
         inode 100
             │
             ▼
          Data Blocks
             ▲
             │
backup.txt ──┘
```

Both names point to the **same inode**.

There is **no original** and **no copy**.

They are simply two directory entries pointing to one inode. ([Wikipedia](https://en.wikipedia.org/wiki/Ln_%28Unix%29?utm_source=chatgpt.com "Ln (Unix)"))

---

# Deleting a Hard Link

Suppose:

```
report.txt

backup.txt

↓

inode 100
```

Delete:

```bash
rm report.txt
```

Result:

```
backup.txt

↓

inode 100

↓

Data
```

The file still exists because another directory entry references the same inode.

Only when the inode's **link count reaches zero** (and no process still has the file open) does Linux free the data blocks. ([Wikipedia](https://en.wikipedia.org/wiki/Ln_%28Unix%29?utm_source=chatgpt.com "Ln (Unix)"))

---

# Soft (Symbolic) Links

A symbolic link is completely different.

```
report.txt

↓

inode 100

↓

Data
```

Create:

```bash
ln -s report.txt shortcut.txt
```

Now:

```
shortcut.txt

↓

inode 250

↓

Contains text:

"report.txt"
```

The symlink has **its own inode**.

It does **not** point directly to the data.

It stores the **path** to another file. ([Wikipedia](https://en.wikipedia.org/wiki/Symbolic_link?utm_source=chatgpt.com "Symbolic link"))

---

# Deleting the Original

```
report.txt

↓

Data

shortcut.txt

↓

"report.txt"
```

Delete:

```bash
rm report.txt
```

Now:

```
shortcut.txt

↓

"report.txt"

↓

Nothing
```

The symbolic link becomes a **dangling (broken) symlink** because the path it references no longer exists. ([Wikipedia](https://en.wikipedia.org/wiki/Symbolic_link?utm_source=chatgpt.com "Symbolic link"))

---

# Hard Link vs Soft Link

|Feature|Hard Link|Soft (Symbolic) Link|
|---|---|---|
|Points to|Inode|File path|
|Own inode?|No (shares target inode)|Yes|
|Works after original filename is deleted?|✅ Yes|❌ No|
|Cross filesystem?|❌ No|✅ Yes|
|Can link directories?|Generally ❌|✅ Yes|
|Looks like Windows shortcut?|❌|✅ Yes|

([Wikipedia](https://en.wikipedia.org/wiki/Ln_%28Unix%29?utm_source=chatgpt.com "Ln (Unix)"))

---

# Complete Mental Model

Think of a library.

```
Library Catalog

↓

Book ID

↓

Book
```

Linux:

```
Filename

↓

Inode Number

↓

Metadata

↓

Disk Blocks

↓

Actual Content
```

Remember this one sentence:

> **A filename is just a label in a directory. The inode is the file's identity. The data blocks hold the actual bytes.**

---

# Practical Commands (Spend 5–10 Minutes)

### Create a file

```bash
echo "Hello Linux" > file.txt
```

### View inode number

```bash
ls -i file.txt
```

Example:

```
3512 file.txt
```

### View metadata

```bash
stat file.txt
```

This shows:

- inode number
    
- permissions
    
- owner
    
- timestamps
    
- link count
    
- size
    

### Create a hard link

```bash
ln file.txt hard.txt
```

Check inode numbers:

```bash
ls -li
```

You'll see **both files have the same inode number**.

### Create a symbolic link

```bash
ln -s file.txt soft.txt
```

List with details:

```bash
ls -l
```

Example:

```
soft.txt -> file.txt
```

### Remove the original

```bash
rm file.txt
```

Now compare:

```bash
cat hard.txt
```

✅ Still works.

```bash
cat soft.txt
```

❌ Fails because the symbolic link points to a path that no longer exists.

---

## Quick Revision

- **Filesystem:** Organizes files and directories on disk.
    
- **Directory:** Maps filenames to inode numbers.
    
- **Filename:** Just a label.
    
- **Inode:** Stores metadata and pointers to the file's data blocks (not the filename).
    
- **Data Blocks:** Store the actual file contents.
    
- **Metadata:** Information like owner, permissions, timestamps, and size.
    
- **Hard Link:** Another filename pointing to the same inode.
    
- **Soft Link:** A separate file containing a path to another file.