# Linux Memory Management — Day 3 (Deep Dive)

This topic is one of the foundations of operating systems. If you truly understand it, you'll find processes, Docker, databases, browsers, game engines, and even exploits much easier to reason about.

---

# Big Picture

Imagine you're playing **GTA V**.

The game thinks it has access to a huge amount of memory:

```
Player
World
Cars
NPCs
Textures
Audio
Physics
AI
```

But your laptop only has **16 GB RAM**.

How can the game behave as if it owns a huge, private memory space?

The answer is **Virtual Memory**.

Every process gets its own **virtual address space**—a private illusion created by the operating system. The kernel, together with the CPU's Memory Management Unit (MMU), translates those virtual addresses into actual physical RAM addresses. ([Kernel Internals](https://kernel-internals.org/mm/mmap/?utm_source=chatgpt.com "Process Address Space - Linux Kernel Internals"))

---

# The Memory Pipeline

```
Your Program
      │
Uses virtual addresses
      │
      ▼
Virtual Address Space
      │
CPU's MMU + Page Tables
      │
      ▼
Linux Kernel
      │
Maps pages
      │
      ▼
Physical RAM
```

Your program never directly works with physical RAM.

It only knows virtual addresses.

---

# Step 1 — Physical Memory (RAM)

Physical memory is the real memory chips installed in your computer.

Example:

```
16 GB DDR5 RAM
```

Physically, RAM is just a huge collection of bytes.

```
RAM

Address
0x0000
0x0001
0x0002
...
```

Every byte has a physical address.

Think of RAM as a giant apartment building.

```
Apartment 1
Apartment 2
Apartment 3
...
Apartment 4,294,967,295
```

Each apartment stores data.

---

## Problem Without Virtual Memory

Suppose you run:

```
Chrome
VS Code
Discord
Spotify
Python
```

If all of them directly used physical addresses:

```
Chrome:
Uses RAM 1000-5000

VS Code:
Uses RAM 4500-8000
```

They could overwrite each other's memory.

That would be a disaster.

Modern operating systems prevent this by giving each process its own private virtual address space. ([Wikipedia](https://en.wikipedia.org/wiki/User_space_and_kernel_space?utm_source=chatgpt.com "User space and kernel space"))

---

# Step 2 — Virtual Memory

Virtual memory is an abstraction.

It is **not** extra RAM.

Instead, it is the address space that a process sees.

Imagine every process gets its own fake map:

```
Process A

0x00000000
...
0x7fffffffffff
```

Process B gets what appears to be the same address range:

```
0x00000000
...
0x7fffffffffff
```

Both processes might use the same virtual address, but the kernel maps them to different physical pages, keeping them isolated. ([Wikipedia](https://en.wikipedia.org/wiki/Page_table?utm_source=chatgpt.com "Page table"))

---

# Why Virtual Memory Exists

It solves several problems:

- Memory isolation
    
- Security
    
- Simplifies programming
    
- Allows memory to exceed available RAM by paging inactive pages to disk
    
- Enables efficient sharing (for example, shared libraries or shared memory) ([Wikipedia](https://en.wikipedia.org/wiki/Virtual_memory?utm_source=chatgpt.com "Virtual memory"))
    

---

# Mental Model

Suppose you write:

```cpp
int x = 10;
```

The compiler may place `x` at:

```
Virtual Address

0x7ffe12345678
```

Your program thinks:

> "My variable is at 0x7ffe12345678."

It has no idea where the data actually lives in physical RAM.

The translation happens automatically in hardware using the MMU and page tables. ([Wikipedia](https://en.wikipedia.org/wiki/Page_table?utm_source=chatgpt.com "Page table"))

---

# Address Translation

Suppose:

```
Virtual Address

0x12345678
```

The CPU cannot use this directly.

Instead:

```
Virtual Address

↓

MMU

↓

Page Table

↓

Physical Address

↓

RAM
```

This translation happens on nearly every memory access.

---

# Pages

Linux does not map memory one byte at a time.

It divides memory into fixed-size blocks called **pages**.

Typically:

```
4 KB
```

One page contains:

```
4096 bytes
```

The page table stores mappings like:

```
Virtual Page 15

↓

Physical Page 432
```

This page-based mapping is the basis of modern virtual memory. ([Wikipedia](https://en.wikipedia.org/wiki/Virtual_memory?utm_source=chatgpt.com "Virtual memory"))

---

# Process Memory Layout

A running program's virtual memory is organized into regions.

```
High Address
+----------------------+
| Stack        ↓       |
+----------------------+
|                      |
|      Free Space      |
|                      |
+----------------------+
| Heap         ↑       |
+----------------------+
| Initialized Data     |
+----------------------+
| Global Variables     |
+----------------------+
| Program Code (Text)  |
+----------------------+
Low Address
```

The heap generally grows upward, while the stack grows downward. ([Grainger Course Websites](https://courses.grainger.illinois.edu/cs240/sp2021/notes/virtualMemory-heap-stack.html?utm_source=chatgpt.com "Virtual Memory - Heap and Stack Memory"))

---

# Stack

The stack stores **temporary execution state**.

Each function call creates a new **stack frame**.

Example:

```cpp
void foo() {
    int a = 10;
}
```

Calling `foo()` creates a frame containing:

- Return address
    
- Local variables
    
- Function parameters
    
- Saved registers
    

When `foo()` returns, that frame disappears automatically. ([D3S](https://d3s.mff.cuni.cz/files/teaching/nswi004/text/ch03s02s01.html?utm_source=chatgpt.com "3.2.1. Process Memory Layout"))

---

## Stack Example

```cpp
int add(int a, int b)
{
    int c = a + b;
    return c;
}
```

While executing:

```
Stack

Return Address

a

b

c
```

After the function returns:

```
Stack

(empty)
```

Everything is automatically cleaned up.

---

# Stack Characteristics

- Very fast
    
- Managed automatically
    
- Small (often a few MB per thread)
    
- Last-In, First-Out (LIFO)
    
- Can overflow with excessive recursion or very large local arrays ([GeeksforGeeks](https://www.geeksforgeeks.org/dsa/stack-vs-heap-memory-allocation/?utm_source=chatgpt.com "Stack vs Heap Memory Allocation"))
    

---

# Heap

The heap is used for **dynamic memory allocation**.

Example:

```cpp
int* arr = new int[100];
```

or

```cpp
malloc(...)
```

The memory remains allocated until it is explicitly freed (or garbage-collected in languages with a GC).

```
Heap

Object A

Object B

Large Array

Tree

Graph
```

Unlike the stack, the heap does not automatically clean itself when a function returns. ([D3S](https://d3s.mff.cuni.cz/files/teaching/nswi004/text/ch03s02s01.html?utm_source=chatgpt.com "3.2.1. Process Memory Layout"))

---

# Why We Need the Heap

Consider:

```cpp
void foo()
{
    int arr[1000000];
}
```

This large array lives on the stack and may cause a **stack overflow**.

Instead:

```cpp
int* arr = new int[1000000];
```

allocates it on the heap, which is designed for larger, longer-lived allocations.

---

# Stack vs Heap

|Feature|Stack|Heap|
|---|---|---|
|Allocation|Automatic|Manual (`new`/`malloc`) or runtime-managed|
|Speed|Very fast|Slower|
|Lifetime|Ends when function returns|Until freed|
|Size|Small|Much larger|
|Use|Function calls, local variables|Dynamic objects, large data|

---

# End-to-End Example

```cpp
int main()
{
    int x = 10;
    int* p = new int(20);
}
```

Memory layout:

```
Stack

x
p

↓

Heap

20
```

Here:

- `x` is stored directly on the stack.
    
- `p` (the pointer variable) is also on the stack.
    
- The integer `20` allocated with `new` is on the heap.
    

---

# How This Connects to Linux

When your program asks for more heap memory (through `malloc()` or `new`), the C runtime may request additional virtual memory from the kernel using mechanisms such as `brk`/`sbrk` or `mmap()`. The kernel creates new virtual memory mappings; physical RAM is often not assigned until the pages are actually accessed (demand paging). ([Wikipedia](https://en.wikipedia.org/wiki/Mmap?utm_source=chatgpt.com "Mmap"))

---

# 5-Minute Practical

Run these commands:

```bash
free -h          # Physical RAM and swap usage
cat /proc/meminfo | head
cat /proc/$$/maps
pmap $$
```

Look at `/proc/$$/maps` and notice regions labeled for the executable, heap (`[heap]`), shared libraries, and stack (`[stack]`). This is your shell's virtual address space laid out by the kernel.

---

## Quick Revision

- **Physical Memory (RAM):** The actual memory chips in your computer.
    
- **Virtual Memory:** A private address space for each process, mapped to physical memory by the kernel and MMU.
    
- **Pages:** Fixed-size chunks (commonly 4 KB) used for virtual-to-physical mapping.
    
- **Stack:** Automatic memory for function calls and local variables; fast but limited.
    
- **Heap:** Dynamic memory for objects that need flexible size or lifetime.
    

Once you're comfortable with this, the next logical topics are **page tables, TLB (Translation Lookaside Buffer), page faults, copy-on-write, and `mmap()`**, which explain how Linux makes virtual memory both fast and efficient.