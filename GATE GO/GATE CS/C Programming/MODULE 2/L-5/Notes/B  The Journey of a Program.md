Yep. **From page 1 through page 51**, all in one set of notes. Since we already covered **pages 1–17**, I’ll continue from **Page 18 → Page 51** so you can append this directly underneath the previous notes.

I’ll keep it **GATE-oriented and concept-heavy**, not beginner filler.

# C Programming — Functions & Compilation System

## Pages 18–51 Notes

---

# 1. GATE 2019 — Function Parameter Passing

Consider:

```c
int jumble(int x, int y)
{
    x = 2 * x + y;
    return x;
}

int main()
{
    int x = 2, y = 5;

    y = jumble(x, y);
    x = jumble(x, y);

    printf("%d %d", x, y);
}
```

The important concept is again:

> **C uses pass-by-value.**

Each call gets **copies** of the arguments.

### First call

```c
y = jumble(x, y);
```

Initially:

```text
main:
x = 2
y = 5
```

Inside `jumble`:

```text
x = 2
y = 5
```

Calculate:

```text
x = 2*x + y
  = 2(2) + 5
  = 9
```

Return:

```text
9
```

So:

```text
main:
x = 2
y = 9
```

### Second call

```c
x = jumble(x, y);
```

Arguments are now:

```text
x = 2
y = 9
```

Inside `jumble`:

```text
x = 2
y = 9
```

Therefore:

```text
x = 2(2) + 9
  = 13
```

Return `13`.

Final:

```text
x = 13
y = 9
```

So the program prints:

```text
13 9
```

The key GATE skill is **tracking caller variables separately from formal parameters**.

---

# 2. From Writing C to Running It

Now the lecture shifts from **C language** to **how a C program actually becomes an executing process**.

The four major stages are remembered as:

# CALL

```text
C → Compiler → Assembler → Linker → Loader
```

**CALL =**

```text
C   → Compiler
A   → Assembler
L   → Linker
L   → Loader
```

The lecture introduces this as:

> **From Writing to Running — CALL: Compiler, Assembler, Linker, Loader**.

---

# 3. Complete Compilation System

The basic pipeline is:

```text
hello.c
   ↓
Preprocessor
   ↓
hello.i
   ↓
Compiler
   ↓
hello.s
   ↓
Assembler
   ↓
hello.o
   ↓
Linker + required object/library files
   ↓
hello
   ↓
Loader
   ↓
Program executing in memory
```

More specifically:

```text
hello.c
  │
  ▼
Preprocessor (cpp)
  │
  ▼
hello.i
  │
  ▼
Compiler (cc1)
  │
  ▼
hello.s
  │
  ▼
Assembler (as)
  │
  ▼
hello.o
  │
  │ + printf.o / libraries
  ▼
Linker (ld)
  │
  ▼
hello
```

The lecture's compilation-system diagram uses exactly this sequence.

---

# 4. Source Program — `.c`

Example:

```c
#include <stdio.h>

int main()
{
    printf("Hello");
}
```

Stored as:

```text
hello.c
```

This is the **source program** written by the programmer.

---

# 5. Preprocessor

The first stage is the **preprocessor**.

```text
hello.c
   ↓
preprocessor
   ↓
hello.i
```

Its job includes processing things such as:

```c
#include
#define
```

For example:

```c
#include <stdio.h>
```

is handled during preprocessing.

So:

```text
hello.c
   ↓
Preprocessor
   ↓
modified source
hello.i
```

---

# 6. Compiler

Next:

```text
hello.i
   ↓
Compiler
   ↓
hello.s
```

The compiler converts the modified C source into **assembly language**.

```text
C source
   ↓
Compiler
   ↓
Assembly
```

Example conceptual output:

```asm
mov ...
```

The lecture notes that the **compiler is the most computationally complex stage among CALL**, not the assembler.

---

# 7. Assembly File — `.s`

The compiler generates:

```text
hello.s
```

This is an **assembly-language program**.

Pipeline:

```text
hello.c
   ↓
hello.i
   ↓
hello.s
```

At this point we are no longer dealing directly with C syntax.

---

# 8. Assembler

Next:

```text
hello.s
   ↓
Assembler
   ↓
hello.o
```

The assembler converts assembly instructions into an **object file**.

```text
Assembly
   ↓
Assembler
   ↓
Object code
```

The output is:

```text
hello.o
```

### Critical GATE point

> **Assembler does NOT produce the executable.**

It produces an **object file**.

---

# 9. Object File

An object file is a **relocatable object program**.

Example:

```text
hello.o
```

It contains compiled machine-level information, but the final executable may still require:

- other object files
    
- library code
    
- resolution of references
    

Therefore:

```text
object file ≠ final executable
```

---

# 10. Linker

The linker combines object files and required library components.

```text
hello.o
printf.o
   │
   ▼
 Linker
   │
   ▼
hello
```

The linker creates the **executable object program**.

---

# 11. Why Do We Need the Linker?

Suppose your code contains:

```c
printf("Hello");
```

You didn't write the implementation of `printf()` inside your program.

Its implementation is supplied through library code.

So conceptually:

```text
Your code
   │
   ├── calls printf()
   │
   ▼
Object file
   │
   │ needs printf implementation
   ▼
Linker
   │
   ├── your object code
   └── library/object code
   │
   ▼
Executable
```

This is why linking is required.

---

# 12. Logical Address

Now the lecture introduces an important memory concept.

Suppose the program contains:

```asm
MOV R1, 1000
```

The address `1000` here is treated as a **logical address**.

The important idea:

> At compilation time, the compiler doesn't know the exact physical memory location where the program will execute years later.

---

# 13. Why Can't the Compiler Know the Physical Address?

Imagine:

```text
Compile program
      ↓
5 years pass
      ↓
Run program
```

The physical memory arrangement at execution time could be completely different.

For example:

```text
RAM

┌──────────────┐
│ OS           │
├──────────────┤
│ Process A    │
├──────────────┤
│ Free         │
├──────────────┤
│ Process B    │
├──────────────┤
│ Free         │
└──────────────┘
```

The program cannot depend on one fixed physical location.

Therefore the compiler works with **logical addresses** rather than assuming a permanent physical RAM address.

---

# 14. Relocatable Code

The lecture describes the generated logical addresses as **relocatable**.

Mental model:

```text
Compile:
"I need this logical address."

Run:
"Where does this actually reside in memory?"
```

So the program can be loaded into different physical locations without rewriting the source program itself.

---

# 15. Logical Memory

Think of the program as having its own logical view of memory:

```text
Logical memory

┌──────────┐
│ ...      │
├──────────┤
│ b        │
├──────────┤
│ a        │
├──────────┤
│ 5        │
└──────────┘
```

The program works using these logical addresses.

The actual physical RAM arrangement is handled separately.

---

# 16. Program → Memory → Process

The lecture gives this progression:

```text
sum.c
  ↓
sum.s
  ↓
sum.o
  ↓
executable program
  ↓
loader
  ↓
executing in memory
  ↓
process
```

So:

```text
Executable on disk
       ↓
     Loader
       ↓
Program in memory
       ↓
Executing
       ↓
Process
```

### Important distinction

```text
Program
= executable instructions stored on disk

Process
= executing instance of that program
```

---

# 17. Loader

The **loader** acts at the time the program is going to run.

```text
Executable
    ↓
 Loader
    ↓
Memory
    ↓
Ready to run
```

The lecture describes the loader as placing the executable into memory **in preparation for execution**.

---

# 18. Complete Mental Model

Keep this entire chain in your head:

```text
                 WRITING
                    │
                  hello.c
                    │
                    ▼
              PREPROCESSOR
                    │
                  hello.i
                    │
                    ▼
                COMPILER
                    │
                  hello.s
                    │
                    ▼
                ASSEMBLER
                    │
                  hello.o
                    │
                    ▼
                 LINKER
                    │
              executable
                    │
                    ▼
                 LOADER
                    │
                    ▼
            program in memory
                    │
                    ▼
                 PROCESS
```

This is the **main conceptual backbone** of pages 20–40.

---

# 19. CALL — What Each Stage Does

|Stage|Input|Output|Main responsibility|
|---|---|---|---|
|Preprocessor|`.c`|`.i`|Preprocess source|
|**Compiler**|`.i`|`.s`|C → Assembly|
|**Assembler**|`.s`|`.o`|Assembly → Object|
|**Linker**|Object files + libraries|Executable|Combine/link|
|**Loader**|Executable|Program in memory|Prepare for execution|

### Fast memory

```text
Compiler  → C → Assembly
Assembler → Assembly → Object
Linker    → Object → Executable
Loader    → Executable → Memory
```

---

# 20. Compiler vs Assembler

A GATE-style statement:

> **Assembler is the step with the highest computational complexity among CALL.**

❌ **False**

The lecture explicitly states:

> Compiler is more complex than assembler.

So:

```text
Compiler
   >
Assembler
```

in computational complexity, according to the lecture's framing.

---

# 21. Assembler Does Not Create Executable

Statement:

> The assembler produces an executable.

❌ **False**

Correct:

```text
Assembler
   ↓
Object file

Linker
   ↓
Executable
```

The lecture explicitly gives this reasoning.

---

# 22. Loader Places Program in Memory

Statement:

> In the loader, the program is placed in memory in preparation for running the code.

✅ **True**

Pipeline:

```text
Executable stored on disk
          ↓
        Loader
          ↓
Program placed in memory
          ↓
       Execution
```

---

# 23. Which CALL Stage Does What?

The lecture asks:

### Static data is placed in memory

Answer:

```text
→ Loader
```

### External labels are resolved

Answer:

```text
→ Linker
```

### Operator precedence is resolved

Answer:

```text
→ Compiler
```

---

# 24. Why Operator Precedence = Compiler?

Consider:

```c
x = a + b * c;
```

The compiler must understand that:

```text
*
```

has higher precedence than:

```text
+
```

So conceptually:

```text
a + b * c
      ↓
a + (b*c)
```

This is a **language-level interpretation** task, so it belongs to the compiler stage.

```text
Operator precedence
       ↓
Compiler
```

---

# 25. Why External Labels = Linker?

Suppose:

```text
file A
   ↓
calls function X

file B
   ↓
defines function X
```

The two pieces may be compiled separately.

The linker connects them:

```text
A.o ─────┐
         ├──→ Linker → executable
B.o ─────┘
```

So:

```text
External references / labels
            ↓
          Linker
```

---

# 26. GATE/Exam Question — Binary From Multiple Object Files

Question:

> Which creates a binary from multiple object files?

Options:

```text
Compiler
Assembler
Linker
Interpreter
```

Answer:

# **Linker**

Because:

```text
object1.o
object2.o
object3.o
    │
    ▼
  LINKER
    │
    ▼
 executable binary
```

---

# 27. Linking Multiple C Files

Now the lecture shows a practical example with:

```text
a.c
b.c
```

### `a.c`

```c
#include <stdio.h>

int sum(int x, int y);

int main()
{
    int a;

    a = sum(2,3);

    printf("%d", a);
}
```

### `b.c`

```c
int sum(int x, int y)
{
    return x+y;
}
```

The important thing:

> These are **separate source files**.

---

# 28. Separate Compilation

Each source file can be compiled separately.

```text
a.c
 ↓
compiler
 ↓
a.i
 ↓
a.s
 ↓
a.o
```

And:

```text
b.c
 ↓
compiler
 ↓
b.i
 ↓
b.s
 ↓
b.o
```

So:

```text
a.c → a.o
b.c → b.o
```

The lecture visualizes the two independent compilation paths.

---

# 29. Why Separate Compilation?

This is a major practical idea.

You don't need to compile the entire program from scratch every time.

Suppose:

```text
a.c → a.o
b.c → b.o
```

If you modify only `b.c`:

```text
a.o  ← already available
b.c → recompile → new b.o
```

Then:

```text
a.o + b.o
     ↓
   linker
```

This is the basic motivation behind **separate compilation**.

---

# 30. Link Time

After separate compilation:

```text
a.o
b.o
```

are passed to the linker.

```text
a.o ──────┐
          │
          ▼
        LINKER
          │
          ▼
      executable
          │
          ▼
        loader
```

The lecture explicitly shows the two source files being compiled separately and then brought together during linking.

---

# 31. What Happens to `sum()`?

In `a.c`:

```c
int sum(int x, int y);
```

This tells the compiler:

```text
sum exists
sum takes two int arguments
sum returns int
```

Then:

```c
a = sum(2,3);
```

creates a reference to `sum`.

But the actual definition is in:

```text
b.c
```

```c
int sum(int x, int y)
{
    return x+y;
}
```

After separate compilation:

```text
a.o → reference to sum
b.o → definition of sum
```

The linker connects them.

---

# 32. The Linker Is the Connector

Think of it like this:

```text
a.o
┌───────────────────┐
│ main()            │
│                   │
│ calls → sum() ────┼─────┐
└───────────────────┘     │
                          │
b.o                       │
┌───────────────────┐     │
│ sum() definition  │◄────┘
└───────────────────┘

           ↓
        LINKER

           ↓

      executable
```

That's the intuition behind **external symbol/reference resolution**.

---

# 33. `printf()` and Libraries

The lecture then uses:

```c
printf("hello\n");
```

as another example.

Your source code doesn't define `printf()` itself.

Instead, the declaration is made available through:

```c
#include <stdio.h>
```

The lecture highlights that `stdio.h` provides the **prototype of `printf`**.

Conceptually:

```text
#include <stdio.h>
        ↓
prototype of printf()
        ↓
compiler knows printf's interface
```

---

# 34. Explicit Declaration of `printf`

The lecture shows:

```c
int printf(char *s);
```

as an explicit declaration/prototype of `printf`.

Then:

```c
int main()
{
    printf("hello\n");
}
```

The important point is that the compiler needs a declaration/prototype for the function being called.

In normal C programming, this is supplied through:

```c
#include <stdio.h>
```

The lecture notes that `stdio.h` provides the prototype.

---

# 35. Optional — Linking Two Files

The lecture labels the next section:

> **Optional — Linking 2 files**.

The important workflow is:

```text
             Separate compilation

a.c ──→ a.o
             \
              \
               → LINKER → executable
              /
             /
b.c ──→ b.o
```

The linker combines the separately generated object files and resolves the relationships between them.

---

# 36. Full Picture — Pages 18–51

Now compress the entire section into one mental model:

```text
                 C PROGRAM
                     │
                     ▼
              Function Calls
                     │
             Pass-by-value
                     │
             Separate local
             execution state
                     │
                     ▼
                source.c
                     │
                     ▼
               PREPROCESSOR
                     │
                   .i
                     │
                     ▼
                 COMPILER
                     │
                   .s
                     │
                     ▼
                ASSEMBLER
                     │
                   .o
                     │
              ┌──────┴──────┐
              │             │
            a.o            b.o
              │             │
              └──────┬──────┘
                     ▼
                  LINKER
                     │
               executable
                     │
                     ▼
                  LOADER
                     │
                     ▼
             program in memory
                     │
                     ▼
                  PROCESS
```

---

# 37. GATE Quick Table

|Question|Answer|
|---|---|
|C → Assembly|**Compiler**|
|Assembly → Object|**Assembler**|
|Multiple objects → Executable|**Linker**|
|External labels/references resolved|**Linker**|
|Executable → Memory|**Loader**|
|Static data placed in memory|**Loader**|
|Operator precedence resolved|**Compiler**|
|Highest complexity among CALL|**Compiler**|
|Does assembler create executable?|**No**|
|Does linker create executable?|**Yes**|
|Separate `.c` files can be compiled independently?|**Yes**|
|`printf` prototype supplied by?|`stdio.h`|

The stage-specific answers are directly reflected in the lecture's question slides.

---

# 38. Must-Know Exam Traps

### ❌ Trap 1

```text
Assembler → executable
```

Wrong.

```text
Assembler → .o
Linker → executable
```

---

### ❌ Trap 2

```text
Linker → loads executable into RAM
```

Wrong.

```text
Linker → executable
Loader → memory
```

---

### ❌ Trap 3

```text
External labels → Compiler
```

Wrong in this lecture's model.

```text
External labels → Linker
```

---

### ❌ Trap 4

```text
Operator precedence → Linker
```

Wrong.

```text
Operator precedence → Compiler
```

---

### ❌ Trap 5

```text
Assembler has highest computational complexity.
```

Wrong.

```text
Compiler > Assembler
```

according to the lecture.

---

# 39. Final Revision Notes

```text
## CALL

C → Compiler → Assembler → Linker → Loader

### Compiler
Input  → modified C source (.i)
Output → assembly (.s)

Main idea:
C semantics → assembly

Operator precedence → Compiler

Compiler is more computationally complex than assembler.


### Assembler
Input  → assembly (.s)
Output → object file (.o)

Assembly → object

NOT executable.


### Linker
Input:
multiple object files + required library/object code

Output:
executable

Responsibilities:
- combine object files
- resolve external references/labels
- produce executable


### Loader
Input:
executable

Output:
program in memory

Responsibilities:
- place executable into memory
- prepare it for execution

Static data → Loader


## Separate Compilation

a.c → a.o
b.c → b.o

then:

a.o + b.o
     ↓
   Linker
     ↓
 executable


## Logical Address

Compiler works with logical/relocatable addresses.

Physical memory location is not fixed at compile time.

Program can later be loaded into memory.


## Program vs Process

Program:
executable code stored on disk.

Process:
executing program in memory.


## Functions

C uses pass-by-value.

Actual arguments
      ↓ copy
Formal parameters

Each function invocation has its own activation record.

Changing formal parameters does not directly change caller variables.


## printf

stdio.h provides the prototype/declaration needed for printf.
```

### 🔥 The one diagram I'd memorize

```text
SOURCE
  │
  ▼
Preprocessor
  │ .i
  ▼
Compiler ─────────→ Operator precedence
  │ .s
  ▼
Assembler
  │ .o
  ▼
Linker ───────────→ External references
  │                 Multiple object files
  │ executable
  ▼
Loader ───────────→ Memory
                    Static data
  │
  ▼
PROCESS
```

This covers **pages 18–51**, continuing from the notes for pages 1–17, and I have **not gone beyond page 51**.