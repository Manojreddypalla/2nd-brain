# C Strings — Pages 49–66

## 1. Does C have a `string` data type?

**No.**

C has:

```c
char
```

but no built-in:

```c
string
```

Instead, a string is represented using a **character array** ending with a **null character `'\0'`**.

```c
char c[] = "Hello";
```

Memory:

```text
'H' 'e' 'l' 'l' 'o' '\0'
```

---

# 2. Character Array vs String

This:

```c
char c[5] = {'H','e','l','l','o'};
```

is a **character array**, but not a valid C string because there is no `'\0'`.

A string needs:

```text
characters + '\0'
```

Example:

```c
char c[6] = {'H','e','l','l','o','\0'};
```

or simply:

```c
char c[] = "Hello";
```

---

# 3. String Literal

```c
char c[] = "Hello";
```

The compiler stores:

```text
H e l l o \0
```

So:

```text
"Hello"
```

occupies **6 characters/bytes**, including the terminating `'\0'`.

---

# 4. Printing a String

Use `%s`:

```c
char c[] = "Hello";

printf("%s", c);
```

Output:

```text
Hello
```

`%s` starts from the address supplied and keeps printing characters until it encounters:

```c
'\0'
```

### Mental model

```text
printf("%s", c)
       ↓
start at c
       ↓
print characters
       ↓
stop at '\0'
```

---

# 5. `%c` vs `%s`

### `%c`

Prints **one character**:

```c
printf("%c", c[0]);
```

Output:

```text
H
```

### `%s`

Prints a **string**:

```c
printf("%s", c);
```

Output:

```text
Hello
```

---

# 6. String Length

The lecture builds a custom `strlen`-like function.

Idea:

```c
int my_strlen(char *c)
{
    int i = 0;

    while(c[i] != '\0')
        i++;

    return i;
}
```

For:

```c
char c[] = "Hello";
```

the loop counts:

```text
H → 1
e → 2
l → 3
l → 4
o → 5
\0 → STOP
```

Therefore:

```text
strlen("Hello") = 5
```

**Important:** `'\0'` is **not counted** in string length.

---

# 7. Why does `strlen` need `'\0'`?

Because a C string doesn't separately store its length.

It knows where the string ends because of:

```text
'\0'
```

So:

```text
String:
H e l l o \0
          ↑
       boundary
```

The function keeps traversing until that boundary.

---

# 8. String Literal with Pointer

You can also write:

```c
char *c = "Hello";
```

Here:

```text
c
↓
'H' 'e' 'l' 'l' 'o' '\0'
```

The pointer `c` points to the beginning of the string literal.

---

# 9. `char c[]` vs `char *c`

This distinction is **very important**.

### Array

```c
char c[] = "Hello";
```

The characters are stored in the array.

```text
c → [H][e][l][l][o][\0]
```

### Pointer

```c
char *c = "Hello";
```

`c` points to the string literal.

```text
c ─────→ "Hello"
```

---

# 10. Modifying a Character Array

This is allowed:

```c
char c[] = "Hello";

c[0] = 'g';
```

Now:

```text
gello
```

The array's elements can be modified.

---

# 11. Modifying a String Literal ⚠️

The lecture shows:

```c
char *t = "Hello";

t[0] = 'g';
```

You **cannot modify the string literal** this way.

The lecture treats the string literal as constant/read-only storage.

### Compare

```c
char c[] = "Hello";
c[0] = 'g';       // ✅

char *t = "Hello";
t[0] = 'g';       // ❌
```

### GATE memory hook

```text
char c[] = "Hello"
→ character array
→ can modify elements

char *c = "Hello"
→ points to string literal
→ don't modify literal
```

---

# 12. Printing Individual Characters

Given:

```c
char c[10] = "hello";
```

You can access:

```c
c[0] → 'h'
c[1] → 'e'
c[2] → 'l'
c[3] → 'l'
c[4] → 'o'
c[5] → '\0'
```

So:

```c
printf("%c", c[i]);
```

prints one character at a time.

---

# 13. Strings Are Basically `char` Arrays

This is the big connection to the previous topic:

```c
char str[] = "hello";
```

is fundamentally:

```text
1D array of char
```

with a special ending:

```text
'\0'
```

So the concepts you just learned about **1D arrays + pointers** directly apply to strings.

---

# 14. String Traversal

Typical pattern:

```c
int i = 0;

while(c[i] != '\0')
{
    // process c[i]
    i++;
}
```

Think:

```text
c
↓
[h][e][l][l][o][\0]
 ↑
 i = 0

then:

[h][e][l][l][o][\0]
    ↑
    i = 1

...

stop when c[i] == '\0'
```

This is the basic pattern behind `strlen`.

---

# 15. String Comparison

Pages 65–66 introduce comparing:

```text
s1 = "abc"
s2 = "abd"
```

The idea shown is based on **lexicographic ordering**—essentially dictionary order.

Compare character by character:

```text
a = a
b = b
c < d
```

Therefore:

```text
"abc" < "abd"
```

So:

```text
s1 < s2 → true
s2 > s1 → true
```

---

# 16. Don't Confuse String Comparison with `==`

This is a **major GATE trap**.

If:

```c
char s1[] = "abc";
char s2[] = "abc";
```

then:

```c
s1 == s2
```

does **not** mean "do the strings contain the same characters?"

Arrays decay to pointers in this context, so you're dealing with **addresses**, not character-by-character string equality.

For actual string-content comparison, C uses a library function such as:

```c
strcmp(s1, s2);
```

The provided pages introduce comparison conceptually but **do not yet develop `strcmp` in detail**, so keep that distinction in mind without going beyond this lecture section.

---

# 🔥 GATE Quick Notes

```text
C has NO built-in string data type.

C string
    ↓
char array
    ↓
must end with '\0'
```

### Example

```c
char s[] = "Hello";
```

Memory:

```text
[H][e][l][l][o][\0]
```

### `%c`

```c
printf("%c", s[0]);
```

→ one character

### `%s`

```c
printf("%s", s);
```

→ characters until `'\0'`

### Length

```text
strlen("Hello") = 5
```

`'\0'` **not counted**.

### Modifying

```c
char s[] = "Hello";
s[0] = 'g';       // ✅
```

```c
char *s = "Hello";
s[0] = 'g';       // ❌ don't modify string literal
```

### String comparison

```text
"abc" vs "abd"
→ compare character-by-character
→ first different character determines ordering
```

---

## 🧠 The whole section in one picture

```text
                 C STRING
                    │
                    ↓
              char array
                    │
             ends with '\0'
                    │
       ┌────────────┼────────────┐
       ↓            ↓            ↓
     %c            %s         strlen
  one char      whole       count until
                string          '\0'
                    │
                    ↓
             Pointer concepts
                    │
                    ↓
         char * can point to it
```

**Most important thing to carry forward:**

> **A C string is not a new data type. It's a `char` array whose end is marked by `'\0'`.**