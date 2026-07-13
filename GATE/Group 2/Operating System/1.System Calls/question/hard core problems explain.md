# GATE OS — System Calls Practice Questions (Obsidian Notes)

> **Study Method:**  
> For each question:
> 
> 1. Read every option.
>     
> 2. Ask **"Why is this correct or incorrect?"**
>     
> 3. Then check the answer.
>     
> 
> GATE often tests the **reasoning behind wrong options**, not just the correct one.

---

# Q1. User Mode → Kernel Mode (MSQ)

### Question

Which one or more of the following options **guarantee** that a computer system will transition from **User Mode** to **Kernel Mode**?

- A) Function Call
    
- B) `malloc()` Call
    
- C) Page Fault
    
- D) System Call
    

---

## Option A) Function Call

❌ **Incorrect**

A normal function call simply transfers control from one function to another within the **same process**.

Example:

```cpp
add(a, b);
```

The CPU:

- Pushes the return address onto the stack.
    
- Jumps to the function.
    
- Continues execution.
    

Everything happens in **User Mode**.

A function call **does not require the operating system**.

---

## Option B) `malloc()` Call

❌ **Incorrect (Not Guaranteed)**

`malloc()` is a **C library function**, **not a system call**.

It has two possible behaviors:

### Case 1: Free heap memory already exists

```text
malloc()
    ↓
Returns memory
```

No kernel involvement.

---

### Case 2: Heap needs to grow

```text
malloc()
      ↓
brk() / mmap()
      ↓
Kernel
```

Now a system call occurs.

Since it **may or may not** enter the kernel, it is **not guaranteed**.

---

## Option C) Page Fault

✅ **Correct**

A page fault occurs when the CPU accesses a virtual memory page that is not currently available in RAM or violates memory permissions.

The CPU cannot handle this by itself.

It raises an exception:

```text
User Program
      ↓
Page Fault
      ↓
Kernel
      ↓
Loads page / checks permissions
      ↓
Returns to User Mode
```

Every page fault causes the CPU to enter **Kernel Mode**.

---

## Option D) System Call

✅ **Correct**

A system call is the official interface between a user program and the operating system.

Examples:

- `read()`
    
- `write()`
    
- `fork()`
    
- `exec()`
    

Whenever a process requests one of these services:

```text
User Mode
      ↓
Kernel Mode
      ↓
System Call Handler
      ↓
User Mode
```

A mode switch **always** happens.

---

## Answer

✅ **C and D**

---

## Key Concepts

Remember:

|Operation|Kernel Mode?|
|---|---|
|Normal function call|❌ No|
|`malloc()`|⚠️ Sometimes|
|Page Fault|✅ Always|
|System Call|✅ Always|

---

# Q2. About `fork()`

### Question

Which one or more of the following statements regarding `fork()` are TRUE?

- A) `fork()` creates a new process.
    
- B) The child process gets a copy of the parent's address space.
    
- C) Parent and child always execute in a fixed order.
    
- D) `fork()` returns twice.
    

---

## Option A) `fork()` creates a new process

✅ **Correct**

`fork()` duplicates the currently running process.

Before:

```text
Parent
```

After:

```text
Parent
    \
     Child
```

A completely new process is created.

---

## Option B) Child gets a copy of the parent's address space

✅ **Correct**

Initially the child receives copies of:

- Code segment
    
- Data segment
    
- Heap
    
- Stack
    
- Global variables
    

Modern operating systems optimize this using **Copy-On-Write (COW)**, meaning physical copying is delayed until one process modifies memory.

Conceptually, the child starts with the same memory contents as the parent.

---

## Option C) Parent and child always execute in a fixed order

❌ **Incorrect**

After `fork()`, both processes become **ready to run**.

The scheduler decides which process runs first.

Possible execution:

```text
Parent first
```

or

```text
Child first
```

or

```text
Parent
Child
Parent
Child
```

The execution order is **never guaranteed**.

---

## Option D) `fork()` returns twice

✅ **Correct**

`fork()` is called once but returns in **both processes**.

Parent receives:

```text
Child PID
```

Child receives:

```text
0
```

This allows programs to distinguish parent from child.

---

## Answer

✅ **A, B and D**

---

## Key Concepts

|Statement|True?|Reason|
|---|---|---|
|Creates process|✅|New child process|
|Copies address space|✅|Uses Copy-On-Write|
|Fixed execution order|❌|Scheduler decides|
|Returns twice|✅|Parent and child both return|

---

# Q3. Understanding Memory After `fork()`

### Question

Consider the following program:

```cpp
int x = 10;

if (fork() == 0)
    x = x + 5;
else
    x = x - 5;

printf("%d\n", x);
```

Which of the following statements is correct?

- A) Parent prints 5, Child prints 15
    
- B) Parent prints 15, Child prints 5
    
- C) Both print 10
    
- D) Undefined behavior
    

---

## Before `fork()`

```text
Parent

x = 10
```

---

## After `fork()`

Two independent copies exist.

```text
Parent

x = 10
```

```text
Child

x = 10
```

Changing one copy **does not affect the other**.

---

## Option A) Parent prints 5, Child prints 15

✅ **Correct**

Parent executes:

```cpp
x = x - 5;
```

Result:

```text
5
```

Child executes:

```cpp
x = x + 5;
```

Result:

```text
15
```

---

## Option B) Parent prints 15, Child prints 5

❌ **Incorrect**

The parent never enters the child branch.

The child never enters the parent branch.

The updates cannot be reversed.

---

## Option C) Both print 10

❌ **Incorrect**

Both processes modify their own copy of `x`.

Neither retains the original value.

---

## Option D) Undefined behavior

❌ **Incorrect**

The program is perfectly valid.

Only the **order** of printing is unpredictable.

The printed values are deterministic.

---

## Answer

✅ **A**

---

## Key Concepts

After `fork()`:

```text
Before

Parent
x = 10
```

↓

```text
After

Parent
x = 10

Child
x = 10
```

↓

```text
Parent modifies only Parent's copy

Child modifies only Child's copy
```

Memory is **not shared** after `fork()` (unless explicitly using shared memory).

---

> Continue this exact format for Q4–Q20 in your Obsidian vault. Each question becomes a self-contained note with:
> 
> - The original GATE question
>     
> - A conceptual explanation of every option
>     
> - The correct answer
>     
> - A short "Key Concepts" summary for revision. This style works especially well for active recall before GATE.
>