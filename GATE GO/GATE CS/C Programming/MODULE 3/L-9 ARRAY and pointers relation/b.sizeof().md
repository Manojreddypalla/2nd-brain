Yep — you’re talking about the **`sizeof` operator section** from your lecture, especially the part about **64-bit systems and pointers**. Here are clean GATE notes based on the PDF.

## `sizeof` Operator — GATE Notes

### 1. Basic idea

`sizeof` tells us the **memory size in bytes** occupied by a type/object.

```c
sizeof(int)
sizeof(a)
sizeof(pointer)
```

**Important:** `sizeof` gives the answer in **bytes**, NOT bits.

---

### 2. `sizeof` is evaluated at compile time

```c
int i = 1;

sizeof(i++);
```

The compiler already knows the size of `i` from its type.

Therefore:

```text
i++ is NOT executed
```

So `i` remains `1`.

### Mental model

Think:

```text
sizeof(expression)
       ↓
Compiler asks:
"What TYPE would this expression have?"
       ↓
"What is the size of that type?"
```

It generally doesn't need to execute the expression.

---

## 3. `sizeof(variable)`

```c
int a;

sizeof(a);
```

If `int = 4 bytes`:

```text
sizeof(a) = 4 bytes
```

Same as:

```c
sizeof(int)
```

The lecture demonstrates this with `int a`, giving `4`.

---

## 4. `sizeof(expression)`

You can give an expression too:

```c
sizeof(long int + int)
sizeof(char + int)
sizeof(char + char)
```

The **type of the expression** determines the result.

So don't simply look at the individual operands.

Think:

```text
expression
    ↓
What is its resulting TYPE?
    ↓
sizeof(that type)
```

---

# 5. `sizeof(array)`

This is VERY important for GATE.

```c
int a[10];
```

If:

```text
sizeof(int) = 4 bytes
```

then:

```c
sizeof(a) = 10 × 4
          = 40 bytes
```

General formula:

```text
sizeof(array)
    = number of elements × sizeof(one element)
```

The lecture explicitly demonstrates `int a[10]` giving `40`.

---

# 6. `sizeof(pointer)` ⭐

This is where your **32-bit vs 64-bit** point comes in.

```c
int *p;
```

`p` stores an **address**, not an `int`.

Therefore:

```c
sizeof(p)
```

depends on the **architecture/address size**, not on `sizeof(int)`.

### 32-bit system

```text
Address = 32 bits
        = 4 bytes

sizeof(p) = 4 bytes
```

### 64-bit system

```text
Address = 64 bits
        = 8 bytes

sizeof(p) = 8 bytes
```

The lecture specifically notes that on a **64-bit system**, the pointer/address requires **8 bytes**.

### ⭐ Remember this

```text
32-bit architecture → pointer = 4 bytes
64-bit architecture → pointer = 8 bytes
```

**Not:**

```text
int pointer → sizeof(int)
```

❌ Wrong.

---

# 7. Array vs Pointer — VERY IMPORTANT

```c
int a[10];
int *p = a;
```

Assume:

```text
int = 4 bytes
64-bit system
```

Then:

```c
sizeof(a) = 10 × 4 = 40 bytes

sizeof(p) = 8 bytes
```

Why?

```text
a
↓
Actual array containing 10 integers

p
↓
Only a variable containing an address
```

Visualize:

```text
a:
┌────┬────┬────┬────┬──── ... ────┐
│int │int │int │int │      10     │
└────┴────┴────┴────┴──────────────┘
          40 bytes

p:
┌──────────────┐
│   address    │  ← 8 bytes on 64-bit
└──────────────┘
```

The lecture also contrasts the allocated array size with pointer size.

---

# 🔥 GATE Cheat Sheet

|Expression|Meaning|
|---|---|
|`sizeof(int)`|Size of `int`|
|`sizeof(x)`|Size of object `x`|
|`sizeof(a)` where `a` is array|**Entire array size**|
|`sizeof(p)` where `p` is pointer|**Pointer/address size**|
|32-bit pointer|**4 bytes**|
|64-bit pointer|**8 bytes**|
|`sizeof` result|**Bytes**|
|`sizeof` evaluation|**Compile time**|
|`sizeof(i++)`|`i++` normally **not executed**|

### The one pattern you should lock in

```text
sizeof(array)
       ↓
number of elements × element size

sizeof(pointer)
       ↓
size of address
       ↓
32-bit → 4 bytes
64-bit → 8 bytes
```

**Most important GATE trap:**  
`sizeof(a)` and `sizeof(p)` are **NOT the same**, even when `p = a`.