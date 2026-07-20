# Thread Libraries

> **Goal:** Understand why Thread Libraries exist, how they work internally, their relationship with User-Level Threads (ULTs) and Kernel-Level Threads (KLTs), and their role in multithreaded programming.

---

# Why Were Thread Libraries Introduced?

Creating and managing threads manually is extremely difficult.

Imagine if every programmer had to:

- Allocate memory for each thread
- Create a Thread Control Block (TCB)
- Maintain thread states
- Switch between threads
- Synchronize execution
- Destroy finished threads

This would make programming very complex.

To simplify thread management, operating systems and programming environments provide **Thread Libraries**.

A Thread Library provides a set of APIs (functions) that allow programmers to easily create, manage, synchronize, and terminate threads without dealing with low-level implementation details.

---

# Definition

A **Thread Library** is a collection of functions (APIs) used to create, control, synchronize, and terminate threads.

Instead of writing low-level thread management code, programmers simply call library functions.

Example:

```c
pthread_create();
```

Instead of manually creating a thread inside the kernel.

---

# Core Idea

```
Application Program

↓

Thread Library

↓

Operating System

↓

CPU
```

The Thread Library acts as an interface between the application and the operating system.

---

# Internal Working

## Step 1

Application wants a new thread.

```
Program

↓

Create Thread
```

---

## Step 2

Program calls the Thread Library.

Example:

```c
pthread_create()
```

---

## Step 3

The Thread Library decides what to do depending on the thread model.

### User-Level Threads

```
Application

↓

Thread Library

↓

Creates thread itself
```

The kernel is never involved.

---

### Kernel-Level Threads

```
Application

↓

Thread Library

↓

System Call

↓

Kernel creates thread
```

The library requests the kernel to create a Kernel Thread.

---

## Step 4

Thread becomes READY.

Scheduler eventually executes it.

---

# Responsibilities of a Thread Library

A Thread Library provides APIs for:

## Thread Creation

Example:

```c
pthread_create()
```

Creates a new thread.

---

## Thread Termination

Example:

```c
pthread_exit()
```

Ends the current thread.

---

## Waiting for Thread Completion

Example:

```c
pthread_join()
```

Waits until another thread finishes execution.

---

## Synchronization

Provides synchronization mechanisms like:

- Mutex
- Condition Variables
- Read-Write Locks
- Barriers

Used to avoid race conditions.

---

## Thread Attributes

Allows configuration of:

- Stack Size
- Priority
- Scheduling Policy
- Detached/Joinable State

---

# User-Level vs Kernel-Level Thread Libraries

## User-Level Thread Library

```
Application

↓

Thread Library

↓

CPU
```

Characteristics:

- Kernel unaware
- Very fast
- No system calls for thread operations
- Used in Many-to-One model

---

## Kernel-Level Thread Library

```
Application

↓

Thread Library

↓

Kernel

↓

CPU
```

Characteristics:

- Uses system calls
- Kernel creates threads
- Supports parallel execution
- Used in One-to-One model

---

# Common Thread Library Functions (POSIX Threads)

| Function | Purpose |
|----------|---------|
| pthread_create() | Create a new thread |
| pthread_exit() | Terminate current thread |
| pthread_join() | Wait for another thread |
| pthread_self() | Get current thread ID |
| pthread_cancel() | Request cancellation of a thread |
| pthread_mutex_lock() | Lock a mutex |
| pthread_mutex_unlock() | Unlock a mutex |

> **Note:** GATE usually tests the *purpose* of these functions rather than their syntax.

---

# Types of Thread Libraries

## 1. POSIX Threads (Pthreads)

- Standard for UNIX/Linux systems.
- Defined by the POSIX standard.
- Most common library in Linux.
- Supports Kernel-Level Threads.

---

## 2. Win32 Threads

- Native thread library for Windows.
- Uses Windows API.
- Supports Kernel-Level Threads.

---

## 3. Java Thread Library

- Built into Java.
- Programmer uses:

```java
Thread
```

or

```java
Runnable
```

The JVM maps Java threads to OS threads (modern JVMs typically use native threads).

---

# Advantages

- Easy thread creation.
- Hides low-level implementation.
- Portable APIs.
- Provides synchronization primitives.
- Reduces programmer effort.

---

# Disadvantages

- Additional abstraction layer.
- Performance depends on implementation.
- User-Level Thread libraries cannot overcome the limitations of ULTs.

---

# Relationship with Multithreading Models

| Model | Role of Thread Library |
|--------|------------------------|
| Many-to-One | Manages all ULTs in user space |
| One-to-One | Calls kernel to create KLTs |
| Many-to-Many | Maps ULTs to multiple KLTs |
| Two-Level | Manages mappings and bound threads |

---

# Real-World Example

Suppose a web browser wants to:

- Download a file
- Play music
- Render a webpage

The programmer simply writes:

```
Create Download Thread

Create Render Thread

Create Audio Thread
```

The Thread Library handles the complex details of creating and managing those threads.

---

# GATE Corner ⭐

## Must Remember

- Thread Library = Collection of APIs for thread management.
- It acts as an interface between the application and the operating system.
- It provides APIs for:
  - Thread creation
  - Termination
  - Synchronization
  - Joining
  - Thread attributes
- In ULT, the library manages threads itself.
- In KLT, the library requests the kernel to manage threads.

---

## Common GATE Traps ⚠️

❌ Thread Library and Thread Scheduler are the same.

✔ False.

A Thread Library provides APIs.
A Scheduler decides which thread executes next.

---

❌ Thread Library always runs in the kernel.

✔ False.

User-Level Thread libraries run entirely in user space.

---

❌ POSIX Threads is a scheduling algorithm.

✔ False.

It is a **thread library/API standard**.

---

❌ Thread Library creates CPU threads directly.

✔ False.

It either manages ULTs itself or requests the kernel to create KLTs.

---

# PYQ Focus 🎯

- Purpose of Thread Libraries
- ULT vs KLT library behavior
- POSIX Threads (Pthreads)
- Win32 Threads
- Java Threads
- Thread creation APIs
- Synchronization APIs
- Difference between Thread Library and Scheduler