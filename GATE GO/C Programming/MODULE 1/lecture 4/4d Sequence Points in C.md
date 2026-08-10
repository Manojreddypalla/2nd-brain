# 4D — Sequence Points in C

## 1. What is a Sequence Point?

A **sequence point** is a point after which the changes/side effects from previous evaluations are **definitely completed**.

Example:

```c
a = 5;
x = a++;
printf("%d", x);
printf("%d", a);
```

After the `;`:

```text
x = 5
a = 6
```

The lecture identifies the **semicolon** as a sequence point.

---

## 2. Important Sequence Points from Lecture

The lecture lists sequence points associated with:

```text
;
if(...)
for(...)
while(...)
switch(...)
?:
&&
||
```

### Key idea

```text
Before sequence point → change may not yet be completed
After sequence point  → change is definitely completed
```

---

# 3. The BIG GATE Trap

Consider:

```c
x = a++ + a++;
```

Here we are trying to modify `a` **multiple times before reaching a sequence point**.

The lecture marks this as:

```text
UNDEFINED
```

### 🚨 Therefore

Don't try to calculate:

```text
a++ → ?
a++ → ?
```

The result is **undefined**.

---

## 4. Another Example

```c
a++ + a++
```

❌ **Undefined**

But:

```c
a++;
a++;
```

✅ Well-defined because the `;` provides a sequence point between them.

---

# 5. Sequence Point + `if`

Example from lecture:

```c
i = 3;

if (i++)
{
    printf("%d", i);
}
```

`i++` happens in the condition.

After the condition evaluation, the increment has taken place:

```text
Before: i = 3
i++    → condition gets 3
After  → i = 4
```

So inside the body:

```c
printf("%d", i);
```

prints:

```text
4
```

---

# ⭐ 4D — GATE Short Notes

```text
SEQUENCE POINT

A point after which previous side effects
are definitely completed.

Important sequence points:
; 
if()
for()
while()
switch()
?:
&&
||

Example:

a = 5;
x = a++;
→ after ; : a = 6, x = 5


🚨 GATE TRAP:

a++ + a++
→ UNDEFINED

Because a is modified multiple times
before a sequence point.


Remember:

a++;
a++;
→ Defined

a++ + a++;
→ Undefined
```

### 🧠 Pattern to recognize

Whenever you see **multiple `++`, `--`, or assignments involving the same variable in one expression**, **STOP and check sequence points before calculating.**