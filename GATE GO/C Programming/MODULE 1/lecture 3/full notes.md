Absolutely. I went through **L-3(2).pdf** and kept the notes **short enough for revision, but deep enough for GATE**, including the important traps from the lecture.

# C Programming — Conditional Statements & Loops

> [!abstract] Core Idea  
> C provides **control-flow statements** to decide **which statements execute** and **how many times they execute**.
> 
> `if / else` → choose based on condition  
> `switch` → choose among discrete cases  
> `loops` → repeat statements  
> `break` → leave loop/switch  
> `continue` → skip current loop iteration

---

# 1. `if` Statement

### Syntax

```c
if (condition)
{
    statements;
}
```

- If condition is **true** → body executes.
    
- If condition is **false** → body is skipped.
    

### C condition rule

```text
0       → false
non-zero → true
```

So:

```c
if (0)       // false
if (1)       // true
if (-5)      // true
if (100)     // true
```

> [!important] GATE Point  
> **Only `0` is false. Every non-zero value is true.**

---

## Without `{}`

```c
if (condition)
    statement1;
statement2;
```

Only **the immediate next statement** belongs to the `if`.

Equivalent to:

```c
if (condition)
{
    statement1;
}

statement2;
```

> [!tip] Trap  
> Indentation does **not** determine the body of `if`.  
> `{}` determines it.

---

# 2. `if - else`

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

Execution:

```text
        condition
        /       \
     true       false
      ↓           ↓
    if-body    else-body
```

Exactly **one** of the two branches executes.

---

# 3. Nested `if`

An `if` can exist inside another `if`.

```c
if (num > 0)
{
    if (num < 10)
    {
        printf("0 < num < 10");
    }
}
```

The inner condition is checked **only if the outer condition is true**.

Mental model:

```text
Outer condition
      ↓ true
Inner condition
      ↓ true
   statement
```

---

# 4. `else-if` Chain

Useful when checking multiple mutually exclusive conditions:

```c
if (marks > 90)
    printf("A");
else if (marks > 80)
    printf("B");
else if (marks > 70)
    printf("C");
else
    printf("D");
```

Execution stops at the **first true condition**.

---

# 5. Dangling `else` ⭐

Consider:

```c
if (a)
    if (b)
        statement1;
    else
        statement2;
```

Which `if` does `else` belong to?

### Rule:

> **`else` always matches the nearest unmatched `if`.**

So it is equivalent to:

```c
if (a)
{
    if (b)
        statement1;
    else
        statement2;
}
```

Not:

```c
if (a)
{
    if (b)
        statement1;
}
else
{
    statement2;
}
```

The lecture demonstrates this matching rule through nested `if-else` examples.

> [!important] GATE Trap  
> When braces are absent, **count unmatched `if`s**, not indentation.

---

# 6. `switch-case`

Used when one expression needs to be compared against several **constant cases**.

### Syntax

```c
switch (expression)
{
    case constant1:
        statements;
        break;

    case constant2:
        statements;
        break;

    default:
        statements;
}
```

---

## Switch Rules

### 1. Switch expression → integer value

The expression after `switch` must produce an **integer value**.

Examples:

```c
switch (x)       // int
switch (x + 2)   // integer expression
switch (ch)      // char
```

### 2. `case` labels must be constant

```c
case 1:
case 10:
case 'A':
```

The case values must be **unique and constant**.

### 3. `case` order doesn't matter

`case` and `default` can occur in different orders.

### 4. `:` is mandatory

```c
case 5:
```

not:

```c
case 5;
```

---

# 7. How `switch` Executes ⭐

Suppose:

```c
int num = 9;

switch (num)
{
    case 7:
        printf("7");
        break;

    case 8:
        printf("8");
        break;

    case 9:
        printf("9");
        break;

    default:
        printf("Other");
}
```

Execution:

```text
switch(num)
     ↓
find matching case
     ↓
case 9
     ↓
execute statements
     ↓
break
     ↓
exit switch
```

Output:

```text
9
```

---

# 8. `break` in `switch` ⭐

`break` terminates the **nearest switch/loop**.

Without `break`:

```c
switch (x)
{
    case 1:
        printf("A");

    case 2:
        printf("B");

    case 3:
        printf("C");
}
```

If `x = 1`:

```text
case 1
 ↓
A
 ↓
case 2
 ↓
B
 ↓
case 3
 ↓
C
```

This is called **fall-through**.

The lecture specifically demonstrates cases executing into subsequent cases when `break` is absent.

> [!important] GATE  
> **Matching a case does NOT mean only that case executes.**
> 
> After entering a case, execution continues downward until:
> 
> - `break`
>     
> - end of `switch`
>     

---

# 9. `default`

```c
switch (x)
{
    case 1:
        ...
        break;

    default:
        ...
}
```

`default` executes when **no case matches**.

It is similar to the `else` branch of an `if-else` structure.

---

# 10. Switch — Important GATE Example

```c
int value = 0;

switch(value)
{
    default:
        value++;

    case 2:
        printf("Human");
        break;

    case 1:
        printf("Inhuman");
}
```

Since `value = 0`, no case matches.

So execution starts at:

```text
default
 ↓
value++
 ↓
value = 1
 ↓
case 2?
```

**No.**

Important:

> `switch` does **not re-evaluate cases** after `value` changes.

It has already selected the entry point. Execution simply falls through from there. The lecture uses this exact type of question.

---

# 11. Loops

> [!abstract] Intuition  
> A loop repeatedly executes a block of statements while a condition permits it.

General flow:

```text
       condition
       /       \
    true       false
      ↓          ↓
    body       exit
      ↓
  condition again
```

The lecture presents loops as a way to avoid writing the same statement repeatedly.

C has:

```text
while
do-while
for
```

---

# 12. `while` Loop

### Syntax

```c
while (condition)
{
    statements;
}
```

Flow:

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

while (count < 100)
{
    printf("Programming is fun!");
    count = count + 1;
}
```

> [!important] GATE  
> `while` is **entry-controlled**.
> 
> The condition is checked **before** the body.
> 
> Therefore it can execute **zero times**.

Example:

```c
while (0)
{
    printf("Hello");
}
```

Output:

```text
nothing
```

---

# 13. `do-while`

### Syntax

```c
do
{
    statements;
}
while (condition);
```

⚠️ **Semicolon after `while(condition)` is mandatory.**

Flow:

```text
    body
      ↓
  condition
   /     \
true     false
 ↓         ↓
body      exit
```

### Main difference

```text
while:
condition → body

do-while:
body → condition
```

Therefore:

> [!important] GATE  
> `do-while` is **exit-controlled** and executes **at least once**, regardless of the initial condition.

Example:

```c
int n = 0;

do
{
    printf("Hello");
} while (n > 0);
```

Output:

```text
Hello
```

---

# 14. `while` vs `do-while`

|Feature|`while`|`do-while`|
|---|---|---|
|Condition checked|Before body|After body|
|Type|Entry-controlled|Exit-controlled|
|Minimum executions|`0`|`1`|
|Semicolon after condition|No|**Yes**|

---

# 15. `for` Loop

### Syntax

```c
for (initialization; test; update)
{
    statements;
}
```

Think:

```text
initialization
      ↓
    test
      ↓
   true?
   /   \
 yes    no
  ↓      ↓
body    exit
  ↓
update
  ↓
 test again
```

### Example

```c
for (int i = 0; i < 5; i++)
{
    printf("%d", i);
}
```

Output:

```text
0 1 2 3 4
```

---

# 16. The 3 Parts of `for` Are Optional ⭐

```c
for (initialization; condition; update)
```

All three parts are optional.

### Example

```c
int i = 0;

for (; i <= 9; )
{
    i++;
}
```

This is valid.

### Infinite loop

```c
for (;;)
{
    // infinite
}
```

Equivalent to:

```c
while (1)
{
}
```

> [!important] GATE  
> `for(;;)` is a valid C loop and represents an **infinite loop** unless something inside terminates it.

---

# 17. Loop Tracing ⭐

For:

```c
for (i = 0; i <= 3; i++)
{
    printf("%d", i);
}
```

Trace:

```text
i = 0
 ↓
0 <= 3 → true → print 0
 ↓
i++
 ↓
i = 1
 ↓
1 <= 3 → true → print 1
 ↓
...
 ↓
i = 3 → print 3
 ↓
i++ → 4
 ↓
4 <= 3 → false
 ↓
exit
```

Output:

```text
0123
```

The lecture explicitly uses this style of tracing for GATE-oriented questions.

---

# 18. `while`, `do-while`, `for` — Same Core Idea

The lecture emphasizes that these loops can express essentially the same repetition logic.

For example:

```c
while (condition)
{
    body;
}
```

can conceptually be represented as:

```c
for (; condition; )
{
    body;
}
```

The **main difference is organization of initialization, condition and update**, and whether the condition is checked before or after the body.

---

# 19. `break`

`break` immediately terminates the **nearest enclosing loop or switch**.

```c
while (condition)
{
    if (x == 0)
        break;

    // other code
}
```

Flow:

```text
loop
 ↓
condition to break?
 ↓ yes
break
 ↓
exit loop
```

### Important

`break` does **not** mean:

> Exit all loops.

It exits only the **nearest enclosing loop/switch**.

---

# 20. `break` in Different Loops

### `while`

```c
while (condition)
{
    if (x)
        break;
}
```

### `do-while`

```c
do
{
    if (x)
        break;
}
while (condition);
```

### `for`

```c
for (...)
{
    if (x)
        break;
}
```

The effect is the same:

```text
break
 ↓
leave nearest loop
```

---

# 21. `continue`

`continue` does **not terminate the loop**.

It skips the remaining statements of the **current iteration** and proceeds with the next iteration.

Example:

```c
for (int i = 0; i < 9; i++)
{
    if (i == 4)
        continue;

    printf("%d", i);
}
```

Output:

```text
01235678
```

`4` is skipped.

---

# 22. `continue` — BIG GATE Difference

### In `while`

```c
while (condition)
{
    // code

    if (condition)
        continue;

    // code
}
```

`continue` goes to the **loop condition**.

```text
continue
   ↓
condition
```

### In `do-while`

```text
continue
   ↓
while(condition)
```

### In `for`

```text
continue
   ↓
update expression
   ↓
condition
```

The lecture explicitly compares these three cases.

> [!important] GATE Trap  
> **`continue` in a `for` loop executes the update expression.**
> 
> ```c
> for(i = 0; i < 10; i++)
> {
>     if(i == 5)
>         continue;
> }
> ```
> 
> At `i == 5`:
> 
> `continue → i++ → condition`

---

# 23. Nested Loops

A loop inside another loop:

```c
for (i = 0; i < 5; i++)
{
    for (j = 1; j <= 10; j++)
    {
        ...
    }
}
```

For every one iteration of the **outer loop**, the inner loop may execute completely.

Mental model:

```text
Outer i = 0
    ↓
Inner loop completes
    ↓
Outer i = 1
    ↓
Inner loop completes
    ↓
...
```

---

# 24. `break` in Nested Loops ⭐

```c
for (i = 0; i < 5; i++)
{
    for (j = 1; j <= 10; j++)
    {
        if (j > 3)
            break;

        printf("*");
    }

    printf("\n");
}
```

The `break` belongs to the **inner loop**, because that is the nearest loop.

```text
outer loop
   │
   └── inner loop
          │
        break
          ↓
     exit INNER loop
          ↓
     outer continues
```

The lecture demonstrates this exact nested-loop behavior.

> [!important] GATE  
> `break` exits **only the nearest enclosing loop**.

---

# 25. `while(1)` + `break`

A common pattern:

```c
while (1)
{
    scanf("%d", &x);

    if (x == 0)
        break;
}
```

Flow:

```text
while(1)
   ↓
always true
   ↓
execute body
   ↓
x == 0 ?
  ↓ yes
 break
   ↓
exit
```

So `while(1)` itself is infinite; `break` provides the termination mechanism. The lecture demonstrates this pattern.

---

# 26. `break` vs `continue`

||`break`|`continue`|
|---|---|---|
|Current iteration|Ends|Skips remaining body|
|Loop terminates?|**Yes**|**No**|
|Next action|Exit loop|Next iteration|
|In `for`|Exits loop|Goes to `update`|
|In nested loops|Exits nearest loop|Affects nearest loop|

### Mental model

```text
break:
body → EXIT LOOP

continue:
body → SKIP REST → NEXT ITERATION
```

---

# 27. GATE Pattern Recognition

When solving output questions, don't just execute the code randomly.

### `if-else`

Ask:

```text
Which condition becomes true?
Which else does it belong to?
```

### `switch`

Ask:

```text
What is switch expression?
Which case is selected?
Is there a break?
If no break → fall-through.
```

### `while`

Ask:

```text
Check condition FIRST → execute body → repeat
```

### `do-while`

Ask:

```text
Execute body FIRST → check condition → repeat
```

### `for`

Always trace:

```text
init → condition → body → update → condition → ...
```

### `break`

```text
Nearest loop/switch → EXIT
```

### `continue`

```text
Skip remaining body
       ↓
for    → update → condition
while  → condition
do-while → condition
```

---

# Quick Revision

> [!summary] L-3 — One-Minute Revision

```text
if
→ execute body if condition is non-zero

if-else
→ exactly one branch executes

0
→ false

non-zero
→ true

Nested if
→ inner condition checked only if outer permits

Dangling else
→ else matches nearest unmatched if

switch
→ expression must produce integer value

case
→ unique constant value

break in switch
→ prevents fall-through

No break
→ execution falls through subsequent cases

while
→ condition → body
→ may execute 0 times

do-while
→ body → condition
→ executes at least once

for
→ initialization → condition → body → update

for(;;)
→ infinite loop

break
→ exits nearest loop/switch

continue
→ skips current iteration

continue in for
→ update → condition

continue in while
→ condition

continue in do-while
→ condition

Nested loop
→ inner loop runs for each outer iteration

break in nested loop
→ exits only nearest loop
```

### ⭐ Highest-priority GATE traps

1. **`0` = false; any non-zero = true.**
    
2. **Without braces, only the immediate next statement belongs to `if`.**
    
3. **`else` attaches to nearest unmatched `if`.**
    
4. **`switch` does not re-evaluate cases after entering.**
    
5. **No `break` → fall-through.**
    
6. **`do-while` executes at least once.**
    
7. **`for(;;)` is valid and infinite.**
    
8. **`break` exits nearest loop/switch.**
    
9. **`continue` does not terminate the loop.**
    
10. **`continue` in `for` executes the update expression.**