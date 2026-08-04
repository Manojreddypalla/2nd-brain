# C Programming — Basic C Program, Variables & `printf()`

## 1. Reference Books

The course primarily follows:

### The C Programming Language — K&R

Written by:

- Brian Kernighan
    
- Dennis Ritchie
    

Dennis Ritchie is the creator of the **C programming language**.

The book is commonly called:

```text
K&R
```

### Computer Systems: A Programmer's Perspective

Used particularly for understanding lower-level concepts such as:

- Signed integers
    
- Unsigned integers
    
- Number representation
    
- How programs interact with computer systems
    

---

# 2. First C Program

A basic C program:

```c
#include <stdio.h>

int main() {
    printf("Hello World\n");
    return 0;
}
```

For now, focus mainly on:

```c
printf("Hello World\n");
```

Do not worry yet about fully understanding:

```c
#include <stdio.h>
int main()
return 0
```

These will make more sense as the course progresses.

> [!important]  
> Learn C layer by layer. Don't try to understand the entire program before learning the basic operations inside `main()`.

---

# 3. `printf()`

> [!definition] printf()  
> `printf()` is a C library function used to produce **formatted output**.

Basic example:

```c
printf("Hello");
```

Output:

```text
Hello
```

Anything written normally inside the double quotes is printed as written.

---

# 4. Newline — `\n`

Example:

```c
printf("Hello\n");
printf("Hi");
```

Output:

```text
Hello
Hi
```

Here:

```text
\n
```

means **newline**.

> [!definition] `\n`  
> `\n` is an escape sequence that moves the output position to the beginning of the next line.

Mental model:

```text
printf("Hello\n");

Hello|
      ↓

Hello
|
```

The cursor moves to the next line.

---

# 5. Text Inside `printf()` Is Not Automatically Calculated

Consider:

```c
printf("5 + 10");
```

Output:

```text
5 + 10
```

NOT:

```text
15
```

Why?

Because:

```text
"5 + 10"
```

is text inside double quotes.

`printf()` does not treat that text as an arithmetic expression.

Mental model:

```text
Inside " "
     ↓
Treat as text
     ↓
Print it
```

---

# 6. Keywords in C

> [!definition] Keyword  
> A **keyword** is a reserved word in C that has a predefined meaning in the language.

Examples include:

```c
int
char
float
double
if
else
for
while
return
```

Because keywords already have special meanings, they cannot normally be used as variable names.

Incorrect:

```c
int return = 5;
```

`return` is already a C keyword.

---

# 7. Variables

Consider:

```c
int x = 5;
```

There are three important parts:

```text
int     x     = 5
 ↓      ↓       ↓
Type   Name    Value
```

> [!definition] Variable  
> A **variable** is a named object used to store a value that can be accessed through its name.

Here:

```text
x
```

is the variable name.

Its value is:

```text
5
```

Its type is:

```text
int
```

---

# 8. Data Types

> [!definition] Data Type  
> A **data type** specifies the kind of value represented by an object and determines how that value is interpreted.

Common C data types introduced here:

```text
char
short
int
float
double
long double
```

Examples:

```c
int age = 22;

char grade = 'A';

double balance = 1000.50;
```

---

# 9. Signed and Unsigned Types

Integer types can represent signed or unsigned values.

Examples:

```c
signed int x;
unsigned int y;
```

### Signed

Can represent values on both sides of zero.

Conceptually:

```text
..., -3, -2, -1, 0, 1, 2, 3, ...
```

### Unsigned

Represents only non-negative values.

Conceptually:

```text
0, 1, 2, 3, 4, ...
```

Integer types such as:

```text
char
short
int
```

can have signed/unsigned variants.

The lecture does not introduce `unsigned` versions of:

```text
float
double
long double
```

> [!important]  
> Don't memorize the exact ranges yet.
> 
> First understand **how integers are represented using bits**. The ranges will then make sense naturally.

---

# 10. Variables and Memory

This is the most important conceptual part of this lecture.

Suppose we write:

```c
int x = 5;
```

The value must eventually exist somewhere in **memory**.

A useful simplified model of memory is:

```text
Address        Memory
────────      ────────
0             [...]
1             [...]
2             [...]
3             [...]
4             [...]
5             [...]
...           [...]
```

Memory can be viewed as a **one-dimensional sequence of bytes**, where each byte has an address.

---

# 11. What Happens With `int x = 5`?

Conceptually:

```c
int x = 5;
```

means that storage is associated with `x`, and that storage represents the value `5`.

Simplified mental model:

```text
Memory

Address
   ↓

... ┌───────────┐
    │     5     │ ← x
... └───────────┘
```

The exact number of bytes used depends on the type and implementation.

But conceptually:

```text
x
↓
name used to access an object in memory
↓
value = 5
```

> [!important]  
> Don't think of `x` as literally being a box floating somewhere.
> 
> Think:
> 
> **`x` is a name in the C program associated with an object whose value is represented in memory.**

This mental model becomes extremely important later for:

```text
Arrays
Pointers
Structures
Dynamic Memory
Linked Lists
Trees
```

---

# 12. Memory Is Byte-Addressable

A useful model:

```text
Address

1000 → [ byte ]
1001 → [ byte ]
1002 → [ byte ]
1003 → [ byte ]
1004 → [ byte ]
...
```

Each byte has an address.

An object may occupy one or more consecutive bytes.

For example, conceptually:

```text
int x = 5;

        x
        ↓
1000 → [....]
1001 → [....]
1002 → [....]
1003 → [....]
```

The lecture intentionally postpones the exact integer size/details.

---

# 13. But How Is `5` Actually Stored?

This leads to an important question.

We write:

```c
int x = 5;
```

But computer memory ultimately stores information using **bits**:

```text
0
1
```

So the real question becomes:

```text
5
↓
Binary representation
↓
Bits
↓
Memory
```

Similarly:

```text
-5
↓
???
↓
Bits
↓
Memory
```

Understanding this requires studying:

- Binary numbers
    
- Signed numbers
    
- Unsigned numbers
    

That is why the course temporarily moves into **Number Systems** before continuing deeper into C.

---

# 14. Format Specifiers

Sometimes we want `printf()` to print the value of a variable.

Example:

```c
int a = 5;

printf("%d", a);
```

Output:

```text
5
```

Here:

```text
%d
```

is a **format specifier**.

> [!definition] Format Specifier  
> A **format specifier** tells a formatted input/output function how a corresponding value should be interpreted and displayed.

Some format specifiers introduced:

|Format|Used for|
|---|---|
|`%d`|signed decimal integer|
|`%c`|character|
|`%f`|floating-point output|
|`%s`|string|
|`%p`|pointer/address representation|
|`%x`|hexadecimal integer|

You will learn the exact rules gradually.

---

# 15. How `printf()` Format Specifiers Work

Consider:

```c
int a = 5;

printf("Value = %d", a);
```

Think of:

```text
"Value = %d"
          ↑
      placeholder
```

and:

```text
a = 5
```

So conceptually:

```text
Value = %d
        ↓
        a
        ↓
        5
```

Output:

```text
Value = 5
```

---

# 16. Multiple Format Specifiers

Consider:

```c
int a = 5;
int b = 10;

printf("%d %d", a, b);
```

The arguments correspond in order.

```text
%d     %d
 ↓      ↓
 a      b
 ↓      ↓
 5      10
```

Output:

```text
5 10
```

Another example:

```c
printf("a = %d, b = %d", a, b);
```

Output:

```text
a = 5, b = 10
```

---

# 17. Literal Text vs Expression

This distinction is extremely important.

### Case 1

```c
printf("5 + 10");
```

Output:

```text
5 + 10
```

Because:

```text
"5 + 10"
```

is literal text.

---

### Case 2

```c
printf("%d", 5 + 10);
```

Now:

```text
5 + 10
```

is an actual C expression passed as an argument.

The expression is evaluated:

```text
5 + 10
   ↓
  15
```

Then `%d` displays the resulting integer.

Output:

```text
15
```

---

# 18. Expression Inside the Argument

Suppose:

```c
int a = 5;
int b = 10;

printf("%d", a + b);
```

First:

```text
a + b
↓
5 + 10
↓
15
```

Then conceptually:

```text
printf("%d", 15);
```

Output:

```text
15
```

Mental model:

```text
Expression
    ↓
Evaluate
    ↓
Result
    ↓
printf()
    ↓
Output
```

---

# 19. Important Example

Consider:

```c
printf("3 + 2 = %d", 3 + 2);
```

Inside the quotes:

```text
3 + 2 =
```

is printed literally.

But:

```c
3 + 2
```

outside the quotes is evaluated.

Therefore:

```text
3 + 2
↓
5
```

Output:

```text
3 + 2 = 5
```

Another example:

```c
printf("3 + 2 = %d and 3 - 2 = %d",
       3 + 2,
       3 - 2);
```

Evaluation:

```text
3 + 2 → 5
3 - 2 → 1
```

Output:

```text
3 + 2 = 5 and 3 - 2 = 1
```

---

# 20. The Core `printf()` Mental Model

Think of:

```c
printf("x = %d", x);
```

as:

```text
FORMAT STRING
      +
ARGUMENTS
      ↓
printf interprets them together
      ↓
OUTPUT
```

For example:

```c
int x = 10;

printf("x = %d", x);
```

Mental execution:

```text
x
↓
10

"x = %d"
      ↑
replace according to corresponding argument

↓
"x = 10"
```

---

# 21. Source Code → Execution

The lecture also briefly shows the workflow:

```text
C Source Code
      ↓
Compile
      ↓
Executable
      ↓
Run
      ↓
Output
```

On a typical Linux setup, compilation may produce something such as:

```text
a.out
```

which can then be executed.

The exact compilation process will be studied later.

---

# 22. Why Study Number Systems Now?

Consider:

```c
int x = -5;
int a = 3;
```

At the C level, these simply look like numbers.

But internally:

```text
-5
3
```

must eventually have binary representations.

Memory ultimately stores bits:

```text
0 and 1
```

Therefore we need to understand:

```text
Decimal Number
      ↓
Binary Representation
      ↓
Stored Bits
      ↓
Memory
```

The next topic introduces enough **Number Systems** to understand how C represents integer values.

The deeper treatment belongs to **Digital Logic**.

---

# 23. Connection to Data Structures

This memory model is going to become very important later.

Right now:

```c
int x = 5;
```

Think:

```text
Variable
   ↓
Object in memory
```

Later:

```text
Array
↓
Multiple elements arranged in memory
```

Then:

```text
Pointer
↓
Value used to refer to a memory location
```

Then:

```text
Linked List
↓
Objects connected using pointers
```

Then:

```text
Tree
↓
Objects connected through pointer relationships
```

So the concepts you are learning now are the foundation for your later DSA work.

---

# 24. Core Definitions — Quick Revision

> [!definition] Keyword  
> A reserved word in C with a predefined meaning.

> [!definition] Variable  
> A named object used to store and access a value.

> [!definition] Data Type  
> Specifies the kind of value represented by an object and how that value should be interpreted.

> [!definition] Memory Address  
> A numeric identifier used to locate a byte in memory.

> [!definition] `printf()`  
> A formatted-output function used to produce output.

> [!definition] Format Specifier  
> A conversion specification such as `%d` or `%c` that tells `printf()` how a corresponding argument should be formatted.

> [!definition] `\n`  
> An escape sequence representing a newline.

---

# 25. GATE Traps to Start Noticing

### Trap 1 — Text is not automatically evaluated

```c
printf("5 + 10");
```

prints:

```text
5 + 10
```

---

### Trap 2 — Expression is evaluated

```c
printf("%d", 5 + 10);
```

prints:

```text
15
```

---

### Trap 3 — Format specifiers correspond to arguments

```c
printf("%d %d", a, b);
```

Think:

```text
first %d  ← a
second %d ← b
```

---

### Trap 4 — Don't assume how integers are stored yet

```c
int x = -5;
```

raises the deeper question:

```text
How is -5 represented using only bits?
```

That requires signed-number representation, which comes next.

---

# 26. Final Mental Model

Keep this picture in your head:

```text
C CODE

int x = 5;
printf("%d", x);

        ↓

VARIABLE

x
↓
value represented in memory

        ↓

EXPRESSION / VALUE

5

        ↓

printf()

%d ← corresponding integer value

        ↓

SCREEN

5
```

And underneath everything:

```text
C Variable
    ↓
Data Type
    ↓
Value Representation
    ↓
Bits
    ↓
Memory
```

---

# What You Should Remember

For now, remember these five things:

1. `int x = 5` creates/initializes an integer object named `x` with value `5`.
    
2. Program values ultimately have representations in memory.
    
3. `printf()` produces formatted output.
    
4. Text inside `" "` is literal except where formatting/escape rules apply.
    
5. Expressions passed as arguments are evaluated before their resulting values are formatted.
    

Most importantly:

> **Variable → Type → Value → Representation → Memory**

The next Number Systems section explains the **representation** part of this chain.