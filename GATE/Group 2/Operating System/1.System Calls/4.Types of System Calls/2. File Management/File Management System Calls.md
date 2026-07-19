# Module 1.5 — File Management System Calls

## Definition

> **File Management System Calls** are system calls used to **create, access, read, write, modify, and delete files** stored on secondary storage.

They provide a controlled interface between user programs and the file system.

---

# Why Do We Need File Management System Calls?

A user program **cannot directly access the disk**.

Instead, it requests the Operating System to perform file operations using **File Management System Calls**.

Without these system calls, applications would not be able to:

- Open files
- Read data
- Write data
- Close files
- Delete files

---

# Common File Management System Calls

| System Call | Purpose |
|-------------|---------|
| `open()` | Opens a file |
| `read()` | Reads data from a file |
| `write()` | Writes data to a file |
| `close()` | Closes a file |
| `lseek()` | Changes the file pointer position |
| `creat()` | Creates a new file |
| `unlink()` | Deletes a file |

---

# File Operation Flow

```text
Program
    │
    ▼
open()
    │
    ▼
read() / write()
    │
    ▼
close()
```

This is the typical sequence for accessing a file.

---

# How It Works

### Step 1

A program requests to open a file.

```c
open("data.txt");
```

↓

### Step 2

The kernel checks:

- Does the file exist?
- Does the process have permission?

↓

### Step 3

The kernel returns a **File Descriptor (FD)**.

↓

### Step 4

The process uses the FD with `read()` or `write()`.

↓

### Step 5

After finishing, the process calls `close()` to release the resource.

---

# What is a File Descriptor?

> A **File Descriptor (FD)** is a **small non-negative integer** returned by `open()` that uniquely identifies an opened file for a process.

Example:

```text
open("notes.txt")

↓

Returns FD = 3
```

Now the program uses:

```c
read(3, ...);
write(3, ...);
close(3);
```

instead of repeatedly specifying the filename.

---

# Example

```text
Program
    │
open("marks.txt")
    │
FD = 3
    │
read(3)
    │
write(3)
    │
close(3)
```

---

# Why Must We Close Files?

Closing a file:

- Releases kernel resources
- Flushes buffered data to disk
- Makes the File Descriptor available for reuse

---

# Key Points

- File operations are performed by the **kernel**.
- User programs access files through **System Calls**.
- `open()` returns a **File Descriptor**.
- `read()` and `write()` use the File Descriptor.
- `close()` releases the file resource.

---

# Relationship

```text
Program

↓

open()

↓

File Descriptor

↓

read()/write()

↓

close()
```

---

# 🎯 GATE Corner

## Must Remember ⭐⭐⭐⭐⭐

- File Management System Calls manage files.
- `open()` returns a **File Descriptor (FD)**.
- `read()` reads data from a file.
- `write()` writes data to a file.
- `close()` releases the opened file.
- User programs **never access disks directly**.

---

## GATE Tricks ⚠️

### ❌ Wrong Statement

> `read()` takes a filename as input.

**False**

It takes a **File Descriptor**.

---

### ❌ Wrong Statement

> `open()` reads the file contents.

**False**

It only opens the file and returns an FD.

---

### ❌ Wrong Statement

> A user program can directly read disk sectors.

**False**

All file access goes through the kernel.

---

### ✅ Correct Statement

> `open()` returns a File Descriptor used by subsequent file operations.

---

## Common MCQs

### Q1

Which System Call returns a File Descriptor?

A. `read()`

B. `write()`

C. `open()`

D. `close()`

✅ **Answer:** **C**

---

### Q2

Which System Call is used to release an opened file?

A. `open()`

B. `close()`

C. `read()`

D. `creat()`

✅ **Answer:** **B**

---

### Q3

Which System Call changes the current file pointer position?

A. `unlink()`

B. `lseek()`

C. `read()`

D. `write()`

✅ **Answer:** **B**

---

### Q4

`read()` operates on

A. File Name

B. Process ID

C. File Descriptor

D. Memory Address only

✅ **Answer:** **C**

---

# Formula Corner 🧮

There are **no mathematical formulas**.

Remember the sequence:

```text
open()

↓

File Descriptor

↓

read()/write()

↓

close()
```

---

# One-Line Revision

> **File Management System Calls allow a user program to safely access files through the kernel using a File Descriptor returned by `open()`.**