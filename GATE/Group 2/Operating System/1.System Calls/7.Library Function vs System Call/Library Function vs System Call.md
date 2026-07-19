# Module 1.10 — Library Function vs System Call

## Definition

### Library Function

> A **Library Function** is a pre-written function provided by a programming library (such as the C Standard Library) to help programmers perform common tasks.

A library function executes in **User Mode** and **may or may not invoke a System Call**.

Examples:

```c
printf()
scanf()
strlen()
malloc()
sqrt()
fopen()
```

---

### System Call

> A **System Call** is the mechanism through which a user program requests services from the Operating System Kernel.

A System Call always executes in **Kernel Mode**.

Examples:

```c
read()
write()
open()
close()
fork()
exec()
wait()
```

---

# Why Do We Need Library Functions?

Imagine writing a program without libraries.

To print

```text
Hello World
```

you would have to

- Know system call numbers
- Load CPU registers
- Invoke kernel trap
- Handle return values

Instead, you simply write

```c
printf("Hello");
```

The library hides all this complexity.

---

# Relationship

```text
Program

↓

Library Function

↓

(Optional)

↓

System Call

↓

Kernel

↓

Hardware
```

---

# Example 1 — printf()

You write

```c
printf("Hello");
```

Actually,

```text
printf()

↓

C Library

↓

write()

↓

Kernel

↓

Terminal
```

Here

- `printf()` → Library Function
- `write()` → System Call

---

# Example 2 — fopen()

```text
fopen()

↓

Library

↓

open()

↓

Kernel
```

---

# Example 3 — strlen()

```c
strlen("Operating System")
```

```text
strlen()

↓

CPU counts characters

↓

Return Length
```

Notice

- No kernel
- No System Call
- No Mode Switch

---

# Example 4 — sqrt()

```c
sqrt(64)
```

```text
sqrt()

↓

CPU

↓

Answer = 8
```

Again,

No kernel involved.

---

# Example 5 — malloc()

```c
malloc(1024);
```

There are two possibilities:

### Small allocation

```text
malloc()

↓

Uses Existing Heap

↓

Return Pointer
```

No System Call.

---

### Large allocation

```text
malloc()

↓

mmap() / brk()

↓

Kernel

↓

Memory Allocated
```

A System Call occurs.

---

# Execution Comparison

## Library Function

```text
Program

↓

Library Function

↓

Return
```

Sometimes

```text
Program

↓

Library Function

↓

System Call

↓

Kernel

↓

Return
```

---

## System Call

```text
Program

↓

System Call

↓

Kernel

↓

Return
```

A System Call **always** enters the kernel.

---

# Library Function vs System Call

| Library Function | System Call |
|------------------|-------------|
| Provided by programming libraries | Provided by the Operating System |
| Executes in User Mode initially | Executes in Kernel Mode |
| Easier for programmers | Low-level interface |
| May not require the OS | Always requires the OS |
| May or may not invoke a System Call | Already a System Call |
| May or may not cause a Mode Switch | Always causes a Mode Switch |
| Usually more portable | OS implementation dependent |

---

# Does Every Library Function Cause a Mode Switch?

## NO ⭐⭐⭐⭐⭐

Examples

| Library Function | Mode Switch? |
|------------------|--------------|
| `printf()` | ✅ Yes (eventually calls `write()`) |
| `scanf()` | ✅ Yes (eventually calls `read()`) |
| `fopen()` | ✅ Yes (eventually calls `open()`) |
| `malloc()` | ⚠️ Sometimes |
| `strlen()` | ❌ No |
| `strcmp()` | ❌ No |
| `memcpy()` | ❌ No |
| `sqrt()` | ❌ No |
| `abs()` | ❌ No |

---

# Does Every System Call Cause a Mode Switch?

## YES ⭐⭐⭐⭐⭐

Every System Call

```text
User Mode

↓

Kernel Mode

↓

User Mode
```

Examples

```text
fork()

exec()

wait()

open()

read()

write()

close()
```

All cause a **Mode Switch**.

---

# Real-Life Analogy

```text
Customer

↓

Receptionist

↓

Manager

↓

Secure Office
```

- Customer → Program
- Receptionist → Library Function
- Manager → System Call
- Secure Office → Kernel

Sometimes the receptionist solves your problem directly.

Sometimes the receptionist must ask the manager.

The manager always enters the secure office.

---

# Key Points

- Library Functions are provided by libraries.
- System Calls are provided by the Operating System.
- Library Functions simplify programming.
- Some Library Functions execute entirely in User Mode.
- Every System Call executes in Kernel Mode.
- Every System Call causes a Mode Switch.
- Library Functions may internally invoke one or more System Calls.

---

# Common Examples

| Library Function | Internally Uses |
|------------------|-----------------|
| `printf()` | `write()` |
| `scanf()` | `read()` |
| `fopen()` | `open()` |
| `fclose()` | `close()` |
| `fread()` | `read()` |
| `fwrite()` | `write()` |

---

# One-Line Revision

> **A Library Function is a programmer-friendly function provided by libraries that may internally invoke a System Call, whereas a System Call is the kernel interface that always executes in Kernel Mode and always causes a Mode Switch.**