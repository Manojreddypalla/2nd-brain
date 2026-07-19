# 🎯 GATE Corner — Library Function vs System Call

## Weightage

⭐⭐⭐⭐⭐ **Very Important**

Frequently asked with:

- API vs System Call
- POSIX
- User Mode & Kernel Mode
- Mode Switch
- C Library Functions

---

# Must Remember ⭐⭐⭐⭐⭐

## Library Function

- Provided by **Programming Libraries** (e.g., C Standard Library)
- Executes in **User Mode** initially
- Easier for programmers
- **May or may not invoke a System Call**
- **May or may not cause a Mode Switch**

---

## System Call

- Provided by the **Operating System**
- Interface to the **Kernel**
- Executes in **Kernel Mode**
- **Always causes a Mode Switch**

---

# Library Function vs System Call ⭐⭐⭐⭐⭐

| Library Function | System Call |
|------------------|-------------|
| Implemented by libraries | Implemented by OS |
| User-level function | Kernel-level function |
| Runs in User Mode initially | Runs in Kernel Mode |
| May not need OS services | Always needs OS services |
| May or may not invoke a System Call | Already a System Call |
| May or may not cause a Mode Switch | Always causes a Mode Switch |

---

# ⭐ MOST IMPORTANT GATE FACT

## Every System Call

```text
User Mode

↓

Kernel Mode

↓

User Mode
```

✅ **Always causes a Mode Switch**

---

## Every Library Function

```text
Library Function

↓

Maybe System Call

↓

Maybe Kernel
```

❌ **Does NOT always cause a Mode Switch**

---

# Common Examples ⭐⭐⭐⭐⭐

| Function | Type | Mode Switch? |
|----------|------|--------------|
| `printf()` | Library Function | ✅ Yes (calls `write()`) |
| `scanf()` | Library Function | ✅ Yes (calls `read()`) |
| `fopen()` | Library Function | ✅ Yes (calls `open()`) |
| `fclose()` | Library Function | ✅ Yes (calls `close()`) |
| `malloc()` | Library Function | ⚠️ Sometimes |
| `strlen()` | Library Function | ❌ No |
| `strcmp()` | Library Function | ❌ No |
| `memcpy()` | Library Function | ❌ No |
| `sqrt()` | Library Function | ❌ No |
| `read()` | System Call | ✅ Yes |
| `write()` | System Call | ✅ Yes |
| `open()` | System Call | ✅ Yes |
| `close()` | System Call | ✅ Yes |
| `fork()` | System Call | ✅ Yes |
| `exec()` | System Call | ✅ Yes |

---

# Library Function → System Call Mapping

| Library Function | Internally Uses |
|------------------|-----------------|
| `printf()` | `write()` |
| `scanf()` | `read()` |
| `fopen()` | `open()` |
| `fclose()` | `close()` |
| `fread()` | `read()` |
| `fwrite()` | `write()` |

---

# ⭐ Most Asked Concept

```text
Program

↓

Library Function

↓

System Call (Optional)

↓

Kernel

↓

Hardware
```

A **Library Function** may solve the problem itself or invoke a **System Call** if kernel services are required.

---

# GATE Tricks ⚠️

### ❌ Wrong Statement

> Every Library Function is a System Call.

**False**

Library Functions are provided by libraries; they are not kernel interfaces.

---

### ❌ Wrong Statement

> Every Library Function causes a Mode Switch.

**False**

Functions like `strlen()`, `memcpy()`, and `strcmp()` execute entirely in User Mode.

---

### ❌ Wrong Statement

> `printf()` is a System Call.

**False**

It is a **Library Function** that eventually calls the `write()` System Call.

---

### ❌ Wrong Statement

> `malloc()` always invokes a System Call.

**False**

It may satisfy requests from the existing heap. It invokes `brk()`/`mmap()` only when additional memory is needed.

---

### ✅ Correct Statement

> Every System Call executes in Kernel Mode and therefore always causes a Mode Switch.

---

# Common MCQs

### Q1

Which of the following is a Library Function?

A. `read()`

B. `write()`

C. `printf()`

D. `fork()`

✅ **Answer:** **C**

---

### Q2

Which of the following is a System Call?

A. `strlen()`

B. `printf()`

C. `open()`

D. `scanf()`

✅ **Answer:** **C**

---

### Q3

Which statement is correct?

A. Every Library Function causes a Mode Switch.

B. Every System Call causes a Mode Switch.

C. Every Library Function executes in Kernel Mode.

D. Every System Call is implemented by the C Library.

✅ **Answer:** **B**

---

### Q4

Which function executes entirely in User Mode?

A. `write()`

B. `read()`

C. `strlen()`

D. `open()`

✅ **Answer:** **C**

---

### Q5

`printf()` ultimately uses which System Call on Unix/Linux?

A. `fork()`

B. `exec()`

C. `write()`

D. `wait()`

✅ **Answer:** **C**

---

# PYQ Keywords

If you see these words, think of **Library Function vs System Call**:

- C Standard Library
- libc
- `printf()`
- `scanf()`
- `strlen()`
- `malloc()`
- User Mode
- Kernel Mode
- Mode Switch
- `read()`
- `write()`

---

# Memory Trick 🧠

```text
Library Function

↓

Maybe Kernel

↓

Maybe Mode Switch
```

```text
System Call

↓

Kernel

↓

Mode Switch ✔
```

---

# 20-Second Revision 🚀

```text
Library Function

↓

Library

↓

Maybe System Call

↓

Kernel
```

```text
System Call

↓

Kernel

↓

Always Mode Switch ✔
```

---

# Golden Rule ⭐

> **Library Functions are user-level helper functions provided by libraries and may or may not invoke System Calls. System Calls are kernel interfaces provided by the Operating System, always execute in Kernel Mode, and always cause a Mode Switch.**