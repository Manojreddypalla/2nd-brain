# C Programming — L-14 Full Short Revision Notes

> **Scope:** Complete 30-page lecture, compressed for **GATE revision + quick recall**. I’ve kept the lecture’s examples and important traps, while removing repetition. Page 18 is blank.

---

# 1. `getchar()` — Reading a Character

### Syntax

```c
int c;
c = getchar();
```

`getchar()` reads **one character** from standard input.

Lecture equivalence:

```c
c = getchar();
```

is equivalent to:

```c
scanf("%c", &c);
```

### Why use `getchar()`?

If only **one character** needs to be read, `scanf()` is unnecessary overhead.

### Important

`getchar()` returns an **`int`**, not `char`.

Why?

Because it must be able to return:

- a valid character
    
- `EOF`
    

So:

```c
int c;
c = getchar();
```

is preferred when checking EOF.

---

# 2. Reading Characters Until a Specific Character

Example idea:

```c
c = getchar();

while(c != 'z') {
    n1++;
    c = getchar();
}
```

For input:

```text
abcdez
```

Characters are read one at a time.

### Mental model

```text
input stream
   ↓
getchar()
   ↓
one character
   ↓
process it
   ↓
getchar()
   ↓
next character
```

---

# 3. `EOF`

`EOF` = **End Of File**

It is a special value used to indicate that no more input is available.

Typical loop:

```c
int c;

c = getchar();

while(c != EOF) {
    // process c
    c = getchar();
}
```

### GATE trap

Do **not** think of EOF as an ordinary character stored in the input.

`getchar()` returns an `int` specifically so it can represent EOF distinctly.

---

# 4. `'\n'` vs `EOF`

These are completely different.

```c
'\n'
```

= newline character.

```c
EOF
```

= special return value indicating end of input.

### Typical input

```text
abc<ENTER>
```

The input contains:

```text
a b c \n
```

The newline is an actual character in the input stream.

---

# 5. `putchar()`

`putchar()` writes **one character** to the screen.

```c
putchar(c);
```

is equivalent to:

```c
printf("%c", c);
```

Example:

```c
char ch = 'a';

putchar(ch);
```

Output:

```text
a
```

### Relationship

|Input|Output|
|---|---|
|`getchar()`|`putchar()`|
|reads one character|writes one character|

---

# 6. Recursive Character Processing

The lecture demonstrates using `getchar()` with recursion.

General pattern:

```c
void fun(void)
{
    int c;

    if((c = getchar()) != '\n')
        fun();

    putchar(c);
}
```

### Core idea

The function keeps reading characters until newline.

For:

```text
abc<ENTER>
```

calls become:

```text
fun()
  → read a
     → fun()
        → read b
           → fun()
              → read c
                 → fun()
                    → read \n
```

Then recursion returns backward.

This makes characters appear in **reverse order**.

```text
Input:  abc
Output: cba
```

### Why?

The `putchar()` happens **after** the recursive call.

So:

```text
read → recurse → return → print
```

This is the same fundamental pattern as recursive reversal problems.

---

# 7. GATE 2008 — Recursive `getchar()` Pattern

The lecture includes a GATE CSE 2008 question involving:

```c
void reverse(void)
{
    int c;

    if( condition )
        reverse();

    statement;
}
```

The key pattern is:

```text
read character
     ↓
if not newline
     ↓
recursive call
     ↓
print during return
```

### Question-solving trigger

Whenever you see:

```c
fun();
putchar(c);
```

inside recursion:

**Think → output happens while recursion unwinds.**

So the output order may be the **reverse of input order**.

---

# 8. Preprocessor

Lines beginning with:

```c
#
```

are processed by the **preprocessor**.

Examples:

```c
#include <stdio.h>
#define MAX 100
```

The preprocessor operates **before compilation**.

---

# 9. Macros

A macro is created using:

```c
#define
```

Example:

```c
#define MAXLINE 120
```

Then:

```c
char buf[MAXLINE + 1];
```

becomes approximately:

```c
char buf[120 + 1];
```

The substitution happens before the compiler processes the code.

### Mental model

```text
Source code
    ↓
Preprocessor
    ↓
Macro substitution
    ↓
Compiler
```

---

# 10. Object-Like Macro

```c
#define MAX 100
```

Usage:

```c
printf("%d", MAX);
```

After preprocessing:

```c
printf("%d", 100);
```

So:

```text
#define MAX 100
       ↓
     MAX → 100
```

---

# 11. Function-Like Macro

Example:

```c
#define PLUSONE(x) x+1
```

Then:

```c
PLUSONE(2)
```

becomes:

```c
2+1
```

### Important

A macro is **text substitution**, not a real function call.

There is no function-call stack frame created for a macro.

---

# 12. Macro Parentheses — VERY IMPORTANT

Bad macro:

```c
#define MULTIPLY(a,b) a*b
```

Now:

```c
MULTIPLY(2+3, 3+5)
```

expands to:

```c
2+3*3+5
```

Operator precedence gives:

```text
2 + 9 + 5
= 16
```

❌ Not `40`.

---

## Correct Macro

```c
#define MULTIPLY(a,b) ((a)*(b))
```

Now:

```c
MULTIPLY(2+3,3+5)
```

becomes:

```c
((2+3)*(3+5))
```

Therefore:

```text
5 × 8
= 40
```

### GATE Rule

For parameterized macros:

```c
#define MACRO(a,b) ((expression involving a and b))
```

**Parenthesize the parameters and the complete expression.**

---

# 13. Macro Expansion — Don't Think Like a Function

Consider:

```c
#define PLUSONE(x) x+1

int i = 3 * PLUSONE(2);
```

Expansion:

```c
int i = 3 * 2 + 1;
```

Therefore:

```text
6 + 1 = 7
```

NOT:

```text
3 * (2 + 1) = 9
```

### Pattern

Always perform **literal textual substitution first**.

Then apply C operator precedence.

---

# 14. Macro vs Function

|Macro|Function|
|---|---|
|Preprocessor substitution|Compiler-level function|
|Text replacement|Actual execution|
|No function-call overhead|Function-call mechanism|
|No type checking of parameters|Type checking applies|
|Can cause precedence problems|Expressions evaluated normally|

For GATE, the most important point is:

> **Macro = textual substitution before compilation.**

---

# 15. Call by Value vs Call by Reference

The lecture explicitly marks **call by reference as NOT required for GATE** and notes that only C programming is in the relevant syllabus context.

Still understand the concept because it connects directly to pointers.

---

# 16. Call by Value

C fundamentally uses **call by value**.

Example:

```c
void fun(int y)
{
    y = 15;
}
```

If:

```c
int y = 3;
fun(y);
```

then:

```text
main's y = 3
       ↓
copy
       ↓
fun's y = 3
       ↓
fun changes its y → 15
```

After returning:

```text
main's y = 3
```

The original variable is unchanged.

---

# 17. Call by Reference Concept

In a reference-style model, the function works with the original variable.

Conceptually:

```text
main variable
     ↑
     │
function accesses same object
```

But **C does not have true C++-style reference parameters**.

In C, pointer parameters are used to achieve the same practical effect.

---

# 18. Pointer-Based Modification

Example:

```c
void addOne(int *ptr)
{
    *ptr = *ptr + 1;
}
```

Caller:

```c
int i = 10;

addOne(&i);
```

Memory idea:

```text
i
┌────┐
│ 10 │
└────┘
 ↑
 │
ptr
```

Inside function:

```c
*ptr
```

accesses the original `i`.

Therefore:

```text
10 → 11
```

---

# 19. Important GATE Distinction

### C parameter passing

C uses:

> **Call by value**

Even when pointers are passed.

Example:

```c
void fun(int *p)
```

`p` itself is passed **by value**.

The value copied into `p` happens to be an **address**.

So:

```text
pointer passed by value
        ↓
copy of address
        ↓
both pointers point to same object
        ↓
*object can be modified
```

This distinction is extremely important.

---

# 20. Static Scoping

C uses **static (lexical) scoping**.

Also called:

> **Lexical scoping**

The meaning of a variable is determined from the **program's structure/source code**, not from the runtime call chain.

---

# 21. Static Scope Example

```c
int b = 5;

int foo()
{
    int a = b + 5;
    return a;
}

int bar()
{
    int b = 2;
    return foo();
}
```

When `foo()` executes:

```c
b
```

refers to the `b` visible according to the **definition of `foo()`**.

That is:

```c
global b = 5
```

So:

```text
foo() → 5 + 5 → 10
```

Even though `foo()` was called from `bar()`, where another `b = 2` exists.

---

# 22. Dynamic Scoping

Under dynamic scoping, variable lookup depends on the **runtime calling chain**.

For:

```text
main()
  ↓
bar()
  ↓
foo()
```

`foo()` would search callers for a matching variable.

So `foo()` could potentially see `bar()`'s local `b`.

### But:

> **C does NOT use dynamic scoping.**

C uses **static scoping**.

---

# 23. Static vs Dynamic Scoping

|Static Scoping|Dynamic Scoping|
|---|---|
|Based on source/program structure|Based on call sequence|
|Determined largely at compile time|Determined during execution|
|C uses it|C does not use it|
|Caller doesn't change variable binding|Caller can affect lookup|

### Memory trick

**Static → Source structure**

**Dynamic → Dynamic call chain**

---

# 24. Block Scope

A variable declared inside a block:

```c
{
    int i = 10;
}
```

has scope limited to that block.

Example:

```c
int main()
{
    int i = 10;

    {
        int i = 15;
    }
}
```

The inner `i` shadows the outer `i`.

### Shadowing

```text
outer i = 10
      ↓
inner block
      ↓
inner i = 15
```

Inside inner block → `15`.

Outside → outer `10`.

---

# 25. Static Scoping + Functions

Very important distinction:

```c
int x = 2;

void c()
{
    printf("%d", x);
}

void b()
{
    int x = 1;
    c();
}
```

When `c()` executes, it does **not** use `b()`'s local `x`.

Why?

Because `c()` is defined in the global environment and C uses **static scope**.

Therefore:

```text
c() → global x
```

not:

```text
c() → caller's x
```

---

# 26. Macro + Scope

Lecture example:

```c
#define a (x+1)

int x = 2;

void c()
{
    printf("%d", a);
}
```

Preprocessor first expands:

```c
printf("%d", (x+1));
```

Then normal C scope rules apply.

So `x` is resolved according to **static scope**.

---

# 27. Static vs Dynamic Example

Consider:

```c
#define a (x+1)

int x = 2;

void c()
{
    printf("%d", a);
}

void b()
{
    int x = 1;
    c();
    printf("%d", a);
}

int main()
{
    b();
    c();
}
```

### Under C's static scope

Inside `c()`:

```text
x = global x = 2
a = 2 + 1 = 3
```

Inside `b()`:

```text
x = local 1
a = 1 + 1 = 2
```

So the output conceptually is:

```text
3 2 3
```

### GATE insight

Do not assume:

> "The caller's local variable becomes visible inside the called function."

That would be **dynamic scoping**, not C.

---

# 28. Global vs Local Variable in Functions

Example from the lecture:

```c
int X = 0;
int Y;

void fun()
{
    X++;
}

void foo()
{
    X++;
    fun();
}

main()
{
    read(Y);

    if(Y > 0)
    {
        int X = 5;
        foo();
    }
    else
        foo();

    write(X);
}
```

The important idea is **scope**, not the syntax of `read()`/`write()`.

The `X` declared inside the `if` block:

```c
int X = 5;
```

is local to that block.

It does **not** replace the global `X` used by `foo()` and `fun()`.

Because those functions are statically scoped.

---

# 29. Static Scope Problem-Solving Method

When you encounter a variable inside a function:

### Don't ask:

> "Who called this function?"

Instead ask:

> **"Where was this function defined, and what declaration is visible there?"**

Example:

```text
main()
 ↓
bar()
 ↓
foo()
```

For variable `x` inside `foo()`:

```text
Look around foo's lexical environment
        ↓
not bar's local variables
        ↓
not main's local variables
        ↓
find appropriate enclosing declaration
```

This mental model solves many GATE questions.

---

# 30. Lecture's Main GATE Traps

## Trap 1 — `getchar()`

```c
getchar()
```

reads **one character**.

But its return type is:

```c
int
```

because of `EOF`.

---

## Trap 2 — `putchar()`

```c
putchar(c)
```

≈

```c
printf("%c", c)
```

---

## Trap 3 — Macro expansion

Always expand first.

```c
#define MULTIPLY(a,b) a*b
```

does **not** automatically mean:

```c
(a*b)
```

---

## Trap 4 — Parenthesize macros

Prefer:

```c
#define MULTIPLY(a,b) ((a)*(b))
```

---

## Trap 5 — C parameter passing

C uses:

> **Call by value**

Pointers can be used to modify the caller's object, but the pointer itself is still passed by value.

---

## Trap 6 — Static scoping

C uses:

> **Static/Lexical scoping**

Not dynamic scoping.

---

## Trap 7 — Caller does not determine variable binding

```c
bar()
{
    int x = 2;
    foo();
}
```

doesn't mean `foo()` automatically sees `x`.

Look at **where `foo()` was defined**.

---

# ⚡ Quick Revision — 60 Seconds

```text
getchar()
→ reads one character
→ returns int
→ EOF requires int

putchar(c)
→ prints one character
→ equivalent to printf("%c", c)

EOF
→ special end-of-input value
→ not an ordinary character

#define
→ preprocessor directive
→ textual substitution

#define MAX 100
→ MAX becomes 100 before compilation

Function-like macro
→ textual substitution, NOT function call

Macro trap
#define M(a,b) a*b
→ operator precedence can change result

Safe macro
#define M(a,b) ((a)*(b))

C parameter passing
→ call by value

Pointer argument
→ address is copied
→ both pointers can refer to same object

C scope
→ static / lexical scoping

Static scope
→ based on source-code structure

Dynamic scope
→ based on runtime call chain
→ C does NOT use it

Block variable
→ limited to block

Shadowing
→ inner declaration hides outer declaration

GATE trigger:
"Which x does function foo use?"
→ Look where foo is DEFINED, not who CALLS foo.
```

### 🎯 Question Triggers

When you see:

- **`getchar()` + recursion** → trace the **call stack/unwinding order**
    
- **`getchar() != EOF`** → remember return type is `int`
    
- **`#define`** → perform **text substitution first**
    
- **macro with arithmetic arguments** → check **parentheses + precedence**
    
- **pointer parameter** → distinguish **copy of address** from modification of pointed object
    
- **same variable name in caller/callee** → think **static scope**
    
- **nested `{}`** → check **block scope + shadowing**
    
- **function called from another function with same variable name** → caller's variable does **not** automatically become visible
    

This lecture is essentially a compact combination of **character I/O + preprocessor/macros + parameter passing + scope**, with the biggest GATE weight here being **macro expansion and static scoping**.