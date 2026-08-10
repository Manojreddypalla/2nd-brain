# Linked List — Complete Foundation Notes

## 1. What is a Linked List?

A **linked list** is a collection of nodes where each node stores:

```text
1. Data
2. Address of the next node
```

Example:

```text
HEAD
 │
 ▼
┌────┬──────┐     ┌────┬──────┐     ┌────┬──────┐
│ 4  │   ●──┼────→│ 15 │   ●──┼────→│ 7  │ NULL │
└────┴──────┘     └────┴──────┘     └────┴──────┘
 data   next       data   next        data   next
```

The final node's `next` is `NULL`, meaning:

> There is no next node.

---

# 2. Creating a Node in C

We can define a node using a structure:

```c
struct node {
    int data;
    struct node *next;
};
```

Mentally visualize it as:

```text
struct node

┌─────────────────┐
│ data            │
├─────────────────┤
│ next            │
└─────────────────┘
```

`data` stores the actual value.

`next` stores an **address**.

---

# 3. Why is `next` a Pointer?

Look at:

```c
struct node *next;
```

We've already seen the same pattern with:

```c
int *p;
```

which means:

> `p` is a pointer to an integer.

Therefore:

```c
struct node *next;
```

means:

> `next` is a pointer to a `struct node`.

So:

```text
int *p
     ↓
pointer to int


struct node *next
             ↓
pointer to struct node
```

This is what allows one node to point to another node.

```text
NODE 1                         NODE 2

┌────────────┐                ┌────────────┐
│ data = 10  │                │ data = 20  │
├────────────┤                ├────────────┤
│ next ──────┼───────────────→│ next       │
└────────────┘                └────────────┘
```

---

# 4. `struct node x` vs `struct node *p`

This distinction is **extremely important**.

### Actual structure variable

```c
struct node x;
```

`x` itself contains the complete node.

```text
x

┌────────────────┐
│ data           │
├────────────────┤
│ next           │
└────────────────┘
```

Memory exists for both members.

---

### Pointer variable

```c
struct node *p;
```

`p` does NOT contain a node.

It only contains an **address of a node**.

```text
p

┌──────────────┐
│   address    │
└──────┬───────┘
       │
       ▼

       struct node
      ┌─────────────┐
      │ data        │
      ├─────────────┤
      │ next        │
      └─────────────┘
```

So remember:

```text
struct node x
      ↓
actual NODE


struct node *p
      ↓
pointer/address of NODE
```

---

# 5. Connecting a Pointer to a Node

Suppose:

```c
struct node x;
struct node *p;

p = &x;
```

Assume `x` lives at address `1000`.

Then:

```text
p
┌──────────┐
│   1000   │
└────┬─────┘
     │
     ▼
1000
┌─────────────┐
│ x.data      │
├─────────────┤
│ x.next      │
└─────────────┘
```

Therefore:

```text
p  → address of x
*p → x itself
```

---

# 6. Stack vs Heap

This becomes important when creating linked lists.

If inside a function you write:

```c
struct node x;
```

a local `x` normally gets automatic storage associated with the function call.

Conceptually:

```text
STACK

┌─────────────┐
│ x           │
│ ┌─────────┐ │
│ │ data    │ │
│ │ next    │ │
│ └─────────┘ │
└─────────────┘
```

But linked-list nodes are commonly created dynamically using:

```c
malloc()
```

That allocated memory comes from the **heap**.

---

# 7. Understanding `malloc`

Start with something simple:

```c
int *p = malloc(sizeof(int));
```

Suppose:

```c
sizeof(int) = 4
```

Then:

```c
malloc(sizeof(int))
```

essentially requests:

```text
Give me 4 bytes from the heap.
```

Suppose malloc finds memory starting at address `5000`.

```text
STACK                         HEAP

p
┌──────────┐                 5000
│   5000   │───────────────→┌────────────┐
└──────────┘                 │  4 bytes   │
                             └────────────┘
```

`malloc()` returns the starting address.

That address is stored in `p`.

---

# 8. Understanding `p` vs `*p`

If:

```c
int *p;
```

then:

```text
p  → pointer to int
*p → the int pointed to by p
```

Example:

```c
*p = 10;
```

Now:

```text
p
│
▼
┌─────────────┐
│     10      │
└─────────────┘
```

So:

```text
p     = address
*p    = value at that address
```

---

# 9. Understanding `sizeof(*p)`

This was the part we discussed.

If:

```c
int *p;
```

then:

```text
p   → int *
*p  → int
```

Therefore:

```c
sizeof(*p)
```

means:

```c
sizeof(int)
```

So:

```c
int *p = malloc(sizeof(int));
```

and:

```c
int *p = malloc(sizeof(*p));
```

allocate the same amount.

---

# 10. The Same Idea With Nodes

Suppose:

```c
struct node *p;
```

Then:

```text
p   → struct node *
*p  → struct node
```

Therefore:

```c
sizeof(*p)
```

means the same size as:

```c
sizeof(struct node)
```

Hence:

```c
struct node *p = malloc(sizeof(struct node));
```

can also be written:

```c
struct node *p = malloc(sizeof(*p));
```

Both allocate enough memory for **one node**.

---

# 11. Why NOT `sizeof(struct node *)`?

This was your earlier question.

These are completely different:

```c
sizeof(struct node)
```

asks:

> How big is an entire node?

While:

```c
sizeof(struct node *)
```

asks:

> How big is a pointer/address?

For example:

```text
sizeof(struct node)      → perhaps 16 bytes

sizeof(struct node *)    → perhaps 8 bytes
```

So this:

```c
malloc(sizeof(struct node))
```

creates space for:

```text
┌──────────────┐
│ data         │
├──────────────┤
│ next         │
└──────────────┘
```

While:

```c
malloc(sizeof(struct node *))
```

only asks for enough memory for:

```text
┌──────────────┐
│   address    │
└──────────────┘
```

That's not enough to store the complete node in general.

---

# 12. Golden `sizeof` Rule

Suppose:

```c
struct node *p;
```

Then:

```text
sizeof(p)
       ↓
size of POINTER


sizeof(*p)
       ↓
size of NODE
```

Likewise:

```text
sizeof(struct node *)
       ↓
size of POINTER


sizeof(struct node)
       ↓
size of NODE
```

So:

```text
sizeof(p)              ≈ sizeof(struct node *)

sizeof(*p)             = sizeof(struct node)
```

This is the distinction you want burned into your mental model.

---

# 13. Creating a Node Dynamically

Now:

```c
struct node *p = malloc(sizeof(*p));
```

Step by step:

```text
1. p's type = struct node *

2. *p's type = struct node

3. sizeof(*p)
        ↓
   size of struct node

4. malloc(size)
        ↓
   allocates node-sized memory on heap

5. malloc returns address

6. address gets stored in p
```

Result:

```text
STACK                         HEAP

p
┌────────────┐
│ address ───┼──────────────→┌─────────────┐
└────────────┘               │ data        │
                             ├─────────────┤
                             │ next        │
                             └─────────────┘
```

---

# 14. Accessing Structure Members

Suppose:

```c
struct node x;
```

Since `x` is an **actual structure**, use:

```c
x.data
x.next
```

The operator is:

```text
.
```

---

If:

```c
struct node *p;
```

and `p` points to a node, use:

```c
p->data
p->next
```

The operator is:

```text
->
```

So:

```text
Actual struct variable

x.data
x.next


Pointer to struct

p->data
p->next
```

---

# 15. Why `p->data` Works

Suppose:

```c
struct node x;
struct node *p = &x;
```

Then:

```text
p
│
▼
x
┌─────────────┐
│ data = 10   │
├─────────────┤
│ next        │
└─────────────┘
```

Since:

```text
*p = x
```

technically we could write:

```c
(*p).data
```

But C gives us a convenient shortcut:

```c
p->data
```

Therefore:

```c
p->data
```

is equivalent to:

```c
(*p).data
```

Remember the parentheses:

```c
(*p).data
```

not:

```c
*p.data
```

---

# 16. Creating Two Nodes

Suppose:

```c
struct node *p = malloc(sizeof(*p));
struct node *q = malloc(sizeof(*q));
```

Now:

```text
p                           q
│                           │
▼                           ▼

┌─────────────┐            ┌─────────────┐
│ data        │            │ data        │
├─────────────┤            ├─────────────┤
│ next        │            │ next        │
└─────────────┘            └─────────────┘
```

Set their values:

```c
p->data = 10;
q->data = 20;
```

```text
p                           q
│                           │
▼                           ▼

┌─────────────┐            ┌─────────────┐
│ data = 10   │            │ data = 20   │
├─────────────┤            ├─────────────┤
│ next        │            │ next        │
└─────────────┘            └─────────────┘
```

---

# 17. Linking Two Nodes

Now:

```c
p->next = q;
```

What is `q`?

`q` contains the **address of the second node**.

And what does `p->next` store?

An **address of another node**.

Therefore we can store `q` inside `p->next`.

```text
p
│
▼
┌──────────────┐
│ data = 10    │
├──────────────┤
│ next ────────┼───────┐
└──────────────┘       │
                       ▼
                 ┌──────────────┐
                 │ data = 20    │
                 ├──────────────┤
                 │ next         │
                 └──────────────┘
                       ▲
                       │
                       q
```

Then:

```c
q->next = NULL;
```

gives:

```text
p
│
▼
┌──────────┬───┐     ┌──────────┬──────┐
│   10     │ ●─┼────→│    20    │ NULL │
└──────────┴───┘     └──────────┴──────┘
```

That's a linked list.

---

# 18. What is `head`?

Usually we keep a pointer to the first node:

```c
struct node *head;
```

Example:

```text
head
 │
 ▼
┌────┬─────┐    ┌────┬─────┐    ┌────┬──────┐
│ 4  │  ●──┼───→│ 15 │  ●──┼───→│ 7  │ NULL │
└────┴─────┘    └────┴─────┘    └────┴──────┘
```

`head` stores the address of the first node.

---

# 19. Understanding `head->next`

Suppose:

```text
head
 │
 ▼
┌────┬─────┐     ┌────┬─────┐
│ 4  │2000 │────→│ 15 │3000 │
└────┴─────┘     └────┴─────┘
1000              2000
```

Then:

```c
head->data
```

is:

```text
4
```

While:

```c
head->next
```

is:

```text
2000
```

because `next` contains the address of the second node.

---

# 20. Understanding `head->next->data`

Start at:

```text
head
 ↓

NODE 1               NODE 2
┌────┬─────┐         ┌────┬─────┐
│ 4  │  ●──┼────────→│ 15 │  ●  │
└────┴─────┘         └────┴─────┘
```

First:

```c
head->next
```

moves conceptually to node 2.

Then:

```c
head->next->data
```

accesses its data.

Result:

```text
15
```

Mental execution:

```text
head
 ↓
NODE 1

head->next
       ↓
     NODE 2

head->next->data
             ↓
             15
```

---

# 21. Accessing Deeper Nodes

Suppose:

```text
head
 ↓
[4] → [15] → [7] → [14] → NULL
```

Then:

```c
head->data
```

gives:

```text
4
```

```c
head->next->data
```

gives:

```text
15
```

```c
head->next->next->data
```

gives:

```text
7
```

```c
head->next->next->next->data
```

gives:

```text
14
```

Every:

```c
->next
```

means conceptually:

> **Move to the next node.**

That's a fantastic mental model.

---

# 22. Pointer Assignment

This is another major part of the lecture.

Suppose:

```text
p                         q
│                         │
▼                         ▼

[10]                     [20]
1000                     2000
```

Therefore:

```text
p = 1000
q = 2000
```

Now execute:

```c
p = q;
```

This does NOT copy the node.

It copies the **address**.

Before:

```text
p → [10]

q → [20]
```

After:

```text
        ┌──── p
        │
        ▼
       [20]
        ▲
        │
        └──── q
```

Both now point to the same object.

So whenever you see:

```c
p = q;
```

think:

> **Make `p` point wherever `q` points.**

---

# 23. The Most Important Linked-List Operation

Suppose:

```text
p
│
▼
[10] → [20] → [30]
```

Now:

```c
p = p->next;
```

What happens?

`p->next` contains the address of `[20]`.

That address is copied into `p`.

So:

```text
BEFORE

p
↓
[10] → [20] → [30]


AFTER

       p
       ↓
[10] → [20] → [30]
```

`p` moved forward one node.

This operation will appear **constantly**:

```c
p = p->next;
```

Mental translation:

> Move `p` to the next node.

---

# 24. Traversing a Linked List

Now that gives us traversal.

Suppose:

```text
head
 ↓
[4] → [15] → [7] → [14] → NULL
```

Start:

```c
struct node *p = head;
```

So:

```text
p
↓
[4] → [15] → [7] → [14] → NULL
```

Process the current node:

```c
printf("%d", p->data);
```

Then move:

```c
p = p->next;
```

Now:

```text
      p
      ↓
[4] → [15] → [7] → [14] → NULL
```

Again:

```c
p = p->next;
```

```text
             p
             ↓
[4] → [15] → [7] → [14] → NULL
```

Eventually:

```text
p = NULL
```

So the standard traversal pattern is:

```c
struct node *p = head;

while (p != NULL) {
    printf("%d ", p->data);
    p = p->next;
}
```

---

# 25. Inserting a Node Between Two Nodes

The lecture also demonstrates this pointer-rewiring idea.

Suppose initially:

```text
p
↓
[4] ─────→ [15] → [7]
```

We create:

```c
struct node *q = malloc(sizeof(*q));
```

Set:

```c
q->data = 6;
```

Currently:

```text
p
↓
[4] ─────→ [15] → [7]


q
↓
[6]
```

We want:

```text
[4] → [6] → [15] → [7]
```

First:

```c
q->next = p->next;
```

Think:

> Make `q->next` point wherever `p->next` currently points.

Result:

```text
[4] ─────────→ [15]
                 ▲
                 │
[6] ─────────────┘
```

Then:

```c
p->next = q;
```

Result:

```text
p
↓
[4] → [6] → [15] → [7]
       ↑
       q
```

This is the core idea behind linked-list insertion:

> **Change addresses, not move elements.**

---

# 26. Why Linked Lists Are Different From Arrays

An array is conceptually:

```text
[10][20][30][40]
 ↑   ↑   ↑   ↑
continuous memory
```

The next element's location comes from its position in contiguous storage.

A linked list:

```text
[10|●] ─────→ [20|●] ─────→ [30|NULL]
 1000           7820           3200
```

Nodes can live at completely different addresses.

The `next` pointer tells us where to go.

So linked-list traversal is fundamentally:

```text
Current Node
     ↓
read next address
     ↓
follow address
     ↓
Next Node
```

---

# 27. The Big Mental Model

Don't think of:

```c
struct node {
    int data;
    struct node *next;
};
```

as scary C syntax.

Think:

```text
NODE
┌────────────────────┐
│ VALUE              │
│                    │
│ WHERE DO I GO NEXT?│
└────────────────────┘
```

And:

```c
struct node *p;
```

means:

```text
"Where is a node?"
```

And:

```c
p->data
```

means:

```text
"Go to that node and get its data."
```

And:

```c
p->next
```

means:

```text
"Go to that node and get the address
of the next node."
```

And:

```c
p = p->next;
```

means:

```text
"Move to the next node."
```

That's basically the foundation of linked lists.

---

# Quick Revision Sheet

```text
struct node x
→ x IS a node

struct node *p
→ p POINTS TO a node

p
→ address of node

*p
→ actual node

sizeof(p)
→ size of pointer

sizeof(*p)
→ size of node

sizeof(struct node *)
→ size of pointer

sizeof(struct node)
→ size of node

malloc(sizeof(*p))
→ allocate enough heap memory for one node

x.data
→ access data using actual struct

p->data
→ access data using pointer

p->next
→ address of next node

p = q
→ p now points wherever q points

p = p->next
→ move p one node forward

p->next = q
→ make current node point to q

NULL
→ points to nothing
```

The **four lines I'd make sure you fully understand before moving on** are:

```c
struct node *p = malloc(sizeof(*p));

p->data = 10;

p->next = q;

p = p->next;
```

Those four ideas become the machinery behind **linked-list traversal, insertion, deletion, reversal, stacks/queues using linked lists, and eventually the pointer thinking you'll reuse in trees**.