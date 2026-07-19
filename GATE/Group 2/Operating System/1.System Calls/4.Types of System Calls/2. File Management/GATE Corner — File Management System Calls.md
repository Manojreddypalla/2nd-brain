# 🎯 GATE Corner — File Management System Calls

## Weightage

⭐⭐⭐⭐⭐ **Very Important**

Frequently asked with:

- System Calls
- File System
- File Descriptor
- I/O Operations
- User Mode & Kernel Mode

---

# Must Remember ⭐⭐⭐⭐⭐

File Management System Calls are used to

- Create files
- Open files
- Read files
- Write files
- Change file position
- Close files
- Delete files

---

## Important System Calls

| System Call | Function |
|-------------|----------|
| `open()` | Opens a file and returns a File Descriptor (FD) |
| `read()` | Reads data from a file |
| `write()` | Writes data to a file |
| `close()` | Closes an opened file |
| `lseek()` | Changes the file pointer position |
| `creat()` | Creates a new file |
| `unlink()` | Deletes a file |

---

# Remember This Sequence ⭐⭐⭐⭐⭐

```text
open()

↓

File Descriptor

↓

read() / write()

↓

close()
```

---

# Most Asked Concepts

## `open()`

- Opens a file
- Returns a **File Descriptor (FD)**
- Does **not** read the file

---

## `read()`

- Reads data
- Uses the **File Descriptor**
- Returns the number of bytes read

---

## `write()`

- Writes data
- Uses the **File Descriptor**
- Returns the number of bytes written

---

## `close()`

- Releases the File Descriptor
- Frees kernel resources

---

# ⭐ IMPORTANT GATE FACT

## Every File Management System Call causes a

```text
Mode Switch
```

because they execute inside the **Kernel**.

Example:

```text
Program

↓

read()

↓

Kernel Mode

↓

Disk Access

↓

User Mode
```

### Examples

- `open()` ✅ Mode Switch
- `read()` ✅ Mode Switch
- `write()` ✅ Mode Switch
- `close()` ✅ Mode Switch
- `lseek()` ✅ Mode Switch

---

# GATE Tricks ⚠️

### ❌ Wrong Statement

> `read()` uses a filename.

**False**

It uses a **File Descriptor**.

---

### ❌ Wrong Statement

> `open()` reads file contents.

**False**

It only opens the file and returns an FD.

---

### ❌ Wrong Statement

> Applications access the disk directly.

**False**

Only the **Kernel** accesses storage devices.

---

### ❌ Wrong Statement

> File Management System Calls execute in User Mode.

**False**

They execute in **Kernel Mode**, so every one causes a **Mode Switch**.

---

### ✅ Correct Statement

> `open()` returns a File Descriptor used by `read()`, `write()`, and `close()`.

---

# Common MCQs

### Q1

Which System Call returns a File Descriptor?

A. `read()`

B. `write()`

C. `open()`

D. `close()`

✅ **Answer:** **C**

---

### Q2

Which System Call uses a File Descriptor to read data?

A. `open()`

B. `read()`

C. `creat()`

D. `unlink()`

✅ **Answer:** **B**

---

### Q3

Which System Call changes the current file pointer?

A. `read()`

B. `write()`

C. `lseek()`

D. `close()`

✅ **Answer:** **C**

---

### Q4

Every File Management System Call necessarily causes

A. Context Switch

B. Mode Switch

C. Process Creation

D. CPU Scheduling

✅ **Answer:** **B**

---

# PYQ Keywords

Whenever you see these words, think of **File Management**:

- File Descriptor (FD)
- `open()`
- `read()`
- `write()`
- `close()`
- `lseek()`
- File Pointer
- Disk Access
- Kernel
- Mode Switch

---

# Memory Trick 🧠

```text
Open

↓

Get FD

↓

Read / Write

↓

Close
```

---

# 15-Second Revision 🚀

```text
open()  → Open File + FD

read()  → Read using FD

write() → Write using FD

close() → Release FD

All execute in Kernel Mode

↓

Mode Switch ✔
```

---

# Golden Rule ⭐

> **Every File Management System Call executes in Kernel Mode and therefore always causes a Mode Switch.**