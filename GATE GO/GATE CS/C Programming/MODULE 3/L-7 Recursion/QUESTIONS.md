# Recursion — All Questions + Answers

> **GATE Rule:** For recursion output questions, trace **CALL → BASE CASE → RETURN**.  
> Always check whether execution happens **before or after** the recursive call.

---

# Q1 — Countdown

```c
void countdown(int n)
{
    if(n == 0)
        return;

    printf("%d ", n);
    countdown(n - 1);
}

int main()
{
    countdown(5);
}
```

### Answer

```text
5 4 3 2 1
```

### Why?

`printf()` executes **before** recursion:

```text
countdown(5) → print 5
    countdown(4) → print 4
        countdown(3) → print 3
            countdown(2) → print 2
                countdown(1) → print 1
                    countdown(0) → return
```

### Pattern

**Print before recursive call → descending order.**

---

# Q2 — Count Up

```c
void countup(int n)
{
    if(n == 0)
        return;

    countup(n - 1);
    printf("%d ", n);
}

int main()
{
    countup(5);
}
```

### Answer

```text
1 2 3 4 5
```

### Why?

The recursive call happens first.

```text
countup(5)
 ↓
countup(4)
 ↓
countup(3)
 ↓
countup(2)
 ↓
countup(1)
 ↓
countup(0)
```

Then while returning:

```text
print 1
print 2
print 3
print 4
print 5
```

### Pattern

**Recursive call before print → ascending order.**

---

# Q3 — Static Counter

```c
int recursiveFunc(int n)
{
    static int count = 0;

    if(n <= 0)
        return count;

    count++;
    return recursiveFunc(n - 1);
}

int main()
{
    int result = recursiveFunc(5);
    printf("Result: %d", result);
}
```

### Answer

```text
Result: 5
```

### Trace

```text
recursiveFunc(5) → count = 1
recursiveFunc(4) → count = 2
recursiveFunc(3) → count = 3
recursiveFunc(2) → count = 4
recursiveFunc(1) → count = 5
recursiveFunc(0) → return 5
```

### Key Point

`count` is **static**, so all recursive calls share the same variable.

---

# Q4 — Static Counter + `n`

```c
int recursiveFunc(int n)
{
    static int count = 0;

    if(n <= 0)
        return count;

    count += n;
    return recursiveFunc(n - 1);
}

int main()
{
    int result = recursiveFunc(3);
    printf("Result: %d", result);
}
```

### Answer

```text
Result: 6
```

### Trace

```text
n = 3 → count = 3
n = 2 → count = 5
n = 1 → count = 6
n = 0 → return 6
```

[  
3+2+1=6  
]

---

# Q5 — Static Variable + Parameter

```c
void increase(int x)
{
    static int i = 0;

    x = i + x;
    i++;

    printf("x=%d, i=%d\n", x, i);
}

int main(void)
{
    int i = 0;

    for(i = 0; i < 4; ++i)
    {
        increase(i);
    }
}
```

### Answer

```text
x=0, i=1
x=2, i=2
x=4, i=3
x=6, i=4
```

### Trace

|Call|Parameter `x`|Static `i` before|`x = i+x`|Static `i` after|
|---|--:|--:|--:|--:|
|1|0|0|0|1|
|2|1|1|2|2|
|3|2|2|4|3|
|4|3|3|6|4|

### Important

There are **two different `i` variables**:

```text
main() → local i
increase() → static i
```

The static `i` retains its value between calls.

---

# Q6 — Static Variable Across Loop Calls

```c
int func(int a)
{
    static int j = 0;

    j++;
    return a * j;
}

int main(void)
{
    int x = 10;

    while(x > 0)
    {
        printf("%d\n", func(x));
        --x;
    }
}
```

### Answer

```text
10
18
24
28
30
30
28
24
18
10
```

### Trace

|`x`|`j` after `j++`|`x × j`|
|--:|--:|--:|
|10|1|10|
|9|2|18|
|8|3|24|
|7|4|28|
|6|5|30|
|5|6|30|
|4|7|28|
|3|8|24|
|2|9|18|
|1|10|10|

### Key Point

`j` is static → it **doesn't reset to 0** on every function call.

---

# Q7 — Post-Decrement in Recursive Argument

```c
void fun(int n)
{
    if(n == 0)
        return;

    fun(n--);
    printf("%d ", n);
}

int main()
{
    fun(3);
}
```

### Answer

```text
No output — infinite recursion / stack overflow.
```

### Why?

This is the trap:

```c
fun(n--);
```

`n--` passes the **old value**.

For `fun(3)`:

```text
current n = 3
n-- → passes 3
local n becomes 2
```

So:

```text
fun(3)
 ↓
fun(3)
 ↓
fun(3)
 ↓
fun(3)
 ↓
...
```

`n` never reaches `0` in the recursive argument.

### GATE Trap

```text
n--  → use old value, then decrement

--n  → decrement first, use new value
```

---

# Q8 — Pre-Decrement in Recursive Argument

```c
void fun(int n)
{
    if(n == 0)
        return;

    fun(--n);
    printf("%d ", n);
}

int main()
{
    fun(3);
}
```

### Answer

```text
0 1 2
```

### Trace

```text
fun(3)
 ↓ --n → 2
fun(2)
 ↓ --n → 1
fun(1)
 ↓ --n → 0
fun(0) → return
```

Now return:

```text
fun(1) → print 0
fun(2) → print 1
fun(3) → print 2
```

### Answer

```text
0 1 2
```

---

# Q9 — Program 1

```c
void recurse()
{
    static int i = 4;

    if(--i)
    {
        recurse();
        printf("%d", i);
    }
}
```

### Answer

```text
000
```

### Trace

```text
i = 4 → --i = 3 → recurse
i = 3 → --i = 2 → recurse
i = 2 → --i = 1 → recurse
i = 1 → --i = 0 → stop
```

Now return.

But `i` is **static and shared**.

So every previous call sees:

```text
i = 0
```

Therefore:

```text
000
```

---

# Q10 — Program 2

```c
void recurse()
{
    static int i = 4;

    printf("%d", i);

    if(--i)
        recurse();
}
```

### Answer

```text
4321
```

### Why?

Print happens before decrement:

```text
print 4 → i becomes 3
print 3 → i becomes 2
print 2 → i becomes 1
print 1 → i becomes 0
stop
```

### Answer

```text
4321
```

---

# Q11 — Program 3

```c
void recurse(int i)
{
    if(--i)
    {
        printf("%d", i);
        recurse(i);
    }
}
```

Call:

```c
recurse(4);
```

### Answer

```text
321
```

### Trace

```text
recurse(4)
 ↓ --i
i = 3 → print 3
 ↓
recurse(3)
 ↓ --i
i = 2 → print 2
 ↓
recurse(2)
 ↓ --i
i = 1 → print 1
 ↓
recurse(1)
 ↓ --i
i = 0 → stop
```

### Important

Here `i` is a **normal parameter**, not static.

Each call has its own copy.

---

# Q12 — Program 4

```c
void recurse(int i)
{
    printf("%d", i);

    if(--i)
        recurse(i);
}
```

Call:

```c
recurse(4);
```

### Answer

```text
4321
```

### Trace

```text
print 4
 ↓
i becomes 3
 ↓
print 3
 ↓
i becomes 2
 ↓
print 2
 ↓
i becomes 1
 ↓
print 1
 ↓
i becomes 0
 ↓
stop
```

### Answer

```text
4321
```

---

# Q9–Q12 Quick Comparison

|Program|Variable|Print position|Output|
|---|---|---|---|
|Q9|`static i`|after recursion|`000`|
|Q10|`static i`|before recursion|`4321`|
|Q11|parameter `i`|after decrement, before recursion|`321`|
|Q12|parameter `i`|before decrement|`4321`|

### Main lesson

Two things control the answer:

```text
1. Is variable STATIC or LOCAL?
2. Does PRINT happen before or after recursion?
```

---

# Q13 — Recursive Power

```c
double power(double x, int n)
{
    if(n == 1)
        return x;
    else
        return x * power(x, n - 1);

    --n;
}

int main()
{
    int a = power(2, 5);
    printf("%d", a);
}
```

### Answer

```text
32
```

### Trace

```text
power(2,5)
= 2 × power(2,4)
= 2 × 2 × power(2,3)
= 2 × 2 × 2 × power(2,2)
= 2 × 2 × 2 × 2 × power(2,1)
= 2 × 2 × 2 × 2 × 2
= 32
```

### Important Trap

```c
return x * power(...);

--n;
```

Anything after `return` is **unreachable**.

So:

```c
--n;
```

has no effect.

---

# Q14 — Mutual Recursion

```c
void func2(int n);

void func1(int n)
{
    if(n > 0)
    {
        printf("func1: %d\n", n);
        func2(n - 1);
    }
}

void func2(int n)
{
    if(n > 0)
    {
        printf("func2: %d\n", n);
        func1(n - 1);
    }
}
```

Call:

```c
func1(3);
```

### Answer

```text
func1: 3
func2: 2
func1: 1
```

### Trace

```text
func1(3)
 ↓
print func1: 3
 ↓
func2(2)
 ↓
print func2: 2
 ↓
func1(1)
 ↓
print func1: 1
 ↓
func2(0)
 ↓
stop
```

### Concept

This is **mutual recursion**:

```text
func1 → func2 → func1 → func2 → ...
```

A function doesn't necessarily have to call itself directly.

---

# Q15 — Global vs Local vs Static

```c
int i = 20;

int main()
{
    int i = 5;

    printf("%d ", i);

    r();

    printf("%d ", i);

    r();

    return 0;
}

void r()
{
    static int j;

    if(i < 10)
    {
        i = 3;
        printf("%d ", i);
    }

    printf("%d ", i + j);

    i++;
    j++;
}
```

### Answer

```text
5 20 5 22
```

### Why?

There are **three different variables**:

```text
main():
    int i = 5       → local to main

global:
    int i = 20      → global

r():
    static int j    → shared between r() calls
```

Inside `r()`:

```c
if(i < 10)
```

`r()` cannot see `main()`'s local `i`.

Therefore it uses the **global `i = 20`**.

---

### First `r()`

```text
global i = 20
static j = 0
```

Condition:

```text
20 < 10 → false
```

Print:

```text
i + j = 20 + 0 = 20
```

Then:

```text
i = 21
j = 1
```

---

### Second `r()`

```text
global i = 21
static j = 1
```

Condition:

```text
21 < 10 → false
```

Print:

```text
21 + 1 = 22
```

Then:

```text
i = 22
j = 2
```

Meanwhile, `main()`'s local `i` **always remains 5**.

### Final output

```text
5 20 5 22
```

---

# 🔥 Final GATE Revision Table

|Concept|Remember|
|---|---|
|Recursive call|New activation record|
|Local variable|Separate copy per call|
|Parameter|Separate copy per call|
|`static` variable|One shared copy|
|Global variable|Shared, unless shadowed|
|Print before recursion|Calling phase|
|Print after recursion|Returning phase|
|`--i`|Decrement → use|
|`i--`|Use → decrement|
|`return`|Function immediately exits|
|Multiple recursive calls|Recursion tree|
|Repeated states|DP opportunity|
|Mutual recursion|`f → g → f → g`|
|Recursion depth|Stack-space consideration|

# 🧠 The 5 Things to Check in Every GATE Recursion Question

```text
1. BASE CASE
      ↓
2. CALL TREE / CALL CHAIN
      ↓
3. STATIC vs LOCAL
      ↓
4. BEFORE vs AFTER RECURSION
      ↓
5. CALL PHASE → RETURN PHASE
```

**If you do these five things, most recursion output questions become mechanical rather than guesswork.**