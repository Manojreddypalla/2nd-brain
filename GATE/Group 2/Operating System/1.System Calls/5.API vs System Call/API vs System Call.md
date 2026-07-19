## Definition

### API (Application Programming Interface)

> An **API** is a set of functions provided by libraries that applications use to access operating system services or other software functionality.

An API is **what the programmer writes in code**.

---

### System Call

> A **System Call** is the mechanism through which a user program requests a service from the Operating System Kernel.

A System Call is **what the Operating System executes**.

---

# The Big Picture

A beginner often thinks:

```text
printf()

↓

Operating System
```

This is **not** what actually happens.

The real flow is

```text
Application

↓

API (Library Function)

↓

System Call

↓

Kernel

↓

Hardware
```

The API hides the complexity of the System Call.

---

# Why Do We Need APIs?

Imagine if programmers had to directly invoke every System Call.

They would need to know

- System Call numbers
- CPU registers
- Calling conventions
- Architecture-specific details

Instead,

they simply call

```c
printf();
fopen();
malloc();
```

The API handles the rest.

---

# How It Works

Suppose you write

```c
printf("Hello");
```

Actually,

```text
printf()

↓

C Standard Library (libc)

↓

write()

↓

Kernel

↓

Screen
```

Notice

- `printf()` is an **API (Library Function)**
- `write()` is the **System Call**

---

# Another Example

Reading a file

```c
fread()
```

Internally

```text
fread()

↓

C Library

↓

read()

↓

Kernel

↓

Disk
```

Again

- `fread()` → API
- `read()` → System Call

---

# Memory Allocation Example

You write

```c
malloc(1024);
```

Internally,

the C library may request memory using system calls such as `brk()` or `mmap()` (implementation depends on the allocator and operating system).

```text
malloc()

↓

C Library

↓

brk()/mmap()

↓

Kernel

↓

Memory Allocated
```

---

# Relationship

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

---

# API vs System Call

| API | System Call |
|------|-------------|
| Interface for programmers | Interface to the kernel |
| Implemented by libraries | Implemented by Operating System |
| May or may not invoke a System Call | Always enters the kernel |
| Easier to use | Lower-level mechanism |
| Portable across OS implementations | OS-specific implementation |
| Executes in User Mode initially | Executes in Kernel Mode |

---

# Does Every API Invoke a System Call?

## No.

This is one of the favorite GATE concepts.

Example

```c
strlen("Operating")
```

It simply counts characters in memory.

No kernel service is needed.

```text
strlen()

↓

User Mode

↓

Done
```

No System Call.

No Mode Switch.

---

Another example

```c
abs(-15)
```

Only CPU arithmetic.

Again,

No System Call.

---

# APIs That Usually Invoke System Calls

Examples

```text
printf()

↓

write()
```

```text
fopen()

↓

open()
```

```text
fread()

↓

read()
```

```text
malloc()

↓

brk()/mmap()   (when more memory is needed)
```

These eventually invoke the kernel.

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

Always.

---

# Does Every API Cause a Mode Switch?

## NO ⭐⭐⭐⭐⭐

Only APIs that invoke a System Call.

Examples

| API | Mode Switch? |
|------|--------------|
| `printf()` | Yes |
| `fopen()` | Yes |
| `fread()` | Yes |
| `malloc()` | Usually, when requesting more memory |
| `strlen()` | No |
| `strcmp()` | No |
| `abs()` | No |
| `sqrt()` | No |

---

# Real-Life Analogy

Imagine a restaurant.

```text
Customer

↓

Waiter

↓

Chef

↓

Kitchen
```

Customer → Program

Waiter → API

Chef → System Call

Kitchen → Kernel

The customer talks to the waiter.

The waiter talks to the chef.

The chef works inside the kitchen.

---

# Key Points

- API is used by programmers.
- System Call is used by the Operating System.
- APIs simplify programming.
- APIs may internally invoke one or more System Calls.
- Every System Call executes in Kernel Mode.
- Every System Call causes a Mode Switch.
- Not every API causes a Mode Switch.

---

# Example Flow

```text
printf()

↓

C Library

↓

write()

↓

Kernel

↓

Display

↓

Return
```

---

# Another Example

```text
strlen()

↓

CPU

↓

Return Length
```

No kernel.

No System Call.

No Mode Switch.

---

# One-Line Revision

> **An API is a programmer-friendly interface, while a System Call is the mechanism used to request services from the Operating System kernel. APIs may internally invoke System Calls, but not all APIs do.**