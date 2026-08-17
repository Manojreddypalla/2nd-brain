Absolutely. This PDF is basically a **complete C Structures + Structure Pointers + Pointer/Precedence + Memory Alignment** lecture, with several GATE-style pointer questions. I’ll turn all 70 pages into **full Obsidian-ready notes**, keeping the lecture’s scope and adding the important GATE traps.

The notes below are based on the uploaded `structures.pdf`, including the handwritten diagrams/examples throughout the pages.

# Structures in C — Complete GATE Notes

## 1. What is a Structure?

A **structure (`struct`)** is a user-defined data type used to group logically related data items, possibly of **different data types**.

### Why structures?

Suppose we want to represent a student:

```c
char name[50];
int roll;
int marks;
```

Using separate variables works, but these variables logically represent **one entity: a student**.

A structure allows us to group them:

```c
struct student {
    char name[50];
    int roll;
    int marks;
};
```

### Mental model

Think of a structure as a **box containing multiple variables**.

```text
student
┌──────────────┐
│ name         │
├──────────────┤
│ roll         │
├──────────────┤
│ marks        │
└──────────────┘
```

The individual variables inside a structure are called **members**.

### Examples

```text
Student → name, roll, marks
Book    → title, author, pages, price
Box     → width, length, height
Circle  → radius
```

---

# 2. Structure Declaration

General syntax:

```c
struct structure_name {
    data_type member1;
    data_type member2;
    ...
};
```

Example:

```c
struct Box {
    int width;
    int length;
    int height;
};
```

Important:

```c
};
```

The semicolon after the closing `}` is required.

---

# 3. Structure Variable

Declaring a structure does **not** automatically create a structure variable.

Example:

```c
struct Box {
    int width;
    int length;
    int height;
};
```

This defines the structure type.

Now:

```c
struct Box b;
```

creates a variable `b`.

Memory concept:

```text
b
┌──────────┐
│ width    │
├──────────┤
│ length   │
├──────────┤
│ height   │
└──────────┘
```

---

# 4. Declaring Multiple Structure Variables

You can declare multiple variables together:

```c
struct book_bank {
    char title[20];
    char author[15];
    int pages;
    float price;
};

struct book_bank book1, book2, book3;
```

All three variables have the same structure type.

```text
book1 → title, author, pages, price
book2 → title, author, pages, price
book3 → title, author, pages, price
```

---

# 5. Declaring Variables Along With Structure Definition

You can also declare variables immediately:

```c
struct employee {
    int id;
    char name[50];
    float salary;
} e1, e2;
```

Here:

```text
struct employee
        ↓
   structure type

e1, e2
   ↓
structure variables
```

### Difference

Method 1:

```c
struct employee {
    int id;
    char name[50];
    float salary;
};

struct employee e1, e2;
```

Method 2:

```c
struct employee {
    int id;
    char name[50];
    float salary;
} e1, e2;
```

Both are equivalent.

---

# 6. `typedef`

`typedef` provides an **alternative name (alias) for a type**.

Syntax:

```c
typedef existing_type new_name;
```

Example:

```c
typedef int dollars;

dollars d;
```

Here:

```text
dollars
   ↓
 int
```

Therefore:

```c
dollars d;
```

is equivalent to:

```c
int d;
```

---

# 7. typedef with Structures

Without `typedef`:

```c
struct bk {
    char title[50];
    char author[50];
    char subject[100];
    int book_id;
};

struct bk Book1, Book2;
```

With `typedef`:

```c
typedef struct bk {
    char title[50];
    char author[50];
    char subject[100];
    int book_id;
} Books;
```

Now:

```c
Books Book1, Book2;
```

is equivalent to:

```c
struct bk Book1, Book2;
```

### Important mental model

```text
struct bk
    ↓
actual structure type

Books
    ↓
alias/name for struct bk
```

`typedef` **does not create a new data structure**.

It simply creates another name for an existing type.

---

# 8. Accessing Structure Members

Use the **dot (`.`) operator**.

Example:

```c
struct rec {
    int a[4];
    long i;
};

struct rec r1;
```

Access:

```c
r1.i
```

Array member:

```c
r1.a[0]
r1.a[1]
r1.a[2]
```

### General form

```c
structure_variable.member
```

Example:

```c
b.price
b.pages
b.name
```

---

# 9. Structure Initialization

Example:

```c
struct book {
    char name;
    float price;
    int pages;
};

struct book b1 = {'B', 130.00, 550};
```

Members receive values in declaration order:

```text
b1.name  = 'B'
b1.price = 130.00
b1.pages = 550
```

Access:

```c
printf("%c", b1.name);
printf("%f", b1.price);
printf("%d", b1.pages);
```

---

# 10. Array of Structures

A structure can be used as an array element.

Example:

```c
struct book {
    char name;
    float price;
    int pages;
};

struct book b[10];
```

Now:

```text
b[0] → name, price, pages
b[1] → name, price, pages
b[2] → name, price, pages
...
b[9] → name, price, pages
```

Access:

```c
b[2].price
```

Meaning:

```text
b[2]
 ↓
third structure
 ↓
price member
```

### Important pattern

```c
array[index].member
```

---

# 11. Input into an Array of Structures

Example:

```c
for(i = 0; i < 10; i++)
{
    scanf("%c %f %d",
          &b[i].name,
          &b[i].price,
          &b[i].pages);
}
```

Each iteration accesses one structure.

```text
i = 0 → b[0]
i = 1 → b[1]
i = 2 → b[2]
...
```

---

# 12. Structure Array Initialization

Example:

```c
struct person {
    char name[20];
    int age;
};

struct person p[3] = {
    {"mike", 29},
    {"heraall", 33},
    {"subramanya", 37}
};
```

Therefore:

```text
p[0].name → "mike"
p[0].age  → 29

p[1].name → "heraall"
p[1].age  → 33

p[2].name → "subramanya"
p[2].age  → 37
```

Access:

```c
printf("%s %d", p[0].name, p[0].age);
```

---

# 13. Pointer as a Structure Member

A pointer can itself be a member of a structure.

Example:

```c
struct test {
    char name[20];
    int *ptr_mem;
};
```

Here:

```text
name      → character array
ptr_mem   → pointer to int
```

Suppose:

```c
struct test t1;
int x = 5;

t1.ptr_mem = &x;
```

Then:

```text
t1.ptr_mem
      ↓
   address of x
      ↓
      x
      5
```

---

# 14. Important: `t1.ptr_mem` vs `*t1.ptr_mem`

This is a classic GATE distinction.

```c
t1.ptr_mem
```

is an **integer pointer**.

It stores an address.

```c
*t1.ptr_mem
```

is an **integer value**.

It accesses the value stored at that address.

Example:

```c
int x = 5;
t1.ptr_mem = &x;
```

Then:

```c
t1.ptr_mem
```

→ address of `x`

while:

```c
*t1.ptr_mem
```

→ `5`

---

# 15. Why `*t1.ptr_mem` Works

Operator precedence matters.

The structure member operator:

```c
.
```

has higher precedence than unary `*`.

Therefore:

```c
*t1.ptr_mem
```

is interpreted as:

```c
*(t1.ptr_mem)
```

NOT:

```c
(*t1).ptr_mem
```

This is extremely important for GATE.

---

# 16. Structure Names Are NOT Pointers

Consider:

```c
struct example {
    int foo;
    int bar;
};

struct example e;
```

`e` is simply a **structure variable**.

It is not automatically a pointer.

```text
e
↓
actual structure object
```

It does not mean:

```text
e → structure
```

unless you explicitly declare a pointer.

---

# 17. Pointer to Structure

To create a pointer to a structure:

```c
struct Box *pBox;
```

Example:

```c
struct Box {
    int width;
    int length;
    int height;
};

struct Box b;
struct Box *pBox;

pBox = &b;
```

Now:

```text
pBox
  ↓
┌───────────┐
│ width     │
│ length    │
│ height    │
└───────────┘
    b
```

`pBox` stores the address of `b`.

---

# 18. Accessing Members Through Structure Pointer

Suppose:

```c
pBox = &b;
```

Then:

```c
(*pBox).width
```

accesses `b.width`.

Similarly:

```c
(*pBox).length
(*pBox).height
```

---

# 19. Arrow Operator `->`

C provides a shorthand for accessing structure members through a pointer.

Instead of:

```c
(*pBox).width
```

write:

```c
pBox->width
```

Similarly:

```c
pBox->length
pBox->height
```

### Fundamental equivalence

```c
p->member
```

is equivalent to:

```c
(*p).member
```

This is one of the most important structure-pointer rules.

---

# 20. Dot vs Arrow

### Structure variable

```c
struct Box b;

b.width;
```

Use:

```text
.
```

### Structure pointer

```c
struct Box *pBox;

pBox->width;
```

Use:

```text
->
```

### Remember

```text
object       → .
pointer      → ->
```

---

# 21. Two Ways of Accessing Structure Members

### Method 1 — Structure variable

```c
b.width
```

### Method 2 — Structure pointer

```c
pBox->width
```

If:

```c
pBox = &b;
```

then:

```c
b.width == pBox->width
```

and:

```c
b.width == (*pBox).width
```

---

# 22. Why `pBox->width` Works

Think of:

```c
pBox->width
```

as:

```c
(*pBox).width
```

Step-by-step:

```text
pBox
 ↓
structure b

*pBox
 ↓
actual structure b

(*pBox).width
 ↓
width member
```

The arrow operator combines the two operations.

---

# 23. Structure Pointer Arithmetic

Consider:

```c
struct stud {
    int roll;
    char dept_code[25];
    float cgpa;
} class[10], *ptr;

ptr = class;
```

Because `class` is an array:

```c
class
```

decays to:

```c
&class[0]
```

Therefore:

```c
ptr = class;
```

means:

```c
ptr = &class[0];
```

---

# 24. Incrementing a Structure Pointer

If:

```c
ptr = class;
```

then:

```c
ptr++;
```

moves the pointer to the **next structure**.

Conceptually:

```text
ptr
 ↓
class[0]

ptr++
 ↓
class[1]
```

The actual address increase is:

```text
sizeof(struct stud)
```

not simply 1 byte.

### General rule

For pointer `p`:

```c
p++
```

moves by:

```text
sizeof(*p)
```

bytes.

So for:

```c
struct stud *ptr;
```

```c
ptr++
```

moves by:

```c
sizeof(struct stud)
```

---

# 25. `++ptr->roll` and Operator Precedence

This is a classic trap.

Expression:

```c
++ptr->roll
```

is interpreted as:

```c
++(ptr->roll)
```

NOT:

```c
(++ptr)->roll
```

Why?

Because `->` has higher precedence than prefix `++`.

So:

```text
ptr->roll
```

is evaluated first.

Then:

```text
++(ptr->roll)
```

increments the member.

---

# 26. High-Precedence Operators

The lecture emphasizes the top part of the C operator precedence table.

### Highest group

```text
()
[]
->
.
postfix ++
postfix --
```

Associativity:

```text
left → right
```

### Next group

```text
sizeof
&
*
+
-
~
!
typecasts
prefix ++
prefix --
```

Associativity:

```text
right → left
```

### Important GATE point

Do not confuse:

```c
*p++
```

with:

```c
(*p)++
```

Because postfix `++` has higher precedence than unary `*`.

Therefore:

```c
*p++
```

means:

```c
*(p++)
```

---

# 27. `(*p).member` vs `*p.member`

Suppose:

```c
struct Box *p;
```

Correct:

```c
(*p).width
```

Because `p` is a pointer.

Incorrect:

```c
*p.width
```

Why?

`.` has higher precedence than `*`.

So:

```c
*p.width
```

means:

```c
*(p.width)
```

which attempts to access `width` from `p` as though `p` were a structure object.

The correct form is:

```c
(*p).width
```

or simply:

```c
p->width
```

---

# 28. Passing Structures to Functions

A structure can be passed to a function **by value**.

Example:

```c
typedef struct {
    char *a;
    char *b;
} t;
```

Then:

```c
void f2(t s)
{
    s.a = "V";
    s.b = "W";
}
```

Calling:

```c
t s = {"A", "B"};

f2(s);
```

passes a **copy of the structure** to `f2`.

Conceptually:

```text
main:

s
┌─────┐
│  A  │
│  B  │
└─────┘

        copy

f2:

s
┌─────┐
│  A  │
│  B  │
└─────┘
```

Changing the local structure variable inside the function does not change the caller's structure variable itself.

---

# 29. Structure Passed by Value — Important Pointer Nuance

Suppose:

```c
struct t {
    char *a;
    char *b;
};
```

and:

```c
t s = {"A", "B"};
```

When `s` is passed by value:

```c
f2(s);
```

the **structure itself is copied**.

But its pointer members are also copied as pointer values.

Therefore:

```text
original:
a → memory containing "A"

copy:
a → SAME memory containing "A"
```

So structure copying is shallow with respect to pointer members.

This distinction becomes very important in GATE questions.

---

# 30. Returning a Structure from a Function

A structure can also be returned by value.

Example concept from the lecture:

```c
struct student {
    char *name;
};

struct student fun(void)
{
    struct student s;

    s.name = "newton";
    printf("%s\n", s.name);

    s.name = "alan";

    return s;
}
```

Then:

```c
struct student m = fun();
```

The returned structure is copied into `m`.

So:

```text
inside fun:
s.name → "alan"

return s
     ↓
m.name → "alan"
```

The important distinction is that `s` inside `fun()` and `m` in `main()` are different structure objects.

---

# 31. Array of Structures + Pointer

Consider:

```c
struct student {
    int x;
};

struct student s[2];
struct student *m;

m = s;
```

Since:

```c
s
```

decays to:

```c
&s[0]
```

we get:

```text
m → s[0]
```

Then:

```c
m++
```

moves to:

```text
s[1]
```

---

# 32. Pointer Member vs Structure Pointer

These are completely different.

### Pointer member

```c
struct test {
    int *p;
};
```

Here the structure **contains a pointer**.

### Structure pointer

```c
struct test *p;
```

Here the pointer points **to a structure**.

### Mental picture

```text
struct test {
    int *p;
};

┌──────────────┐
│ p ───────────┼──→ integer
└──────────────┘
```

versus:

```text
struct test *p;

p ─────────────→ structure
```

---

# 33. Array of Pointers

An array can contain pointers.

Example:

```c
int *x[3];
```

This means:

```text
x = array
each element = pointer to int
```

Diagram:

```text
x
┌─────┐
│ x[0]│ ──→ integer
├─────┤
│ x[1]│ ──→ integer
├─────┤
│ x[2]│ ──→ integer
└─────┘
```

It does **not** mean a pointer to an array.

---

# 34. Pointer to an Array

Compare:

```c
int *x[3];
```

with:

```c
int (*x)[3];
```

### `int *x[3]`

Array of 3 pointers to int.

```text
x
 ↓
[pointer][pointer][pointer]
```

### `int (*x)[3]`

Pointer to an array of 3 integers.

```text
x
 ↓
[ int ][ int ][ int ]
```

### GATE parsing trick

Remember:

```text
[] and () bind before *
```

So:

```c
int *x[3]
```

→ `x` is an array.

But:

```c
int (*x)[3]
```

→ `x` is a pointer.

---

# 35. GATE 2006 — Pointer to Structure Array

The lecture contains a GATE IT 2006 problem involving:

```c
struct test {
    int i;
    char *c;
};
```

and an array:

```c
test[] = {
    {5, "become"},
    {4, "better"},
    {6, "jungle"},
    {8, "ancestor"},
    {7, "brother"}
};
```

with:

```c
struct test *p = st;
```

The key idea is:

```text
p → st[0]
```

Then:

```c
p + 1
```

moves to:

```text
st[1]
```

and:

```c
p++
```

changes the pointer itself.

But:

```c
(*p).i
```

accesses the `i` member of the structure currently pointed to by `p`.

### Pattern to recognize

Whenever you see:

```c
struct X *p = array;
```

immediately visualize:

```text
p → array[0]
```

Then track every `p++`, `++p`, `p--`, etc.

---

# 36. Array of Pointers to Arrays

The lecture's GATE IT 2006 problem also demonstrates:

```c
int a1[] = {...};
int a2[] = {...};
int a3[] = {...};

int *x[] = {a1, a2, a3};
```

Here:

```c
x
```

is an **array of pointers**.

```text
x[0] → a1
x[1] → a2
x[2] → a3
```

So:

```c
x[0][2]
```

means:

```text
x[0]
 ↓
a1
 ↓
third element
```

---

# 37. `x[i][j]` with Array of Pointers

Suppose:

```c
int *x[] = {a1, a2, a3};
```

Then:

```c
x[i][j]
```

is equivalent to:

```c
*(x[i] + j)
```

because:

```c
x[i][j]
```

means:

```c
*(x[i] + j)
```

This is a very useful GATE identity.

---

# 38. Pointer Arithmetic Pattern

Remember these equivalences:

```c
a[i]
```

is:

```c
*(a + i)
```

Similarly:

```c
p[i]
```

is:

```c
*(p + i)
```

Therefore:

```c
i[p]
```

is also:

```c
*(i + p)
```

if the types make the expression valid.

---

# 39. Structure Pointer + Array

For:

```c
struct student s[10];
struct student *p = s;
```

we have:

```text
p       → s[0]
p + 1   → s[1]
p + 2   → s[2]
```

And:

```c
p->x
```

means:

```c
s[0].x
```

while:

```c
(p + 2)->x
```

means:

```c
s[2].x
```

---

# 40. `p++` vs `++p`

### Post-increment

```c
*p++
```

means:

```c
*(p++)
```

Use current pointer, then increment pointer.

### Pre-increment

```c
*++p
```

means:

```c
*(++p)
```

Increment pointer first, then dereference.

### Parenthesized member version

```c
(*p)++
```

increments the **value/member pointed to**.

These three are fundamentally different:

```c
*p++
*++p
(*p)++
```

---

# 41. GATE Pointer Dry-Run Pattern

Suppose:

```c
int a[5] = {1, 6, 9, 5, 2};
int *ptr = a;
```

Then:

```text
ptr → a[0]
      1
```

### `*ptr++`

1. Use current `ptr`
    
2. Dereference it
    
3. Increment pointer
    

Result:

```text
1
```

Then:

```text
ptr → a[1]
```

### `*++ptr`

1. Increment pointer
    
2. Dereference
    

Result:

```text
6
```

### `++*ptr`

Means:

```c
++(*ptr)
```

It increments the value pointed to by `ptr`.

---

# 42. Self-Referential Structures

A structure can contain a pointer to another object of the same structure type.

Typical pattern:

```c
struct node {
    int data;
    struct node *next;
};
```

The lecture shows a structure containing:

```c
struct s1 *p;
```

This is the foundation of:

- linked lists
    
- trees
    
- graphs
    
- dynamic data structures
    

### Why pointer?

This is invalid:

```c
struct node {
    int data;
    struct node next;
};
```

because the compiler would need to know:

```text
size of node
    ↓
contains another node
    ↓
contains another node
    ↓
...
```

Infinite size.

A pointer has fixed size, so:

```c
struct node *next;
```

solves the problem.

---

# 43. Nested Structure

A structure can contain another structure.

Example:

```c
struct address {
    int house;
    int pin;
};

struct student {
    int roll;
    struct address addr;
};
```

Access:

```c
s.addr.house
s.addr.pin
```

Think:

```text
student
├── roll
└── addr
    ├── house
    └── pin
```

---

# 44. Copying One Structure to Another

Structures of the same type can be directly assigned.

Example:

```c
struct PairOfInts {
    int a;
    int b;
};

struct PairOfInts x, y;

x.a = 20;
x.b = 40;

y = x;
```

After:

```c
y = x;
```

we get:

```text
x:          y:
a = 20      a = 20
b = 40      b = 40
```

This is **structure assignment**.

---

# 45. Structure Assignment

For compatible structures:

```c
y = x;
```

copies the structure's member values.

For ordinary scalar members:

```c
int
float
char
```

their values are copied.

For arrays contained directly inside the structure, the array contents are copied as part of the structure assignment.

---

# 46. Important: Array Member vs Pointer Member

This is one of the most important distinctions from pages 62–63.

### Case 1 — Array member

```c
struct {
    char string[32];
    int len;
} a, b;
```

After:

```c
b = a;
```

the contents of `a.string` are copied into `b.string`.

They are separate arrays.

```text
a.string → [hello]
b.string → [hello]

different storage
```

Changing:

```c
b.string[0] = 'y';
```

does not change `a.string`.

---

# 47. Pointer Member — Shallow Copy

Now:

```c
struct {
    char *string;
    int len;
} a, b;
```

Suppose:

```c
a.string = malloc(30);
strcpy(a.string, "hello");

b = a;
```

Now both:

```text
a.string
b.string
```

contain the **same address**.

```text
a.string ─────┐
              ↓
          heap memory
          "hello"
              ↑
b.string ─────┘
```

Therefore:

```c
b.string[2] = 'y';
```

also changes what `a.string` sees.

This is called a **shallow copy**.

---

# 48. Shallow Copy vs Deep Copy

### Shallow copy

Copies pointer values.

```text
A ──→ X
B ──→ X
```

Both point to the same object.

### Deep copy

Creates separate memory.

```text
A ──→ X

B ──→ Y
```

where:

```text
X != Y
```

### GATE trigger

Whenever you see:

```c
struct B = A;
```

and the structure contains pointers:

> **Immediately check whether both pointers now point to the same memory.**

---

# 49. Comparing Structures

C does **not** allow direct comparison of structures using:

```c
if (x == y)
```

For example:

```c
struct name {
    int a;
    char b;
};

struct name x, y;
```

This:

```c
x == y
```

produces a **compile-time error**.

You must compare members individually:

```c
if (x.a == y.a && x.b == y.b)
```

### GATE trap

```text
Structure assignment → allowed
Structure equality using == → not allowed
```

---

# 50. Structure Equality vs Structure Assignment

### Allowed

```c
y = x;
```

### Not allowed

```c
if (x == y)
```

This distinction is highly testable.

---

# 51. Structure Memory Layout

A structure is stored in memory as a sequence of its members, but **padding may be inserted**.

Example:

```c
struct stu_a {
    int i;
    char c;
};
```

Assuming:

```text
int = 4 bytes
char = 1 byte
```

Raw member size:

```text
4 + 1 = 5 bytes
```

But actual structure size may be:

```text
8 bytes
```

because of padding/alignment.

---

# 52. Why Padding Exists

CPUs often access certain data types more efficiently when they start at suitable memory addresses.

For example, if `int` requires alignment to 4-byte boundaries:

```text
address 100 → int
address 104 → int
address 108 → int
```

The compiler may insert unused bytes between members.

These unused bytes are called **padding**.

---

# 53. Structure Alignment

The lecture's alignment model states:

```text
char  → 1-byte alignment
short → 2-byte alignment
int   → 4-byte alignment
long  → 8-byte alignment
```

**Important:** these are the alignment assumptions used in the lecture/examples. Actual C implementation sizes/alignment are platform/compiler dependent.

---

# 54. Padding Before a Member

Rule from the lecture:

> Before each member, padding may be inserted so that the member begins at an address divisible by its required alignment.

Example:

```c
struct stu {
    int i;
    char c;
};
```

If structure starts at address `100`:

```text
100 → int i
104 → char c
105–107 → padding
```

Structure size:

```text
8 bytes
```

The ending padding is important because arrays of structures must keep every element correctly aligned.

---

# 55. Why Padding at the End?

Suppose:

```c
struct A {
    int i;
    char c;
};
```

If size were only:

```text
5 bytes
```

then:

```c
struct A a[2];
```

would place:

```text
a[0] → address 100
a[1] → address 105
```

But `a[1].i` would begin at an address not divisible by 4.

Therefore the compiler rounds the structure size appropriately.

```text
a[0] → 100
a[1] → 108
```

if the structure size is 8.

---

# 56. Structure Size Is Not Necessarily Sum of Members

Never blindly calculate:

```text
sizeof(struct) =
sum(sizeof(all members))
```

because padding may exist.

Instead:

```text
member sizes
+
internal padding
+
tail padding
=
sizeof(struct)
```

---

# 57. Member Ordering Affects Structure Size

Consider:

```c
struct A {
    char c;
    long l;
    char d;
};
```

versus:

```c
struct B {
    long l;
    char c;
    char d;
};
```

They contain the same members but may have different sizes due to alignment.

### GATE strategy

When calculating structure size:

1. Start from address 0.
    
2. Place each member at the next properly aligned address.
    
3. Add padding whenever required.
    
4. At the end, round total size to the structure's required alignment.
    

---

# 58. Example: `int` + `char`

```c
struct A {
    int i;
    char c;
};
```

Assuming:

```text
int alignment = 4
char alignment = 1
```

Layout:

```text
offset
0      i i i i
4      c
5      padding
6      padding
7      padding
```

Total:

```text
8 bytes
```

---

# 59. Example: `long` + `char`

Assuming:

```text
long = 8 bytes
char = 1 byte
long alignment = 8
```

```c
struct B {
    long l;
    char c;
};
```

Layout:

```text
0–7    long
8      char
9–15   padding
```

Total:

```text
16 bytes
```

---

# 60. Example: `long` + `int` + `char`

```c
struct C {
    int i;
    long l;
    char c;
};
```

Possible layout:

```text
offset 0–3    int
offset 4–7    padding
offset 8–15   long
offset 16     char
offset 17–23  padding
```

Total:

```text
24 bytes
```

The exact result follows the assumed alignment rules.

---

# 61. Rearranging Members

Sometimes changing member order reduces padding.

For example:

```c
struct X {
    char c;
    int i;
    long l;
};
```

can have more padding than:

```c
struct Y {
    long l;
    int i;
    char c;
};
```

### General intuition

Put larger-alignment members strategically rather than randomly.

But for GATE, **calculate the actual offsets rather than relying on a memorized "sort by size" rule**.

---

# 62. Lecture Alignment Example

The lecture compares:

```c
struct stu_a {
    int i;
    char c;
};
```

with:

```c
struct stu_b {
    long l;
    char c;
};
```

Under the lecture assumptions:

```text
stu_a → 8 bytes
stu_b → 16 bytes
```

because `long` requires stronger alignment.

---

# 63. More Alignment Examples

The lecture gives structures such as:

```c
struct stu_c {
    int i;
    long l;
    char c;
};
```

and:

```c
struct stu_d {
    long l;
    int i;
    char c;
};
```

Their arrangement and resulting sizes demonstrate that:

> **The order of members matters because padding depends on the current memory offset.**

---

# 64. `double`, `int`, `char`

The lecture also considers:

```c
struct stu_e {
    double d;
    int i;
    char c;
};
```

and:

```c
struct stu_f {
    int i;
    double d;
    char c;
};
```

The placement of `double` can introduce different amounts of padding depending on where it occurs.

### GATE lesson

Never calculate structure size only by:

```text
double + int + char
```

You must calculate the **offset of each member**.

---

# 65. Alignment Algorithm — GATE Method

For each member:

### Step 1

Know:

```text
current offset
```

### Step 2

Know member alignment requirement.

### Step 3

Find the next address satisfying that alignment.

### Step 4

Insert padding if necessary.

### Step 5

Place member.

### Step 6

Continue.

### Step 7

At the end, round total size to the structure's alignment requirement.

---

# 66. Fast Padding Formula

If current offset is `x` and required alignment is `A`:

```text
next aligned offset = smallest multiple of A ≥ x
```

Equivalent:

```text
padding = (A - (x % A)) % A
```

Then:

```text
member starts at x + padding
```

This formula is useful for GATE calculations.

---

# 67. Structure Alignment in Arrays

Suppose:

```c
struct S {
    int a;
    char b;
};
```

and:

```text
sizeof(struct S) = 8
```

Then:

```c
struct S arr[3];
```

will occupy:

```text
arr[0] → offset 0
arr[1] → offset 8
arr[2] → offset 16
```

Therefore:

```c
arr[i]
```

is located at:

```text
base + i * sizeof(struct S)
```

---

# 68. Structure Pointer Arithmetic + `sizeof`

For:

```c
struct S *p;
```

the expression:

```c
p + 1
```

moves by:

```c
sizeof(struct S)
```

bytes.

This connects **structures + pointers + arrays**.

```text
array of structures
        ↓
pointer to first structure
        ↓
pointer arithmetic
        ↓
sizeof(structure)
```

This is a very important GATE connection.

---

# 69. Structure Containing Array

Example from the lecture:

```c
struct S {
    short int a[5];
    long b;
};
```

Assuming:

```text
short = 2 bytes
long = 8 bytes
```

Raw array size:

```text
5 × 2 = 10 bytes
```

Then alignment of `long` may require padding.

For example:

```text
short short short short short
        10 bytes
padding
long
```

So:

```text
structure size ≠ 10 + 8
```

automatically.

You must account for alignment.

---

# 70. Important Pointer/Structure Identities

These should become automatic.

### Structure object

```c
s.x
```

### Structure pointer

```c
p->x
```

### Equivalent form

```c
p->x
```

=

```c
(*p).x
```

### Structure array

```c
s[i].x
```

### Structure-array pointer

```c
(p+i)->x
```

Equivalent:

```c
(*(p+i)).x
```

---

# 71. Operator Precedence Cheat Sheet

For the operators relevant to this lecture:

|Priority|Operators|Associativity|
|---|---|---|
|Highest|`()` `[]` `->` `.` postfix `++` `--`|Left → Right|
|Next|`sizeof` `&` `*` unary `+` `-` `~` `!` cast prefix `++` `--`|Right → Left|

### Consequences

```c
p->x
```

works as expected.

```c
(*p).x
```

needs parentheses.

```c
*p.x
```

is interpreted as:

```c
*(p.x)
```

not:

```c
(*p).x
```

---

# 72. The Three Most Dangerous Expressions

Memorize the **reason**, not the expressions.

### 1.

```c
*p++
```

means:

```c
*(p++)
```

Pointer moves after dereference.

---

### 2.

```c
*++p
```

means:

```c
*(++p)
```

Pointer moves first.

---

### 3.

```c
(*p)++
```

means:

```c
value pointed to by p is incremented
```

So:

```text
*p++   → pointer changes
*++p   → pointer changes
(*p)++ → pointed value changes
```

---

# 73. Structure Pointer + Increment

Suppose:

```c
struct student {
    int x;
};

struct student s[2];

struct student *p = s;
```

Then:

```c
p->x
```

means:

```c
s[0].x
```

After:

```c
p++;
```

now:

```c
p->x
```

means:

```c
s[1].x
```

But:

```c
p->x++
```

increments the **member**, not the pointer.

Because:

```text
-> has higher precedence than postfix ++
```

---

# 74. `++p->x`

Means:

```c
++(p->x)
```

It increments the structure member.

It does **not** increment the pointer.

---

# 75. Function Passing — Big Picture

There are two major ways to pass a structure.

### Pass structure

```c
void f(struct S s)
```

The function receives a copy.

### Pass pointer to structure

```c
void f(struct S *s)
```

The function receives an address.

Then modifications can affect the original structure:

```c
s->x = 10;
```

### Mental model

```text
pass by value:

main object
   ↓
   COPY
   ↓
function object
```

versus:

```text
pass pointer:

main object
   ↑
   |
pointer
   |
function
```

---

# 76. Structure Pointer as Function Parameter

Example:

```c
void modify(struct student *p)
{
    p->roll = 10;
}
```

Call:

```c
struct student s;

modify(&s);
```

Inside:

```c
p->roll
```

refers to:

```c
s.roll
```

So the original object changes.

---

# 77. Structure Assignment with Pointer Members

Suppose:

```c
struct S {
    int x;
    int *p;
};
```

and:

```c
struct S a, b;
```

After:

```c
b = a;
```

we get:

```text
b.x = a.x

b.p = a.p
```

Therefore:

```text
a.p ──→ X
b.p ──→ X
```

The pointer itself is copied, not the object it points to.

---

# 78. Structure Assignment with Array Members

Suppose:

```c
struct S {
    int x;
    char name[20];
};
```

After:

```c
b = a;
```

the contents of `name` are copied.

```text
a.name → own array
b.name → separate array
```

This is fundamentally different from:

```c
char *name;
```

---

# 79. GATE Trap — "Struct Copy Means Deep Copy"

Do **not** use this rule:

> Structure assignment always performs deep copy.

Incorrect.

Correct:

```text
Direct array member
→ array contents copied

Pointer member
→ pointer value copied
→ pointed object NOT copied
```

---

# 80. GATE Trap — `struct` vs `typedef`

These are different concepts.

```c
struct student
```

is the structure type name.

If:

```c
typedef struct student Student;
```

then:

```c
Student
```

is simply an alias.

Therefore:

```c
Student s;
```

means:

```c
struct student s;
```

---

# 81. GATE Trap — Structure Name Is Not Object

Given:

```c
struct student {
    int x;
};
```

`struct student` describes a **type**.

You need:

```c
struct student s;
```

to create an object.

---

# 82. GATE Trap — Dot vs Arrow

```c
s.x
```

when `s` is a structure object.

```c
p->x
```

when `p` is a pointer to structure.

Remember:

```text
object → .
pointer → ->
```

---

# 83. GATE Trap — `->` Is Not Dereferencing Alone

```c
p->x
```

is conceptually:

```c
(*p).x
```

So `->` combines:

```text
dereference pointer
+
access structure member
```

---

# 84. GATE Trap — Pointer Arithmetic Depends on Type

For:

```c
int *p;
```

```c
p + 1
```

moves by:

```text
sizeof(int)
```

For:

```c
struct student *p;
```

```c
p + 1
```

moves by:

```text
sizeof(struct student)
```

For:

```c
char *p;
```

```c
p + 1
```

moves by:

```text
sizeof(char) = 1
```

---

# 85. GATE Trap — `p++` Doesn't Increment the Structure

If:

```c
struct S *p;
```

then:

```c
p++;
```

changes the pointer.

It does **not** increment any structure member.

But:

```c
p->x++
```

increments member `x`.

---

# 86. GATE Trap — `++p->x`

This:

```c
++p->x
```

means:

```c
++(p->x)
```

because:

```text
-> has higher precedence
```

---

# 87. GATE Trap — `*p.x`

If:

```c
struct S *p;
```

then:

```c
*p.x
```

does NOT mean:

```c
(*p).x
```

because `.` has higher precedence.

Correct:

```c
(*p).x
```

or:

```c
p->x
```

---

# 88. GATE Trap — `*p++`

```c
*p++
```

means:

```c
*(p++)
```

not:

```c
(*p)++
```

The latter requires parentheses.

---

# 89. GATE Trap — Structure Comparison

```c
if (a == b)
```

for structures:

```text
NOT allowed
```

But:

```c
b = a;
```

is allowed when types are compatible.

---

# 90. GATE Trap — Structure Size

Never assume:

```text
sizeof(struct) = sum of members
```

because:

```text
padding
+
alignment
```

can increase size.

---

# 91. GATE Trap — Same Members ≠ Same Size

Two structures can contain the same data types but different ordering:

```c
struct A {
    char c;
    int i;
};

struct B {
    int i;
    char c;
};
```

Their sizes can differ depending on alignment rules.

---

# 92. GATE Trap — Pointer Array vs Array Pointer

### Array of pointers

```c
int *p[5];
```

Read:

> `p` is an array of 5 pointers to int.

### Pointer to array

```c
int (*p)[5];
```

Read:

> `p` is a pointer to an array of 5 integers.

The parentheses completely change the meaning.

---

# 93. GATE Trap — `p[i][j]`

For:

```c
int *p[3];
```

```c
p[i][j]
```

means:

```c
*(*(p + i) + j)
```

This is useful when solving pointer-expression questions.

---

# 94. Structure Problem-Solving Framework

When you see a complicated GATE structure/pointer problem, don't try to evaluate the whole expression mentally.

Use this process.

### Step 1 — Identify every object

Example:

```c
struct S s[3];
struct S *p;
```

Draw:

```text
s[0]
s[1]
s[2]

p → ?
```

### Step 2 — Resolve pointer initialization

```c
p = s;
```

means:

```text
p → s[0]
```

### Step 3 — Track pointer movement

```c
p++
```

means:

```text
p → s[1]
```

### Step 4 — Resolve `->`

```c
p->x
```

means:

```c
(*p).x
```

### Step 5 — Apply precedence

For:

```c
*p++
```

first determine:

```c
p++
```

then dereference.

### Step 6 — Only then calculate the value.

This eliminates most pointer-expression mistakes.

---

# 95. Master Mental Model

You can connect almost the entire lecture with this:

```text
STRUCTURE
   │
   ├── groups variables
   │
   ├── can form arrays
   │      │
   │      └── pointer arithmetic
   │
   ├── can contain pointers
   │      │
   │      └── shallow copy issues
   │
   ├── can have pointer to structure
   │      │
   │      └── ->
   │
   ├── can be passed to functions
   │      ├── by value
   │      └── by pointer
   │
   └── occupies memory
          │
          ├── alignment
          └── padding
```

---

# 96. One-Page Syntax Sheet

```c
// Structure declaration
struct Student {
    int roll;
    char name[20];
};

// Structure object
struct Student s;

// Structure initialization
struct Student s = {10, "ABC"};

// Member access
s.roll
s.name

// Array of structures
struct Student s[10];

// Array member access
s[2].roll

// Structure pointer
struct Student *p;

// Pointer assignment
p = &s;

// Structure pointer access
p->roll

// Equivalent
(*p).roll

// typedef
typedef struct Student Student;

// Now
Student s;

// Pointer arithmetic
p++
p + 1

// Array of pointers
int *p[5];

// Pointer to array
int (*p)[5];
```

---

# 97. Must-Know Equivalences

```text
p->x
=
(*p).x
```

```text
a[i]
=
*(a+i)
```

```text
p[i]
=
*(p+i)
```

```text
*p++
=
*(p++)
```

```text
*++p
=
*(++p)
```

```text
(*p)++
=
increment value pointed by p
```

```text
x[i][j]
=
*(*(x+i)+j)
```

For structure arrays:

```text
p = s
→ p = &s[0]
```

and:

```text
p + i
→ &s[i]
```

---

# 98. GATE Question Triggers

When you see these, immediately think of the associated concept:

|Question contains|Think|
|---|---|
|`struct`|Structure object/type|
|`typedef struct`|Type alias|
|`.`|Structure object/member|
|`->`|Structure pointer|
|`(*p).x`|Structure pointer dereference|
|`p++` where `p` is struct pointer|Move by `sizeof(struct)`|
|`p->x++`|Increment member|
|`++p->x`|Increment member|
|`*p++`|Dereference old pointer, then move|
|`*++p`|Move pointer, then dereference|
|`(*p)++`|Increment pointed value|
|`struct S s[10]`|Array of structures|
|`struct S *p = s`|Pointer to first structure|
|`int *p[5]`|Array of pointers|
|`int (*p)[5]`|Pointer to array|
|`b = a`|Structure assignment|
|`a == b`|Structure comparison → invalid|
|Structure contains pointer + assignment|Shallow copy|
|Structure size question|Padding/alignment|
|Different member order|Possible different size|
|`sizeof(struct)`|Include padding|

---

# 99. Final Revision Map

For GATE, I would mentally divide this entire lecture into **5 connected blocks**:

### Block 1 — Structure Basics

```text
struct
members
objects
initialization
typedef
.
```

### Block 2 — Structures + Arrays

```text
array of structures
s[i].member
structure initialization
```

### Block 3 — Structures + Pointers

```text
struct *
&
->
(*p).member
pointer arithmetic
```

### Block 4 — Structures + Functions + Copying

```text
pass by value
pass pointer
return structure
structure assignment
pointer members
shallow copy
```

### Block 5 — GATE Pointer + Memory Problems

```text
precedence
p++
++p
*p++
*++p
(*p)++
array of pointers
pointer to array
structure pointer arithmetic
padding
alignment
sizeof(struct)
```

---

# 🔥 The 10 Things You Absolutely Must Know

If you're revising this before solving questions, make sure these are automatic:

1. **`.` → structure object**
    
2. **`->` → pointer to structure**
    
3. **`p->x == (*p).x`**
    
4. **`p + 1` moves by `sizeof(*p)`**
    
5. **`*p++ == *(p++)`**
    
6. **`*++p == *(++p)`**
    
7. **`(*p)++` increments the pointed value**
    
8. **Structure assignment is allowed, structure `==` is not**
    
9. **Pointer members cause shallow copies**
    
10. **`sizeof(struct)` can be larger than the sum of member sizes because of padding/alignment**
    

The **pages 49–70 are especially important for GATE** because they transition from learning syntax into actual pointer-expression, structure-copying, and memory-alignment questions.

For your current GATE workflow, I would **not reread all 70 pages repeatedly**. Build these notes once, then move to **GATE/PYQ questions** and use the questions to expose which pointer/alignment patterns you haven't internalized yet.