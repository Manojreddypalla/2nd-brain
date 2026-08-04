# Systems C — Day 1: How C Maps to Memory

## 1. Core Mental Model

In C, don't think only:

```c
int x = 10;
```

as:

```text
x = 10
```

Think:

```text
        x
        │
        ▼
Memory
┌────────────────┐
│ 4 bytes        │
│ representing   │
│ value 10       │
└────────────────┘
        ▲
        │
       &x
     address
```

A variable is a **named object stored somewhere in memory**.

At runtime, the machine mainly works with:

- bytes
    
- addresses
    
- registers
    
- instructions
    

The name `x` mainly exists to help us write and reason about the program.

---

## 2. Value vs Address

```c
int x = 10;
```

### `x`

```text
x → value of the object
```

Example:

```c
printf("%d\n", x);
```

Output:

```text
10
```

### `&x`

`&` is the **address-of operator**.

```text
&x → memory address where x begins
```

Example:

```c
printf("%p\n", (void *)&x);
```

Possible output:

```text
0x7ffd8c2c9a54
```

The exact address can change between runs.

### Key distinction

```text
x         → value
&x        → address
sizeof(x) → size in bytes
```

---

## 3. Why Types Matter

Memory itself is essentially bytes/bits.

A type tells the compiler how an object should be handled.

```c
char a = 10;
int b = 10;
```

Both represent `10`, but their storage can differ.

Typical system:

```text
char
┌──────┐
│  10  │
└──────┘
 1 byte


int
┌────┬────┬────┬────┐
│    │    │    │    │
└────┴────┴────┴────┘
        4 bytes
```

A type helps determine:

1. **Storage requirement** — how many bytes are needed.
    
2. **Interpretation** — how those bits should be treated.
    

So:

```text
Type
 ↓
size + interpretation of bits
```

---

## 4. `sizeof`

`sizeof` gives the size of a **type or object in bytes**.

```c
sizeof(x)
sizeof(int)
sizeof(double)
```

Example:

```c
int x = 10;

printf("%zu\n", sizeof(x));
```

Typical output:

```text
4
```

Common sizes on many modern systems:

|Type|Typical Size|
|---|--:|
|`char`|1 byte|
|`short`|2 bytes|
|`int`|4 bytes|
|`long`|platform-dependent|
|`long long`|8 bytes|
|`float`|4 bytes|
|`double`|8 bytes|

> [!important]  
> Do not blindly assume these sizes except that `sizeof(char)` is defined as `1`. Use `sizeof` when the actual size matters.

---

## 5. An Object Occupies a Region of Memory

```c
int x = 10;
```

If:

```c
sizeof(x) == 4
```

then `x` occupies **4 bytes of storage**.

Conceptually:

```text
Address
0x1000 ──┐
0x1001   │
0x1002   ├── x
0x1003 ──┘
```

`&x` represents the address where the object `x` begins.

```text
&x
 ↓
0x1000
┌─────────┐
│ byte 1  │
├─────────┤
│ byte 2  │
├─────────┤
│ byte 3  │
├─────────┤
│ byte 4  │
└─────────┘
```

This becomes important later for **pointers and pointer arithmetic**.

---

## 6. Printing Value, Size and Address

```c
#include <stdio.h>

int main(void)
{
    int x = 10;

    printf("value   = %d\n", x);
    printf("size    = %zu bytes\n", sizeof(x));
    printf("address = %p\n", (void *)&x);

    return 0;
}
```

Important format specifiers:

```text
%d   → int
%c   → char
%f   → floating-point output
%zu  → sizeof result (size_t)
%p   → address/pointer
```

For `%p`:

```c
(void *)&x
```

is the standard way to pass the address for printing.

---

## 7. Multiple Variables

```c
int a = 10;
int b = 20;
int c = 30;
```

Each variable is a separate object occupying some storage.

```text
Memory

┌──────────────┐
│      a       │
├──────────────┤
│      ?       │
├──────────────┤
│      b       │
├──────────────┤
│      ?       │
├──────────────┤
│      c       │
└──────────────┘
```

You can inspect their addresses:

```c
printf("a: %p\n", (void *)&a);
printf("b: %p\n", (void *)&b);
printf("c: %p\n", (void *)&c);
```

> [!warning]  
> Do not assume local variables will always appear in source-code order or at predictable addresses. Compiler decisions, alignment, optimization, ABI rules, and other factors can affect layout.

---

## 8. Compilation

```bash
gcc -Wall -Wextra day01.c -o day01
```

### `gcc`

GNU Compiler Collection.

### `-Wall`

Enables a broad and useful collection of compiler warnings.

### `-Wextra`

Enables additional warnings.

### `-o day01`

Sets the output executable name.

Then:

```bash
./day01
```

runs the executable from the current directory.

---

## 9. Systems View

Start translating C mentally like this:

```c
int x = 10;
```

### High-level view

```text
x = 10
```

### Systems view

```text
Create an int object
        ↓
reserve appropriate storage
        ↓
store a representation of 10
        ↓
x names that object
        ↓
&x gives its starting address
```

Later we will connect this model to:

```text
Variables
    ↓
Addresses
    ↓
Pointers
    ↓
Arrays
    ↓
Structs
    ↓
Stack / Heap
    ↓
Kernel / Systems Code
```

---

# Quick Revision

```text
Variable
→ named object stored in memory

x
→ value of x

&x
→ address of x

sizeof(x)
→ size of x in bytes

Type
→ helps determine storage + interpretation

Memory
→ bytes at addresses
```

## Core Mental Model

```text
SOURCE

int x = 10;

        ↓

      Memory

      &x
       │
       ▼
┌─────────────┐
│             │
│      x      │
│             │
│   4 bytes   │
│             │
└─────────────┘
       │
       ▼
   value = 10
```

> [!tip] Day 1 Checkpoint  
> Stop seeing a C variable as only a mathematical name holding a value.
> 
> Start seeing it as an **object occupying bytes at some address in memory**.
> 
> **`x` → value**  
> **`&x` → address**  
> **`sizeof(x)` → storage size**