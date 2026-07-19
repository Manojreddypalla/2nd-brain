# 🎯 GATE Corner — API vs System Call

## Weightage

⭐⭐⭐⭐⭐ **Very Important**

Frequently asked with:

- System Calls
- Library Functions
- POSIX
- User Mode & Kernel Mode
- Mode Switch

---

# Must Remember ⭐⭐⭐⭐⭐

## API (Application Programming Interface)

- Interface used by **programmers**.
- Provided by **libraries** (e.g., C Standard Library).
- Easier to use.
- May or may **not** invoke a System Call.

---

## System Call

- Interface between **User Program** and **Operating System Kernel**.
- Implemented by the **Operating System**.
- Always executes in **Kernel Mode**.
- Always causes a **Mode Switch**.

---

# API vs System Call ⭐⭐⭐⭐⭐

| API | System Call |
|------|-------------|
| Programmer Interface | Kernel Interface |
| Implemented by Libraries | Implemented by OS |
| Usually executes in User Mode | Executes in Kernel Mode |
| May or may not call the kernel | Always enters the kernel |
| May not cause Mode Switch | Always causes Mode Switch |

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

## Every API

```text
API

↓

Maybe System Call

↓

Maybe Kernel
```

❌ **Does NOT always cause a Mode Switch**

Only APIs that invoke a System Call will cause a Mode Switch.

---

# Examples ⭐⭐⭐⭐⭐

| Function | API / System Call | Mode Switch? |
|----------|-------------------|--------------|
| `printf()` | API | ✅ Yes (eventually calls `write()`) |
| `fopen()` | API | ✅ Yes (eventually calls `open()`) |
| `fread()` | API | ✅ Yes (eventually calls `read()`) |
| `malloc()` | API | ⚠️ May cause one (when requesting more memory from the OS) |
| `strlen()` | API | ❌ No |
| `strcmp()` | API | ❌ No |
| `abs()` | API | ❌ No |
| `sqrt()` | API | ❌ No |
| `read()` | System Call | ✅ Yes |
| `write()` | System Call | ✅ Yes |
| `open()` | System Call | ✅ Yes |
| `close()` | System Call | ✅ Yes |

---

# Most Asked Concept ⭐⭐⭐⭐⭐

```text
Programmer

↓

API

↓

System Call

↓

Kernel

↓

Hardware
```

The **API hides the complexity** of the System Call.

---

# GATE Tricks ⚠️

### ❌ Wrong Statement

> Every API is a System Call.

**False**

Many APIs are implemented entirely in user space.

---

### ❌ Wrong Statement

> Every API causes a Mode Switch.

**False**

Only APIs that invoke a System Call.

---

### ❌ Wrong Statement

> Every System Call is an API.

**False**

A System Call is a kernel interface, whereas an API is a programmer interface.

---

### ❌ Wrong Statement

> `strlen()` causes a Mode Switch.

**False**

It operates entirely in user memory.

---

### ❌ Wrong Statement

> `printf()` is a System Call.

**False**

`printf()` is an **API (library function)**.

Internally it eventually invokes **`write()`**, which is the actual System Call.

---

### ✅ Correct Statement

> Every System Call executes in Kernel Mode and therefore causes a Mode Switch.

---

# Common MCQs

### Q1

Which of the following is a System Call?

A. `printf()`

B. `scanf()`

C. `read()`

D. `strlen()`

✅ **Answer:** **C**

---

### Q2

Which of the following is an API?

A. `write()`

B. `read()`

C. `fork()`

D. `printf()`

✅ **Answer:** **D**

---

### Q3

Which statement is correct?

A. Every API causes a Mode Switch.

B. Every System Call causes a Mode Switch.

C. Every API executes in Kernel Mode.

D. APIs are implemented by the kernel.

✅ **Answer:** **B**

---

### Q4

Which function works entirely in User Mode?

A. `read()`

B. `write()`

C. `strlen()`

D. `open()`

✅ **Answer:** **C**

---

### Q5

`printf()` eventually invokes which System Call on Unix/Linux?

A. `fork()`

B. `exec()`

C. `write()`

D. `wait()`

✅ **Answer:** **C**

---

# PYQ Keywords

Whenever you see these words, think of **API vs System Call**:

- API
- Library Function
- C Library
- `printf()`
- `strlen()`
- `read()`
- `write()`
- User Mode
- Kernel Mode
- Mode Switch

---

# Memory Trick 🧠

```text
API

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
API

↓

Library

↓

Maybe System Call

↓

Kernel
```

Remember:

```text
Every API

≠

System Call
```

But

```text
Every System Call

↓

Kernel

↓

Mode Switch ✔
```

---

# Golden Rule ⭐

> **API is for programmers, System Call is for the kernel. APIs may or may not invoke System Calls, but every System Call executes in Kernel Mode and always causes a Mode Switch.**
> 