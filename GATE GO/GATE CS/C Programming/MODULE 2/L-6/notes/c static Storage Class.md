# `static` Storage Class — Obsidian Notes

> **Source:** Lecture pages 36–52 only.  
> **Focus:** `static` storage class + lecture examples.

---

## 1. `static` Storage Class

`static` is one of the four storage classes in C:

```text
auto
register
static
extern
```

### Properties of `static`

| Property          | Value               |
| ----------------- | ------------------- |
| **Storage**       | Static memory       |
| **Initial value** | `0`                 |
| **Scope**         | Within block        |
| **Lifetime**      | Till end of program |

---

## 2. Local `static` Variable

```c
void fun()
{
    static int x = 5;
}
```

For a local `static` variable:

```text
Scope    → Within block
Lifetime → Entire program
Storage  → Static memory
```

### ⭐ Important

> **Local `static` = local scope + program-long lifetime**

So:

```text
Block ends
    ↓
Scope ends
    ↓
But variable still exists
    ↓
Until program ends
```

---

## 3. `auto` vs `static`

||`auto`|`static`|
|---|---|---|
|Storage|Stack|Static memory|
|Initial value|Garbage|`0`|
|Scope|Block|Block|
|Lifetime|End of block|End of program|
|Value survives function call?|❌|✅|

### Remember

```text
auto
→ Block scope + Block lifetime

static
→ Block scope + Program lifetime
```

---

## 4. Static Variable Retains Value

```c
void fun()
{
    static int x = 0;
    x++;
    printf("%d ", x);
}
```

If `fun()` is called 3 times:

```text
1st call → 1
2nd call → 2
3rd call → 3
```

Output:

```text
1 2 3
```

### Why?

`static` variable is **not destroyed when the function ends**.

```text
Call 1:
x = 0 → 1
       ↓
   function ends
       ↓
   x remains 1

Call 2:
x = 1 → 2
       ↓
   x remains 2

Call 3:
x = 2 → 3
```

---

## 5. Static Initialization

```c
void fun()
{
    static int x = 5;
    x++;
}
```

`x = 5` is **not executed again on every function call**.

```text
Initial:
x = 5

1st call → 6
2nd call → 7
3rd call → 8
```

So:

```text
static variable
→ initialized once
→ retains modified value
```

---

## 6. Global `static`

```c
static int x;
```

A global `static` variable is stored in:

```text
Static memory
```

### Important

```text
static ≠ global
```

`static` can be used with:

```text
Local variable
Global variable
```

---

## 7. Same Variable Name ≠ Same Variable

Example:

```c
static int y = 5;

void fun()
{
    static int y;
}

int main()
{
    static int y = 2;
}
```

There are **3 different variables**:

```text
Global y
fun()'s y
main()'s y
```

Each has separate storage and value.

### Key idea

```text
Same name
    +
Different scope
    ↓
Different variables
```

---

## 8. Static Variables in Different Functions

```c
int fun1()
{
    static int y = 5;
    y--;
    return y;
}

int fun2()
{
    static int y;
    y++;
    return y;
}
```

The two `y`s are **different variables**.

```text
fun1()'s y → starts at 5
fun2()'s y → starts at 0
```

### Values across calls

```text
fun1():

1st → 4
2nd → 3
3rd → 2
4th → 1
```

```text
fun2():

1st → 1
2nd → 2
3rd → 3
4th → 4
```

---

## 9. Lecture Output Question

```c
int x = 0;

for(int i = 1; i < 5; i++)
{
    x += fun1() + fun2();
}
```

### Track the values

|Iteration|`fun1()`|`fun2()`|`x`|
|--:|--:|--:|--:|
|1|4|1|5|
|2|3|2|10|
|3|2|3|15|
|4|1|4|20|

Therefore:

```text
Output = 20
```

---

# ⭐ GATE QUICK REVISION

```text
static
│
├── Storage       → Static memory
├── Initial value → 0
├── Scope         → Within block
└── Lifetime      → Till end of program
```

### Local `static`

```text
Local scope
+
Program-long lifetime
+
Value persists between function calls
```

### Most important rule

```text
static local variable
→ initialized once
→ does NOT reset on every function call
→ retains its previous value
```

### Output-question strategy

Whenever you see:

```c
static int x = ...;
```

**Make a separate state for `x` and carry its value to the next function call.**

```text
Call 1 → modify x
Call 2 → use modified x
Call 3 → use modified x
...
```

### One-line memory trick

> **`static` = block scope, program lifetime, value survives calls.**