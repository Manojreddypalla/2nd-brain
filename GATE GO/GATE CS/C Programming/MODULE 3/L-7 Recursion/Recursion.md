# Recursion — GATE Short Notes

## 1. Core Mental Model

Think of recursion as:

> **A function creates a new activation record every time it calls itself.**

Each recursive call has its own:

- parameters
- local automatic variables
- return address
- execution state

But **static variables are shared across all calls**.

This distinction is extremely important for C output questions.

---

# 2. Base Case + Recursive Case

Every useful recursive function has:

```
if (base_condition)
    return base_value;
else
    recursive_call();
```

Example from the lecture:

```
int factorial(int n)
{
    if (n == 0 || n == 1)
        return 1;

    return n * factorial(n - 1);
}
```

Mathematically:

fact(n)={1n×fact(n−1)​n=0,1n>1​

The lecture illustrates this using `factorial(3)` and its chain of calls.

---

# 3. Recursion ≠ Iteration

The important difference isn't merely “function calls itself.”

### Iteration

```
one activation record
      ↓
loop changes state
      ↓
same function frame
```

### Recursion

```
f(5)
 ↓
f(4)
 ↓
f(3)
 ↓
f(2)
 ↓
f(1)
```

Each call creates a **new stack frame**.

So recursion naturally consumes stack space.

---

# 4. CALL → RETURN Order

This is probably the **most important thing for output questions**.

Consider:

```
void fun(int n)
{
    if (n == 0)
        return;

    printf("%d ", n);
    fun(n - 1);
}
```

For:

```
fun(3);
```

Execution:

```
fun(3)
 print 3
   ↓
 fun(2)
  print 2
    ↓
   fun(1)
    print 1
      ↓
     fun(0)
      return
```

Output:

```
3 2 1
```

### But:

```
void fun(int n)
{
    if (n == 0)
        return;

    fun(n - 1);
    printf("%d ", n);
}
```

Now printing happens **after the recursive call**.

```
fun(3)
 ↓
fun(2)
 ↓
fun(1)
 ↓
fun(0)
 ↑ print 1
 ↑ print 2
 ↑ print 3
```

Output:

```
1 2 3
```

### GATE rule

**Before recursive call → descending order**

**After recursive call → ascending order**

The lecture repeatedly tests exactly this distinction.

---

# 5. Activation Record / Stack

For:

```
factorial(3)
```

conceptually:

```
┌──────────────┐
│ factorial(1) │
├──────────────┤
│ factorial(2) │
├──────────────┤
│ factorial(3) │
├──────────────┤
│ main()       │
└──────────────┘
```

When `factorial(1)` returns:

```
factorial(1) → 1
factorial(2) → 2 × 1 = 2
factorial(3) → 3 × 2 = 6
```

So recursion has **two phases**:

```
CALLING PHASE
3 → 2 → 1 → base

RETURNING PHASE
1 → 2 → 6
```

The lecture explicitly visualizes these activation records and the call/return process.

---

# 6. Recursion Tree

For:

```
countdown(5)
{
    if(n == 0)
        return;

    printf("%d", n);
    countdown(n-1);
}
```

Tree:

```
countdown(5)
     |
countdown(4)
     |
countdown(3)
     |
countdown(2)
     |
countdown(1)
     |
countdown(0)
```

Only **one recursive call per activation**.

Therefore:

T(n)=T(n−1)+O(1)

Hence:

T(n)=O(n)​

Space:

O(n)​

because at maximum depth there are `n` active stack frames.

---

# 7. Multiple Recursive Calls

This is where recursion starts connecting directly to **DP**.

Example:

```
f(n)
{
    return f(n-1) + f(n-2);
}
```

Tree:

```
             f(5)
           /      \
        f(4)      f(3)
       /   \      /   \
     f(3) f(2)  f(2) f(1)
      ...
```

Notice:

```
f(3)
```

gets calculated more than once.

That's the key DP connection:

> **Recursion gives the state-transition structure.  
> DP avoids recomputing the same states.**

So when you see:

```
recursive solution
+
same subproblem appears repeatedly
```

your brain should immediately think:

```
OVERLAPPING SUBPROBLEMS
        ↓
MEMOIZATION / TABULATION
```

---

# 8. Static Variable — VERY IMPORTANT

This lecture spends significant time on `static` recursion questions.

Example:

```
void fun()
{
    static int n = 5;

    if(n == 0)
        return;

    n--;
    fun();
    printf("%d ", n);
}
```

### Critical distinction

Normal local:

```
int n;
```

Each recursive call gets its **own copy**.

Static:

```
static int n;
```

There is **only one copy shared across all calls**.

Think:

```
Automatic local:

fun() → n₁
fun() → n₂
fun() → n₃


Static:

          ┌── n
fun() ────┤
fun() ────┤
fun() ────┤
```

### Static variable properties

- initialized only once
- retains value between function calls
- shared by recursive calls
- lifetime = entire program

---

# 9. Static + Recursion = Two Dimensions of State

This is a great way to solve tricky output questions.

Suppose:

```
static int i = 5;
```

Then distinguish:

### Stack state

```
Call 1 → its parameters/local variables
Call 2 → its parameters/local variables
Call 3 → its parameters/local variables
```

### Static state

```
             ONE shared i
                  ↓
Call 1 ───────────┤
Call 2 ───────────┤
Call 3 ───────────┤
```

So when tracing recursion, ask:

> **Is this variable attached to the stack frame or shared globally through static storage?**

That one question destroys a lot of GATE recursion traps.

---

# 10. `++i` / `--i` With Recursive Calls

The lecture has several questions around this pattern.

For:

```
if (--i)
{
    recurse();
    printf("%d", i);
}
```

remember:

```
--i
```

means:

```
decrement FIRST
then evaluate condition
```

Whereas:

```
if (i--)
```

means:

```
evaluate old value
then decrement
```

### GATE trap

Never mentally treat:

```
--i
```

and

```
i--
```

as interchangeable.

---

# 11. Parameter vs Static Variable

Very important distinction.

```
void fun(int n)
{
    static int x;
}
```

Here:

```
n → separate for each call

x → shared among every call
```

Example:

```
fun(3)
 n = 3
 x = 0

    fun(2)
     n = 2
     x = 1

        fun(1)
         n = 1
         x = 2
```

So:

Parameter/local→per activation​ static→shared​

---

# 12. Recursive Output Pattern

Whenever you see:

```
fun(n);
printf(...);
```

think:

```
CALL FIRST
RETURN
PRINT
```

Therefore output comes from **bottom → top**.

But:

```
printf(...);
fun(n);
```

means:

```
PRINT FIRST
CALL
```

Therefore output comes from **top → bottom**.

### Cheat table

|Code position|Output direction|
|---|---|
|`printf → recursive call`|descending|
|`recursive call → printf`|ascending|
|`printf → recursive call → printf`|both directions|

---

# 13. Recursion + `return`

Don't confuse:

```
return f(n-1);
```

with:

```
f(n-1);
```

The first **propagates the returned value**.

Example:

```
return n + f(n-1);
```

For:

```
f(3)
```

becomes:

```
3 + f(2)
3 + (2 + f(1))
3 + (2 + (1 + f(0)))
```

Then values resolve during unwinding.

---

# 14. Recursion and Complexity

For one recursive call:

T(n)=T(n−1)+O(1)

→

O(n)​

For two calls such as:

T(n)=T(n−1)+T(n−2)+O(1)

→ exponential growth in the naive implementation.

For divide-and-conquer:

T(n)=2T(n/2)+O(n)

→

O(nlogn)

So **don't memorize “recursion = O(n)”**.

Instead:

> **Look at the recurrence generated by the recursive calls.**

---

# 15. Recursion → DP Connection

Since you've already studied DP, keep this mental chain:

```
RECURSION
   ↓
Define state
   ↓
Define transition
   ↓
Base case
   ↓
Recursive computation
   ↓
Do we recompute same state?
   ↓
YES
   ↓
MEMOIZATION / TABULATION
```

Example:

```
fib(5)
├── fib(4)
│   ├── fib(3)
│   └── fib(2)
└── fib(3)
    ├── fib(2)
    └── fib(1)
```

Repeated:

```
fib(3)
fib(2)
```

That's where DP enters.

So recursion isn't a separate island from DP.

**DP is often recursion + remembering answers.**

---

# 16. GATE Output Question Method

When you get a recursive C output question, **don't execute it mentally like normal code.**

Use this order:

### Step 1 — Find base case

```
if(...)
    return;
```

### Step 2 — Draw call chain/tree

```
f(5)
 ↓
f(4)
 ↓
f(3)
```

or:

```
       f(5)
      /    \
    f(4)   f(3)
```

### Step 3 — Mark variable type

Ask:

```
parameter?
local?
static?
global?
```

### Step 4 — Mark statements

```
BEFORE recursive call
AFTER recursive call
```

### Step 5 — Execute CALL phase

Go down the tree.

### Step 6 — Execute RETURN phase

Come back upward.

This is much safer than trying to “run” the entire program in your head.

---

# 🔥 GATE Traps From This Lecture

Remember these specifically:

1. **Recursive calls create separate activation records.**
2. **Static variables do NOT get a new copy per recursive call.**
3. `printf` **before** recursive call → output while going down.
4. `printf` **after** recursive call → output while coming back.
5. `--i` and `i--` have different evaluation order.
6. Don't assume recursion automatically means `O(n)`.
7. Multiple recursive calls can create a **recursion tree**.
8. Repeated states in that tree → **DP opportunity**.
9. Parameters/local automatic variables are **per activation**.
10. Static variables are **shared across recursive activations**.
11. Always distinguish **call phase vs return phase**.
12. For output questions, **draw the call tree/stack first**.

The lecture's later questions specifically test static state, recursive output ordering, pre/post decrement, forward declarations, and global/static interactions.

## 🧠 One-page mental model

```
                 RECURSION
                     │
           ┌─────────┴─────────┐
           ↓                   ↓
       CALL STACK          RECURSION TREE
           │                   │
     one call/frame       multiple calls
           │                   │
           ↓                   ↓
   activation record      repeated states?
                               │
                              YES
                               ↓
                              DP
```

And for C specifically:

```
                VARIABLE
                   │
       ┌───────────┴───────────┐
       ↓                       ↓
  local/parameter            static
       ↓                       ↓
new copy per call       ONE shared copy
       ↓                       ↓
stack frame             static storage
```

**For your level, this is the part I'd actually revise before GATE.** Don't waste time rewriting the definition of recursion again—you've already crossed that bridge. The valuable stuff now is **stack tracing + recursion trees + static variables + output order + recurrence/DP connection**.