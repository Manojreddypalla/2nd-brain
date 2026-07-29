# Chapter 1 — Variables, Memory & Addresses

> **Goal:** Stop thinking of variables as names and start thinking of them as locations in memory.

---

# 1. The Big Picture

When you're writing C++, you're really giving instructions to the CPU to work with **memory**.

Most beginners think like this:

```cpp
int age = 20;
```

> "I created a variable."

That's true...

But internally, something much more interesting happened.

The compiler sees something like this:

```
"I need 4 bytes of memory."

↓

Reserve those 4 bytes.

↓

Call that location "age".

↓

Store 20 inside it.
```

The variable name exists only for **you and the compiler**.

The CPU doesn't know what `age` is.

The CPU only knows:

```
Address 0x61FF08

↓

Contains

20
```

---

# Mental Model

Imagine RAM is a giant apartment building.

```
Memory (RAM)

+------------------+
| Apartment 1000   |
+------------------+
| Apartment 1004   |
+------------------+
| Apartment 1008   |
+------------------+
| Apartment 1012   |
+------------------+
```

Every apartment has

- an address
    
- some space inside
    

Variables simply occupy apartments.

---

# Example

```cpp
int age = 20;
```

Memory might become

```
Address        Value

0x61FF08 ----> 20
```

The compiler remembers

```
age

↓

0x61FF08
```

Whenever you write

```cpp
age
```

the compiler replaces it internally with

```
Go to address

0x61FF08
```

---

# Important

The variable **is not the value.**

Nor is it the address.

It is simply a **name** attached to a memory location.

Think

```
Variable Name

↓

Label

↓

Memory

↓

Actual Data
```

---

# Creating Multiple Variables

```cpp
int a = 10;
int b = 25;
```

Memory could look like

```
Stack Memory

Address        Value

0x61FF08 -----> 10

0x61FF0C -----> 25
```

Notice

```
a

↓

10


b

↓

25
```

Different variables

Different addresses

---

# Why are addresses different?

Because every object occupies memory.

For an `int`

```
sizeof(int)

↓

4 bytes
```

So

```
Address

100

↓

104

↓

108

↓

112
```

Each integer starts 4 bytes later.

---

# What is RAM?

RAM (Random Access Memory) is simply a very large collection of bytes.

Think of it as

```
Byte 0

Byte 1

Byte 2

Byte 3

Byte 4

...
```

An `int`

```
4 bytes
```

A `double`

```
8 bytes
```

A `char`

```
1 byte
```

Every object simply occupies some consecutive bytes.

---

# Visualizing an Integer

Suppose

```cpp
int a = 10;
```

Decimal

```
10
```

Binary

```
00000000
00000000
00000000
00001010
```

Memory stores bytes.

Not decimal numbers.

So internally

```
Byte 0

00001010

Byte 1

00000000

Byte 2

00000000

Byte 3

00000000
```

_(On most modern systems, which are little-endian.)_

---

# The Stack

All local variables live inside the **stack**.

Example

```cpp
int main()
{
    int a = 10;
    int b = 25;
}
```

Memory

```
Stack

+----------------+
| b = 25         |
+----------------+
| a = 10         |
+----------------+
```

The stack automatically grows and shrinks as functions are called and return.

You don't manage it manually.

---

# Printing Addresses

We can ask

> "Where is this variable stored?"

using

```cpp
&
```

called the **address-of operator**.

Example

```cpp
#include <iostream>

int main()
{
    int a = 10;

    std::cout << &a;
}
```

Possible output

```
0x61FF08
```

That means

```
Variable

↓

a

↓

Address

0x61FF08
```

---

# The Address-of Operator (`&`)

This operator asks

> "Give me the memory address of this object."

Example

```cpp
int a = 10;

&a
```

Result

```
0x61FF08
```

Think

```
Object

↓

Address-of

↓

Memory Location
```

---

# Printing Multiple Addresses

```cpp
int a = 10;
int b = 25;

std::cout << &a << '\n';
std::cout << &b << '\n';
```

Example output

```
0x61FF08

0x61FF0C
```

Notice

```
Difference

=

4 bytes
```

because each integer occupies 4 bytes.

---

# Why are addresses printed in hexadecimal?

Addresses are usually shown in **hexadecimal (base 16)** because it's a compact way to represent binary memory addresses.

Example:

```
Binary
0000 0000 0000 1010

Hexadecimal
0x000A
```

Each hexadecimal digit represents **4 bits**, making addresses much shorter and easier to read than long binary strings.

The `0x` prefix simply means "this number is in hexadecimal."

---

# The `sizeof` Operator

Sometimes we want to know

> "How many bytes does this object occupy?"

Use

```cpp
sizeof
```

Example

```cpp
int a = 10;

std::cout << sizeof(a);
```

Output

```
4
```

Meaning

```
a

↓

occupies

4 bytes
```

---

# `sizeof` vs Value

Don't confuse

```cpp
sizeof(a)
```

with

```cpp
a
```

If

```cpp
int a = 999999;
```

Then

```
a

↓

999999
```

but

```
sizeof(a)

↓

4
```

The value changes.

The storage size usually doesn't.

---

# Printing Everything Together

```cpp
#include <iostream>

void printInfo(int& value)
{
    std::cout
        << "Address : " << &value << '\n'
        << "Value   : " << value << '\n'
        << "Size    : " << sizeof(value) << " bytes\n";
}

int main()
{
    int a = 10;
    int b = 25;

    printInfo(a);
    std::cout << '\n';
    printInfo(b);
}
```

Possible output

```
Address : 0x61FF08
Value   : 10
Size    : 4 bytes

Address : 0x61FF0C
Value   : 25
Size    : 4 bytes
```

---

# Dry Run

```cpp
int a = 10;
```

### Step 1

Compiler sees

```
Need an integer.
```

---

### Step 2

Reserve 4 bytes.

```
Address

0x61FF08
```

---

### Step 3

Store

```
10
```

inside those bytes.

---

### Step 4

Associate

```
a

↓

0x61FF08
```

---

### Step 5

When executing

```cpp
std::cout << a;
```

the compiler effectively translates that into

```
Read the integer stored at

0x61FF08

↓

Print it.
```

---

# Common Beginner Mistakes

### ❌ Thinking variables are values

Wrong:

```
a is 10
```

Better:

```
a is a name.

10 is the value stored in the memory owned by a.
```

---

### ❌ Thinking variables store addresses

They don't.

A normal variable stores **data**.

Only a **pointer** stores an address.

We'll study pointers in the next chapter.

---

### ❌ Believing addresses never change

They often **change between program runs** because of mechanisms like Address Space Layout Randomization (ASLR).

What matters is that **within a single run**, each live object has a valid address.

---

# Key Takeaways

- Every object occupies memory.
    
- Variables are **names**, not the memory itself.
    
- Every object has an address.
    
- `&` gives the address of an object.
    
- `sizeof` tells you how many bytes an object occupies.
    
- Local variables are typically stored on the **stack**.
    
- The CPU works with **addresses**, while variable names are a convenience provided by the compiler.
    

---

## Next Chapter

We'll build directly on this by answering a natural question:

> If every object has an address, **can we store an address inside another variable?**

That question leads us to one of the most important concepts in C++: **Pointers**.