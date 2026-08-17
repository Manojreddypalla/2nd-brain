# `extern` Storage Class — Obsidian Notes

> **Lecture-only notes — Pages 53–65**

## 1. `extern` Storage Class

`extern` is used when a variable is **defined somewhere else** and we want to refer to it.

### Properties

|Property|`extern`|
|---|---|
|**Storage**|Static memory|
|**Initial value**|`0`|
|**Scope**|Global|
|**Lifetime**|Till end of program|
|**Available to linker**|✅ Yes|

---

## 2. Basic Idea

Suppose we have:

```c
int a;
```

and somewhere else:

```c
extern int a;
```

The `extern` declaration tells the compiler:

> **There is a global variable `a` somewhere.**

```text
extern int a;
       ↓
"Don't create a new variable.
 Refer to the existing global a."
```

---

# 3. `extern` Does NOT Create a New Variable

Example:

```c
int a;

int main()
{
    extern int a;
    printf("%d", a);
}
```

There is only **one `a`**:

```text
int a;
  ↓
Actual global variable
```

and:

```c
extern int a;
```

just refers to that variable.

### Think:

```text
Definition:
int a;
   ↓
Creates variable

Declaration:
extern int a;
   ↓
Refers to existing variable
```

---

# 4. Example from Lecture

```c
#include <stdio.h>

int max;

int main()
{
    int len;

    extern int max;

    printf("%d", max);

    max = 5;
}
```

Here:

```text
int max;
```

is the **global variable**.

```c
extern int max;
```

inside `main()` tells the compiler that `max` is a global variable.

Initially:

```text
max = 0
```

So:

```c
printf("%d", max);
```

prints:

```text
0
```

Then:

```c
max = 5;
```

changes the global variable to `5`.

---

# 5. `extern` Can Be Omitted

In the lecture's example:

```c
int max;

int main()
{
    int len;

    extern int max;

    printf("%d", max);

    max = 5;
}
```

The `extern` declaration is useful for explicitly telling the compiler about the global variable.

But in this case, because `max` is already visible as a global variable, the `extern` declaration can be omitted.

---

# 6. Why Use `extern`?

The important use shown in the lecture is when the global variable is **in another source file**.

Think:

```text
File 1
────────────
int max;
      ↓
global variable


File 2
────────────
extern int max;
      ↓
use the global variable
```

The linker connects the reference to the actual variable.

---

# 7. Compiler + Linker Idea

The lecture shows the process:

```text
Source code
    ↓
Preprocessing
    ↓
Compiler
    ↓
Assembler
    ↓
Object files
    ↓
Linker
    ↓
Executable
    ↓
Running program
```

`extern` becomes particularly useful when something needs to be resolved across source files.

---

# 8. `extern` and Linker

Very important:

```text
extern variable
      ↓
Available to linker
```

Why?

Because `extern` can refer to a global variable that may be defined in **another source file**.

```text
File A                    File B
──────                    ──────
int max;                  extern int max;
   │                          │
   └────────── Linker ────────┘
                    ↓
              same global max
```

---

# 9. `extern` and Local Variables

### ❌ Cannot use `extern` to refer to a local variable.

Example:

```c
int main()
{
    int a;

    extern int a;   // ❌
}
```

Here the `extern` is trying to refer to the local `a`.

The lecture explicitly says:

> We cannot refer to local variables using `extern`.

---

# 10. Correct Use

```c
int a;       // global variable

int main()
{
    extern int a;   // refers to global a

    printf("%d", a);
}
```

Here:

```text
int a;
  ↓
Global variable

extern int a;
  ↓
Reference to global variable
```

This is valid.

---

# 11. Important Note About `extern`

The lecture gives this exact point:

> **`extern` always refers to global variables, but it can be used with local or global variables.**

Meaning:

```text
extern declaration can appear:
        ↓
   local scope
   OR
   global scope

But what it refers to:
        ↓
   GLOBAL variable
```

---

# 12. `extern` Inside a Function

This is valid:

```c
int a;

int main()
{
    extern int a;

    printf("%d", a);
}
```

The `extern` declaration is **inside `main()`**, but it refers to the **global `a`**.

```text
          global a
             ↑
             │
        extern int a
             │
          inside main
```

---

# 13. Real Use of `extern`

The lecture marks this as the **real use** of the keyword:

```text
extern
  ↓
Multiple source files
  ↓
2 files or more
```

Example:

### `file1.c`

```c
int max = 5;
```

### `file2.c`

```c
extern int max;

printf("%d", max);
```

Both files can work with the same global variable through linking.

---

# ⭐ GATE QUICK REVISION

```text
extern
│
├── Storage       → Static memory
├── Initial value → 0
├── Scope         → Global
├── Lifetime      → Till end of program
└── Linker        → Available
```

### Core idea

```text
int a;
   ↓
Defines/creates global variable

extern int a;
   ↓
Refers to existing global variable
```

### Most important

```text
extern
→ refers to GLOBAL variable
→ can be declared inside a function
→ cannot refer to a local variable
→ useful across multiple source files
→ linker resolves the reference
```

---

# `static` vs `extern`

||`static`|`extern`|
|---|---|---|
|Storage|Static memory|Static memory|
|Initial value|`0`|`0`|
|Lifetime|End of program|End of program|
|Linker|❌ Not available|✅ Available|
|Main idea|Static storage / scope behavior|Refer to existing global variable|
|Multiple files|—|✅ Main use|

### 🔥 Remember

```text
static
→ "Keep this variable in static storage."

extern
→ "The variable exists somewhere else;
    I want to refer to it."
```