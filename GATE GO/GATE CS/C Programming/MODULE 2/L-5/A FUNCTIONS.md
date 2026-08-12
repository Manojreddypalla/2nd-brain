# C Programming — Functions

### Notes: Pages 1–17 only

---

## 1. Why Functions?

Real-world programs become large and complex.

Instead of keeping everything in one huge block, divide the program into **smaller logical units called functions**.

```text
Large Program
     ↓
 ┌───────┐
 │ main  │
 └───────┘
   ↓   ↓
 func1 func2
   ↓
 func3
```

### Mathematical intuition

A function behaves like:

```text
y = f(x)
```

- `x` → input
    
- `f` → operation
    
- `y` → output
    

This gives **modularity** and makes programs easier to manage/read.

---

# 2. Function Syntax

General form:

```c
return_type function_name(parameter_list)
{
    // statements

    return statement;
}
```

Example:

```c
int square(int x)
{
    return x * x;
}
```

### Components

```text
int       → return type
square    → function name
int       → parameter type
x         → parameter name

{ ... }   → function body
```

The curly braces group the statements belonging to the function.

---

# 3. Function Definition

General:

```c
type fname(type arg1, type arg2)
{
    // local variables
    // executable code
}
```

Example:

```c
float mul(float x, float y)
{
    float result;

    result = x * y;

    return result;
}
```

Here:

- `float` → return type
    
- `mul` → function name
    
- `x, y` → parameters
    
- `result` → local variable
    
- `return result` → sends result back to caller
    

---

# 4. Function Call

A function is executed through a **function call**.

```c
fname(arg1, arg2);
```

Example:

```c
int y;

y = mul(10, 5);
```

Here:

```text
mul(10, 5)
    ↓
function call
    ↓
mul executes
    ↓
returns result
    ↓
stored in y
```

---

# 5. Function Declaration / Prototype

A **function declaration is also called a function prototype**.

Example:

```c
int mul(int m, int n);
```

The prototype tells the compiler the function's:

```text
return type
parameter types
function name
```

It does **not** contain the function body.

```c
int mul(int, int);
```

is therefore a prototype.

---

# 6. Prototype — Parameter Names Are Optional

These declarations are equivalent:

```c
int mul(int a, int b);
```

and

```c
int mul(int, int);
```

The parameter names don't matter in the declaration.

Only the **types** are required.

### Therefore

```c
int mul(int a, int b);
```

and

```c
int mul(int x, int y);
```

represent the same function prototype.

The lecture explicitly marks these forms as equally acceptable.

### GATE point

Don't confuse:

```c
int mul(int, int);       // declaration
```

with:

```c
int mul(int a, int b)    // definition
{
    return a + b;
}
```

For the **definition**, parameter names are needed because the function body refers to them.

---

# 7. Function Declaration + Definition

Example:

```c
#include <stdio.h>

int fun(int x, int y);       // declaration

int main()
{
    int a;

    a = fun(2, 3);

    printf("%d", a);
}

int fun(int x, int y)        // definition
{
    return x + y;
}
```

Execution relationship:

```text
main()
  │
  │ fun(2,3)
  ▼
fun()
  │
  │ return x+y
  ▼
main()
```

The declaration allows the function to be used before its definition appears in the source file.

---

# 8. `void` Functions

A function may have **no return value**.

```c
void display(void)
{
    printf("No type, no parameters");
}
```

Here:

```text
void before name
     ↓
no return value

void inside ()
     ↓
no parameters
```

So:

```c
void display(void)
```

means:

> Function accepts no parameters and returns nothing.

---

# 9. `return` in `void` Function

A `void` function doesn't need a return statement:

```c
void display(void)
{
    printf("Hello");
}
```

But it can use:

```c
return;
```

to terminate the function.

Example from the lecture:

```c
void sum(int a, int b)
{
    printf("sum = %d", a + b);

    return;
}
```

Here `return;` is optional because the function is `void`.

---

# 10. Function Activation

When a function is called, its execution requires its own **activation record (AR)**.

Example:

```c
main()
{
    int y;

    y = mul(10, 5);
}
```

and:

```c
int mul(int x, int y)
{
    int p;

    p = x * y;

    return p;
}
```

Conceptually:

```text
Before call:

Stack
┌──────────────┐
│ AR of main   │
│ y            │
└──────────────┘
```

After `mul()` is called:

```text
Stack
┌──────────────┐
│ AR of mul    │
│ x = 10       │
│ y = 5        │
│ p            │
├──────────────┤
│ AR of main   │
│ y            │
└──────────────┘
```

So each function invocation gets its own execution context.

The lecture begins introducing this activation-record idea in pages 15–17.

---

# 11. GATE 2005 — Function Declaration

The lecture gives a GATE 2005 question:

```c
double foo(double);       /* Line 1 */

int main()
{
    double da, db;

    db = foo(da);
}

double foo(double a)
{
    return a;
}
```

The important point is that the declaration:

```c
double foo(double);
```

appears before the call.

Therefore the compiler knows:

```text
foo
↓
takes a double
↓
returns a double
```

The lecture's answer indicates that **deleting Line 1 causes compiler errors**.

### GATE takeaway

If a function is called before the compiler has seen an appropriate declaration/definition, this can cause compilation problems.

---

# 12. Call by Value

This is the **most important concept in pages 15–17**.

Consider:

```c
void swap(int x, int y)
{
    int temp = x;

    x = y;
    y = temp;
}

int main()
{
    int a = 3;
    int b = 4;

    swap(a, b);

    printf("a = %d, b = %d", a, b);
}
```

At first glance you might think:

```text
a = 4
b = 3
```

But **NO**.

The original values of `a` and `b` are not swapped.

---

# 13. Why?

C passes function arguments **by value**.

When:

```c
swap(a, b);
```

is executed:

```text
a = 3 ─────copy────→ x = 3
b = 4 ─────copy────→ y = 4
```

There are now **two separate sets of variables**.

```text
main activation record
┌─────────────┐
│ a = 3       │
│ b = 4       │
└─────────────┘

swap activation record
┌─────────────┐
│ x = 3       │
│ y = 4       │
│ temp        │
└─────────────┘
```

Changing:

```c
x
y
```

doesn't change:

```c
a
b
```

The lecture explicitly illustrates the separate `main` and `swap` activation records.

---

# 14. Dry Run — `swap(a,b)`

Initial state:

```text
main:

a = 3
b = 4
```

Call:

```c
swap(a,b);
```

### Step 1 — Parameter copying

```text
x = 3
y = 4
```

So:

```text
a = 3    b = 4
x = 3    y = 4
```

### Step 2

```c
temp = x;
```

```text
temp = 3
```

### Step 3

```c
x = y;
```

```text
x = 4
```

### Step 4

```c
y = temp;
```

```text
y = 3
```

Now:

```text
swap:
x = 4
y = 3
```

But:

```text
main:
a = 3
b = 4
```

### Therefore

```text
❌ a and b are NOT swapped.

Final:
a = 3
b = 4
```

---

# 15. Activation Record — Important Mental Model

Think of a function call as creating a **new box of execution state**.

```text
                 STACK
                  ↓

┌─────────────────────────┐
│ swap() activation       │
│                         │
│ x = 3                   │
│ y = 4                   │
│ temp = 3                │
└─────────────────────────┘
│ main() activation       │
│                         │
│ a = 3                   │
│ b = 4                   │
└─────────────────────────┘
```

So:

```text
main's a ≠ swap's x
main's b ≠ swap's y
```

even though their **values initially match**.

This is the key reason the swap fails.

---

# 16. What You Should Remember for GATE

### Functions

```text
Function = modular unit of computation
```

### Definition

```c
int f(int x, int y)
{
    return x+y;
}
```

### Call

```c
f(2,3);
```

### Prototype

```c
int f(int, int);
```

### Prototype parameter names

```c
int f(int a, int b);
int f(int, int);
```

Both are valid/equivalent declarations.

### `void`

```c
void f(void);
```

→ no arguments + no return value.

### Function call

Creates a separate **activation record/execution context**.

### C parameter passing

> **C uses pass-by-value.**

```text
actual argument
      ↓ copy
formal parameter
```

Therefore:

```c
swap(a,b);
```

does **not** swap `a` and `b` when ordinary integer parameters are used.

---

## 🧠 The single mental picture for pages 1–17

```text
             FUNCTION
                 │
       ┌─────────┴─────────┐
       │                   │
   declaration          definition
   / prototype              │
       │                   │
       └─────────┬─────────┘
                 │
             function call
                 │
                 ▼
        NEW ACTIVATION RECORD
                 │
          arguments copied
                 │
          pass-by-value
                 │
                 ▼
        separate local state
```

**Stop here.** Pages **18 onward are not included** in these notes.