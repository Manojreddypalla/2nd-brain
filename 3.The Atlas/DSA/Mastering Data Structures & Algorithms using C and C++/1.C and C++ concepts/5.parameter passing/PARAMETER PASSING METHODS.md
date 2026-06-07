# BIG IDEA

When you call a function:

```
fun(a);
```

Question:

> “What exactly is sent to the function?”

There are 3 major ways:

```
1. Pass by Value
2. Pass by Address
3. Pass by Reference
```

---

# 1. PASS BY VALUE

---

# CORE IDEA

Function gets a COPY.

```
Original variable remains safe.
```

---

# Example

```cpp
#include<iostream>
usingnamespacestd;

voidchange(intx)
{
x =100;
}

intmain()
{
inta =10;

change(a);

cout<<a;
}
```

Output:

```
10
```

---

# MEMORY VISUALIZATION

Before call:

```
main stack frame:

a = 10
```

After:

```
main stack frame:
a = 10

change stack frame:
x = 10
```

IMPORTANT:

```
x is NOT a
```

It is just a copy.

---

# WHY?

Because:

```
change(a);
```

means:

```
copy value of a into x
```

---

# REAL UNDERSTANDING

Pass by value creates:

```
NEW MEMORY
```

inside called function.

So modifying `x` changes only local copy.

---

# USE WHEN?

Use Pass by Value when:

✅ You don't want original modified

✅ Small data types

✅ Function only reads values

---

# SWAP FAILS WITH PASS BY VALUE

```
voidswap(intx,inty)
{
inttemp =x;
x =y;
y =temp;
}
```

Why fails?

Because only copies are swapped.

Original variables untouched.

---

# 2. PASS BY ADDRESS

---

# CORE IDEA

Instead of copying value:

```
Send MEMORY ADDRESS
```

Function can directly access original variable.

---

# Example

```
#include<iostream>
usingnamespacestd;

voidchange(int *x)
{
    *x =100;
}

intmain()
{
inta =10;

change(&a);

cout<<a;
}
```

Output:

```
100
```

---

# UNDERSTANDING STEP-BY-STEP

---

# Step 1

```
&a
```

means:

```
address of a
```

Suppose:

```
a stored at 200
```

Then:

```
&a = 200
```

---

# Step 2

```
change(&a);
```

passes:

```
200
```

---

# Step 3

Function receives:

```
int *x
```

So:

```
x = 200
```

Pointer now points to `a`.

---

# Step 4

```
*x =100;
```

means:

```
go to address 200
change value there
```

So original variable changes.

---

# MEMORY MODEL

```
a = 10 at address 200

x stores 200

*x accesses value at 200
```

---

# IMPORTANT CONNECTION

This is the foundation of:

- Linked Lists
- Trees
- Graphs
- Dynamic memory
- Heap allocation

Because all use addresses.

---

# USE WHEN?

✅ Need to modify original

✅ Large objects

✅ Dynamic memory

✅ Low-level control

---

# 3. PASS BY REFERENCE

(C++ feature)

---

# CORE IDEA

Reference = another name for same variable.

---

# Example

```
voidchange(int &x)
{
x =100;
}
```

---

# Call

```
change(a);
```

No `&a`.

No pointers.

Still modifies original.

---

# WHY?

Because:

```
x is alias of a
```

---

# MEMORY MODEL

```
a ----+
      |
x ----+
```

Both names point to SAME memory.

---

# IMPORTANT DIFFERENCE

## Pass by Address

You manually work with addresses.

```
*x
&a
```

---

## Pass by Reference

Compiler handles internally.

Cleaner syntax.

---

# WHY REFERENCE FEELS MAGIC

Because internally compiler behaves almost like pointers.

But syntax becomes simple.

---

# COMPARISON TABLE

|Method|Copy Created?|Can Modify Original?|Syntax|
|---|---|---|---|
|Value|Yes|No|normal|
|Address|No|Yes|pointer|
|Reference|No|Yes|`&`|

---

# MOST IMPORTANT INTERVIEW UNDERSTANDING

---

# VALUE

```
Function gets COPY
```

---

# ADDRESS

```
Function gets LOCATION
```

---

# REFERENCE

```
Function gets ALIAS
```

---

# MASTER MEMORY CONNECTION

```
Pass by Value
    ↓
Copies data

Pass by Address
    ↓
Uses pointers

Pass by Reference
    ↓
Uses aliases
```