Yes. You’re right — for **GATE preparation**, the PDF's examples and questions are important, not just the definitions.

I went through **all 74 pages of L-3(2).pdf** and pulled the actual **examples, GATE questions, output questions, and the answers/logic shown in the lecture**.

# L-3 C Programming — Complete GATE Notes + Questions

## 1. `if` Statement

### Syntax

```c
if (test_expression)
{
    statement-block;
}
```

The block executes when the test expression is **true**.

### C truth rule ⭐

```text
0       → FALSE
non-zero → TRUE
```

Example:

```c
if (0)
    printf("A");

printf("B");
printf("C");
```

Output:

```text
BC
```

Because `0` is false, while the statements outside `if` execute normally.

### GATE Rule

> **In C, there is no separate Boolean requirement for `if`; the expression is interpreted numerically.**

---

# 2. `if` Without Braces ⭐

```c
if (grade <= 10)
    printf("YOU DID NOT STUDY.\n");

printf("YOU Like this course !\n");
```

Only the **immediately following statement** belongs to `if`.

Equivalent:

```c
if (grade <= 10)
{
    printf("YOU DID NOT STUDY.\n");
}

printf("YOU Like this course !\n");
```

### ⚠️ GATE Trap

Indentation doesn't matter.

```c
if (condition)
    statement1;
    statement2;
```

means:

```c
if (condition)
    statement1;

statement2;
```

---

# 3. `if-else`

```c
if (condition)
{
    // true block
}
else
{
    // false block
}
```

If condition is true → `if` block.

If condition is false → `else` block.

Example from lecture:

```c
int i = 20;

if (i <= 15)
{
    printf("i is smaller than 15");
}
else
{
    printf("i is greater than 15");
}
```

Since:

```text
20 <= 15 → false
```

Output:

```text
i is greater than 15
```

---

# 4. Nested `if`

An `if` can contain another `if`.

```c
if (num > 0)
{
    if (num < 10)
    {
        printf("num is between 0 and 10");
    }
}
```

Execution:

```text
num > 0 ?
   ↓ yes
num < 10 ?
   ↓ yes
print
```

---

# 5. `else-if` Structure

```c
if (num > 90)
{
    printf("You earned an A");
}
else if (num > 80)
{
    printf("You earned a B");
}
```

The `else if` is essentially an `if` attached to the `else`.

---

# 6. Dangling `else` ⭐⭐⭐

Consider:

```c
if (a < 10)
    if (a % 2 == 0)
        printf("a is even and less than 10");
    else
        printf("mystery");
```

Which `if` does `else` belong to?

### Rule:

> **`else` always belongs to the nearest unmatched `if`.**

So it means:

```c
if (a < 10)
{
    if (a % 2 == 0)
        printf("a is even and less than 10");
    else
        printf("mystery");
}
```

### GATE shortcut

When you see:

```c
if(A)
    if(B)
        X;
    else
        Y;
```

Immediately mentally add:

```c
if(A)
{
    if(B)
        X;
    else
        Y;
}
```

---

# 7. `switch-case`

### Syntax

```c
switch (x)
{
    case constant1:
        ...
        break;

    case constant2:
        ...
        break;

    default:
        ...
}
```

---

## Switch Rules ⭐⭐⭐

### Rule 1 — Expression must produce an integer

```c
switch(x)
```

`x` must be an integer type or an expression that evaluates to an integer.

Examples from the lecture:

```c
int x;
char c;
short s;
```

are valid switch expressions.

---

### Rule 2 — Case values must be constant

```c
case 1:
case 5:
case 'A':
```

Case labels must be **constant and unique**.

---

### Rule 3 — Case labels can occur in any order

```c
case 5:
case 2:
case 10:
default:
```

Order does not matter.

---

### Rule 4 — `:` is required

```c
case 5:
```

not:

```c
case 5;
```

---

# 8. Switch Example

```c
int num = 8;

switch(num)
{
    case 7:
        printf("Value is 7");
        break;

    case 8:
        printf("Value is 8");
        break;

    case 9:
        printf("Value is 9");
        break;

    default:
        printf("Out of range");
        break;
}
```

`num = 8`.

Therefore:

```text
switch(8)
   ↓
case 8
   ↓
Value is 8
   ↓
break
   ↓
exit switch
```

---

# 9. Switch Fall-Through ⭐⭐⭐

If `break` is missing:

```c
switch(num)
{
    case 1:
        printf("Case 1");

    case 2:
        printf("Case 2");

    case 3:
        printf("Case 3");
}
```

If:

```c
num = 1;
```

then:

```text
case 1
 ↓
print Case 1
 ↓
case 2
 ↓
print Case 2
 ↓
case 3
 ↓
print Case 3
```

### Important

Once execution enters a matching case, it **continues downward** until:

- `break`, or
    
- end of switch.
    

This is **fall-through**.

---

# 10. Switch Question — Fall-Through

```c
int num = 3;

switch(num)
{
    case 1:
        printf("Case1");

    case 3:
        printf("Case3");

    case 2:
        printf("Case2");
        break;

    default:
        printf("Default");
}
```

Start:

```text
num = 3
```

Find:

```text
case 3
```

Then:

```text
Case3
 ↓
case 2
 ↓
Case2
 ↓
break
```

### Answer

```text
Case3
Case2
```

The lecture demonstrates this exact fall-through behavior.

---

# 11. GATE 2012 — Switch Question ⭐⭐⭐

**GATE CSE 2012, Question 3** appears in the lecture.

```c
char inChar = 'A';

switch(inChar)
{
    case 'A':
        printf("Choice A\n");

    case 'B':
    case 'C':
        printf("Choice B");

    case 'D':
    case 'E':
    default:
        printf("No Choice");
}
```

### Trace

`inChar = 'A'`

So:

```text
case 'A'
 ↓
Choice A
 ↓
case B
 ↓
case C
 ↓
Choice B
 ↓
case D
 ↓
case E
 ↓
default
 ↓
No Choice
```

There are **no `break`s**.

### ✅ Answer

```text
Choice A
Choice B
No Choice
```

The lecture marks this as the correct option.

### Pattern

```text
case A:
    code

case B:
case C:
    code

case D:
case E:
default:
    code
```

Multiple cases can intentionally share the same body.

---

# 12. Switch Expression Is Evaluated Once ⭐⭐⭐

Consider:

```c
int value = 0;

switch(value)
{
    default:
        value++;

    case 2:
        printf("Humans are human centric");
        break;

    case 1:
        printf("This is inhuman");
}

printf("%d", value);
```

### Step 1

Initially:

```text
value = 0
```

### Step 2

```c
switch(value)
```

No case `0` exists.

Therefore execution starts at:

```text
default
```

### Step 3

```c
value++;
```

Now:

```text
value = 1
```

### Critical point ⭐

C does **NOT** go back and search for `case 1`.

It has already chosen the entry point.

Execution simply continues downward:

```text
default
 ↓
value++
 ↓
case 2
 ↓
printf
 ↓
break
```

Therefore:

```text
Humans are human centric
1
```

The lecture explicitly highlights this behavior.

> **GATE Rule:** Changing the switch variable inside the switch does not cause the cases to be re-evaluated.

---

# 13. Switch Question — `switch(num+2)`

```c
int num = 2;

switch(num + 2)
{
    printf("Hey");

    case 1:
        printf("Case1: Value is %d", num);

    case 2:
        printf("Case2: Value is %d", num);

    case 3:
        printf("Case3: Value is %d", num);

    default:
        printf("Default Value is %d", num);
}
```

Calculate:

```text
num + 2 = 2 + 2 = 4
```

Cases:

```text
1
2
3
```

No case `4`.

Therefore execution starts at `default`.

### Answer

```text
Default Value is 2
```

Notice:

```text
switch expression = 4
```

but:

```text
num = 2
```

So `printf("%d", num)` prints **2**.

The lecture's answer confirms this.

---

# 14. Invalid `case` — Important GATE Trap ⭐

```c
int month = 2;
int year = 2000;
int numDays = 0;

switch(month)
{
    case month:
        printf("%d", numDays);
}
```

Is:

```c
case month:
```

valid?

### ❌ No.

A `case` label requires an **integer constant expression**.

`month` is a variable, not a constant expression.

Therefore:

```text
Compilation Error
```

The lecture demonstrates the compiler rejecting this exact code.

### Remember

```c
case 1:       // ✅
case 2 + 3:   // ✅
case 'A':     // ✅

case x:       // ❌ if x is variable
```

---

# 15. Loops

Loops allow repeated execution of statements.

Without loops:

```c
printf("Programming is fun!");
printf("Programming is fun!");
printf("Programming is fun!");
...
```

For 100 repetitions, that would mean writing the statement 100 times.

C provides:

```text
while
do-while
for
```

---

# 16. `while` Loop

### Syntax

```c
while(condition)
{
    statements;
}
```

Execution:

```text
condition
   ↓
true?
 /   \
yes   no
 ↓     ↓
body   exit
 ↓
condition again
```

### Example

```c
int count = 0;

while(count < 100)
{
    printf("Programming is fun!\n");
    count = count + 1;
}
```

`count` moves:

```text
0 → 1 → 2 → ... → 99 → 100
```

Then:

```text
100 < 100 → false
```

Exit.

---

# 17. While Loop Question

```c
int x = 10;

while(x > 0)
{
    x = x - 3;
}
```

Trace:

```text
x = 10
 ↓
10 > 0 → yes → x = 7
 ↓
7 > 0 → yes → x = 4
 ↓
4 > 0 → yes → x = 1
 ↓
1 > 0 → yes → x = -2
 ↓
-2 > 0 → false
```

### ✅ Answer

```text
x = -2
```

The lecture marks option **B (-2)**.

### GATE pattern

Don't stop when the variable crosses the boundary.

The update happens **after the condition has already allowed that iteration**.

---

# 18. `do-while`

### Syntax

```c
do
{
    statements;
}
while(condition);
```

⚠️ **Semicolon is mandatory.**

Flow:

```text
body
 ↓
condition
 ↓
true → body again
false → exit
```

Unlike `while`, the condition is checked **after** the body.

---

# 19. `do-while` Question

```c
int n = 0, m = 1;

do
{
    printf("%d", m);
    m++;
}
while(m <= n);
```

Initially:

```text
n = 0
m = 1
```

### First iteration

Body executes **without checking condition first**:

```text
print 1
m = 2
```

Now:

```text
2 <= 0 → false
```

Exit.

### ✅ Output

```text
1
```

The lecture demonstrates this example.

---

# 20. `do-while` Example — `n = 4`

```c
int n = 4, m = 1;

do
{
    printf("%d", m);
    m++;
}
while(m <= n);
```

Trace:

```text
m=1 → print 1
m=2 → print 2
m=3 → print 3
m=4 → print 4
m=5

5 <= 4 → false
```

### Output

```text
1234
```

---

# 21. `do-while` — Executes At Least Once ⭐⭐⭐

```c
int m = 3;

do
{
    printf("%d", m);
    m = 0;
}
while(m > 0);
```

Even though eventually:

```text
m > 0 → false
```

the body has already executed once.

### Output

```text
3
```

### GATE Rule

```text
while     → possibly 0 executions
do-while  → minimum 1 execution
```

---

# 22. `while` vs `do-while`

```text
while:
condition → body

do-while:
body → condition
```

Therefore:

||`while`|`do-while`|
|---|---|---|
|Condition|Before body|After body|
|Minimum execution|0|1|
|Type|Entry controlled|Exit controlled|
|`;` after condition|No|Yes|

---

# 23. `for` Loop

### Syntax

```c
for(initialization; test; update)
{
    statements;
}
```

Execution order:

```text
initialization
      ↓
    test
      ↓
   body
      ↓
   update
      ↓
    test
      ↓
   body
      ↓
   update
      ↓
     ...
```

---

# 24. `for` Example — Reverse Loop

```c
int p;

for(p = 10; p > 0; p--)
{
    printf("%d", p);
}
```

Trace:

```text
10 9 8 7 6 5 4 3 2 1
```

Then:

```text
p = 0
0 > 0 → false
```

### Output

```text
10987654321
```

---

# 25. All Three Parts of `for` Are Optional ⭐⭐⭐

```c
for(initialization; test; update)
```

Each part can be omitted.

Example:

```c
int i = 0;

for(; i <= 9; )
{
    i++;
    printf("%d", i);
}
```

Trace:

```text
i=0
 ↓
0<=9 → yes
 ↓
i=1 → print 1

...

i=9 → print 9
i=10 → print 10

10<=9 → false
```

### Output

```text
12345678910
```

---

# 26. Empty `for` Loop — GATE Question ⭐⭐⭐

```c
int i;

for(i = 0; i <= 3; i++);

printf("%d", i);
```

Look carefully:

```c
for(i = 0; i <= 3; i++);
                                ↑
                         semicolon!
```

The loop body is an **empty statement**.

So:

```text
i=0
 ↓
i<=3 → i++
 ↓
i=1
 ↓
i<=3 → i++
 ↓
i=2
 ↓
i<=3 → i++
 ↓
i=3
 ↓
i<=3 → i++
 ↓
i=4
 ↓
4<=3 → false
```

Then:

```c
printf("%d", i);
```

prints:

```text
4
```

### ✅ Answer

```text
4
```

The lecture explicitly marks **C. 4**.

### ⭐ GATE Trap

```c
for(...);
```

is **not** a syntax error.

It means:

```c
for(...)
{
    // empty body
}
```

---

# 27. `for(;;)` ⭐

Because all three components are optional:

```c
for(;;)
{
}
```

is valid.

It behaves as an infinite loop unless something inside terminates it. The lecture's treatment of optional `for` expressions leads directly to this pattern.

---

# 28. `break`

`break` immediately exits the **nearest enclosing loop or switch**.

```c
while(condition)
{
    if(condition_to_break)
        break;

    // remaining code
}
```

---

# 29. `while(1)` + `break`

Lecture example:

```c
int a;

while(1)
{
    printf("enter the number:");
    scanf("%d", &a);

    if(a == 0)
        break;
}
```

`while(1)` means:

```text
1 → non-zero → true
```

So the loop is infinite.

But:

```c
if(a == 0)
    break;
```

terminates it.

### Key idea

```text
while(1)
   ↓
infinite loop
   ↓
break
   ↓
exit nearest loop
```

---

# 30. `break` in `while`

```c
while(testExpression)
{
    // code

    if(condition_to_break)
        break;

    // code
}
```

`break` jumps outside the loop.

It does **not** go back to the loop condition.

---

# 31. `break` in `do-while`

```c
do
{
    // code

    if(condition)
        break;

    // code

}
while(testExpression);
```

If `break` executes:

```text
break
 ↓
exit entire do-while
```

It does **not** reach the `while(testExpression)` condition.

---

# 32. `break` in `for`

```c
for(init; testExpression; update)
{
    // code

    if(condition)
        break;

    // code
}
```

When `break` executes:

```text
break
 ↓
exit for loop
```

The update expression does **not** execute after the break.

---

# 33. `break` in Nested Loops ⭐⭐⭐

```c
for(...)
{
    for(...)
    {
        if(condition)
            break;
    }
}
```

Which loop is terminated?

### Only the inner loop.

```text
Outer loop
   │
   └── Inner loop
          │
        break
          ↓
    exit INNER loop
          ↓
    outer continues
```

This is explicitly demonstrated in the lecture's nested-loop example.

> **GATE Rule:** `break` affects the **nearest enclosing loop/switch only**.

---

# 34. Break — GATE Output Example

```c
int hours = 10;
int i = 0;

for(i = 0; i < hours; i++)
{
    if(i == 5)
        break;

    printf("%d", i);
}
```

Trace:

```text
i=0 → print 0
i=1 → print 1
i=2 → print 2
i=3 → print 3
i=4 → print 4
i=5 → break
```

Notice:

```text
5 is NOT printed
```

### ✅ Output

```text
01234
```

The lecture gives this exact output.

---

# 35. `continue`

`continue` skips the **remaining body of the current iteration**.

It does **NOT** terminate the loop.

Example:

```c
for(int j = 0; j < 8; j++)
{
    if(j == 4)
        continue;

    printf("%d", j);
}
```

Trace:

```text
0 → print
1 → print
2 → print
3 → print
4 → continue → skip printf
5 → print
6 → print
7 → print
```

### Output

```text
0123567
```

The lecture demonstrates the skipped value behavior.

---

# 36. `continue` — Different Loops ⭐⭐⭐

This is **very important for GATE**.

## `while`

```c
while(condition)
{
    if(x)
        continue;

    // code
}
```

`continue` goes to:

```text
condition
```

---

## `do-while`

```c
do
{
    if(x)
        continue;

    // code
}
while(condition);
```

`continue` goes to:

```text
while(condition)
```

---

## `for`

```c
for(init; condition; update)
{
    if(x)
        continue;

    // code
}
```

`continue` goes to:

```text
update
 ↓
condition
```

The lecture explicitly compares all three.

### ⭐ Memorize this flow

```text
while:
continue → condition

do-while:
continue → condition

for:
continue → update → condition
```

---

# 37. `break` vs `continue`

||`break`|`continue`|
|---|---|---|
|Loop terminates?|✅ Yes|❌ No|
|Current iteration ends?|Yes|Yes|
|Goes to next iteration?|❌|✅|
|`for` update runs?|❌|✅|
|Affects nearest loop?|✅|✅|

### Mental model

```text
break
↓
LEAVE LOOP

continue
↓
SKIP REST OF CURRENT ITERATION
↓
NEXT ITERATION
```

---

# 38. Complete GATE Pattern Sheet

## `if`

```text
0       → false
non-zero → true
```

## `if` without braces

```text
Only next immediate statement belongs to if.
```

## Nested `if`

```text
Outer condition must succeed before inner condition is checked.
```

## Dangling `else`

```text
else → nearest unmatched if
```

## `switch`

```text
switch expression → integer
case → constant + unique
case order → irrelevant
```

## Switch execution

```text
Find matching case
       ↓
Start execution there
       ↓
Continue downward
       ↓
break → exit
```

## No matching case

```text
default → start there
```

## Important

```text
Changing switch variable does NOT re-search cases.
```

## `while`

```text
condition → body → condition
```

Can execute:

```text
0 times
```

## `do-while`

```text
body → condition → body...
```

Executes:

```text
at least 1 time
```

## `for`

```text
init → condition → body → update → condition...
```

## `for(;;)`

```text
infinite loop
```

## `for(...);`

```text
empty loop body
```

## `break`

```text
exit nearest loop/switch
```

## `continue`

```text
skip current iteration
```

And:

```text
while     → condition
do-while  → condition
for       → update → condition
```

---

# 🔥 L-3 GATE Questions — Final Answer List

|Question|Answer|
|---|---|
|`if(0)`|Body doesn't execute|
|Non-zero condition|True|
|`else` matching|Nearest unmatched `if`|
|Switch without `break`|Fall-through|
|GATE 2012 switch|**Choice A + Choice B + No Choice**|
|`switch(num+2)` where `num=2`, cases 1,2,3|**Default Value is 2**|
|`case month:` where `month` is variable|**Compilation error**|
|`switch(value=0)` → `default: value++`|**No re-evaluation; falls through**|
|`while(x>0) x-=3`, x=10|**-2**|
|`do-while`, n=0,m=1|**1**|
|`do-while`, n=4,m=1|**1234**|
|`do {print 3; m=0;} while(m>0)`|**3**|
|`for(i=0;i<=3;i++); printf("%d",i)`|**4**|
|`for(p=10;p>0;p--)`|**10 9 8 ... 1**|
|`for(;i<=9;) {i++;}` starting i=0|**1 to 10**|
|`for i=0..9`, break at i=5|**01234**|
|`for j=0..7`, continue at j=4|**0123567**|
|`break` in nested loop|Exits **inner/nearest** loop|

### ⭐ The 5 things I'd absolutely star in Obsidian

```text
1. else → nearest unmatched if

2. switch has fall-through unless break

3. switch expression is NOT re-evaluated after entering

4. do-while executes at least once

5. continue:
   while     → condition
   do-while  → condition
   for       → update → condition
```

These are the **high-yield traps** from this particular L-3 lecture, so I'd make sure they're visible in your revision notes.