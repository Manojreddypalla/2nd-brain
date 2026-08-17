# C Programming — Memory Layout of a Program + Virtual Memory

## Notes up to Page 17

### 1. Memory Layout of a Program

When a C program is compiled and executed, its memory is organized into different regions.

Basic layout:

```text
        High Address
┌─────────────────────┐
│       Stack         │
│         ↓           │
│                     │
│         ↑           │
│       Heap          │
├─────────────────────┤
│   Static / Data     │
├─────────────────────┤
│   Code / Text       │
└─────────────────────┘
        Low Address
```

The **Stack, Heap, and Static area** together form the major memory regions discussed in the lecture.

---

## 2. Virtual Memory

The memory shown to a program is **virtual memory**.

A process does not directly see the entire physical RAM as one continuous memory space.

The program works with its own **virtual address space**.

Example:

```text
Process / Program
        │
        ▼
Virtual Memory
┌──────────────┐
│    Stack     │
│              │
│    Heap      │
│              │
│    Static    │
│              │
│    Code      │
└──────────────┘
```

The lecture emphasizes that virtual memory works largely **silently and automatically**, without requiring intervention from the application programmer.

---

# 3. Stack

The **Stack** stores:

- Local variables
    
- Function activation records / function call information
    

### Important properties

- Stack size **changes at runtime**.
    
- Function calls create stack frames/activation records.
    
- Returning from a function removes its activation information.
    

Example:

```c
int main()
{
    int y;
    char *str;
}
```

`y` and `str` are local variables, so their storage is associated with the stack.

---

# 4. Heap

The **Heap** stores **dynamically allocated data**.

Example:

```c
char *str;

str = malloc(100 * sizeof(char));
```

The memory requested using `malloc()` comes from the **heap**.

```text
Stack
┌──────────────┐
│ main()       │
│ y            │
│ str ────────────────┐
└──────────────┘      │
                      ▼
                  Heap
              ┌────────────┐
              │ dynamically│
              │ allocated  │
              │ memory     │
              └────────────┘
```

### Important property

Heap size **changes at runtime**.

Memory can be dynamically allocated and later released using functions such as:

```c
malloc()
free()
```

The lecture specifically uses `malloc()` to illustrate heap allocation.

---

# 5. Static Area

The **Static area** contains:

- Static variables
    
- Global variables
    
- Program code in the lecture's simplified memory layout
    

Its size is considered **fixed at runtime** in the lecture's model.

The lecture later depicts the static portion as containing:

```text
Static
├── Data segment
└── Code segment
```

---

# 6. Code Segment

The **Code segment / Text segment** contains the program's instructions.

For example:

```c
printf("Hello");
```

The machine instructions generated for the program are stored in the code/text region.

So:

```text
Code Segment
    ↓
Program instructions
```

The lecture's memory diagram places the **Code segment at the lower portion**, followed by the data/static region.

---

# 7. Data Segment

The data segment belongs to the static portion of the program's memory.

It is associated with **static/global data**.

Simplified layout:

```text
┌──────────────┐
│    Stack     │
├──────────────┤
│              │
│     Heap     │
├──────────────┤
│ Data Segment │
├──────────────┤
│ Code Segment │
└──────────────┘
```

---

# 8. Why Stack and Heap Have Different Roles

Think of them as two different allocation strategies.

### Stack

```text
Function call
     ↓
Local variables
     ↓
Function returns
     ↓
Storage disappears
```

### Heap

```text
malloc()
   ↓
Dynamic memory allocated
   ↓
Memory remains allocated
   ↓
free()
   ↓
Memory released
```

The lecture demonstrates that the heap is useful when dynamically allocating memory, while stack storage is associated with function calls and local variables.

---

# 9. Stack Boundary and Heap Boundary

The lecture shows the stack and heap growing toward each other:

```text
        Stack
          ↓
          ↓
     ───────────  ← Stack boundary

        Free
       space

     ───────────  ← Heap boundary
          ↑
          ↑
        Heap
```

This creates an important idea:

> **Stack and heap dynamically use the available virtual address space.**

If the stack has grown sufficiently, there may not be enough room to continue allocating additional stack frames.

Similarly, heap allocation cannot continue indefinitely.

The lecture illustrates this with multiple function calls filling the stack and `malloc()` consuming heap space.

---

# 10. Design Choice — Stack vs Heap

The lecture discusses the design choice between stack and heap boundaries.

Example:

```c
main()
{
    fun1();
    fun2();
    fun3();
}
```

As functions are called:

```text
main()
 ├── fun1()
 ├── fun2()
 └── fun3()
```

their activation records occupy stack space.

If the stack becomes full, another function call may not be possible even if there is unused space elsewhere in the virtual address space.

Similarly, heap allocation consumes heap space.

Therefore, **stack and heap boundaries are an important design consideration.**

---

# 11. Program Memory vs Physical Memory

The lecture presents memory as an **illusion from the program's point of view**.

Multiple processes can each have their own memory layout:

```text
Process 1        Process 2        Process 3

Memory           Memory           Memory
┌───────┐        ┌───────┐        ┌───────┐
│ Stack │        │ Stack │        │ Stack │
│ Heap  │        │ Heap  │        │ Heap  │
│ Data  │        │ Data  │        │ Data  │
│ Code  │        │ Code  │        │ Code  │
└───────┘        └───────┘        └───────┘

 CPU              CPU              CPU
Registers        Registers        Registers
```

This is why each process can behave as though it has its own memory.

The lecture explicitly introduces this as an **illusion** and connects it to virtual memory.

---

# GATE QUICK REVISION

### Memory regions

|Region|Main purpose|Size|
|---|---|---|
|**Stack**|Local variables + function activation records|Changes at runtime|
|**Heap**|Dynamically allocated data|Changes at runtime|
|**Static area**|Static/global data + code representation in lecture|Fixed at runtime|
|**Code/Text**|Program instructions|Fixed|
|**Data**|Static/global data|Fixed|

### Remember

```text
Stack → Local + Function calls
Heap → Dynamic allocation
Static → Static + Global
Code → Instructions
Virtual Memory → Process's logical view of memory
```

**Pages 1–17 covered only.**