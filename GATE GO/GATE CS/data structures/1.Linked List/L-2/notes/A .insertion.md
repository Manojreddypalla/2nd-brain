# GATE CSE — Linked List Notes

## Pages 1–12: Traversal + Insertion in Singly Linked List

> **Scope:** Only pages **1–12** of `L-2(4).pdf`. I’m keeping this as **short revision notes + conceptual depth + GATE traps**, not adding topics from later pages.

---

# 1. Length of a Linked List

A linked list:

```text
HEAD
 ↓
[4|•] → [15|•] → [7|•] → [40|NULL]
```

To find the number of nodes:

### Method 1 — Count while current node exists

```c
int length(struct node *head)
{
    int count = 0;

    while (head != NULL)
    {
        count++;
        head = head->next;
    }

    return count;
}
```

### Mental model

`head` is simply a **pointer walking through the list**.

```text
head
 ↓
[4] → [15] → [7] → [40] → NULL
```

Each iteration:

```text
count++
head = head->next
```

Eventually:

```text
head == NULL
```

Stop.

### Complexity

```text
Time  = O(n)
Space = O(1)
```

---

## ⚠️ Important GATE Trap

You may see:

```c
while (head->next != NULL)
```

instead of:

```c
while (head != NULL)
```

These are **not equivalent**.

For:

```text
4 → 15 → 7 → 40 → NULL
```

`head->next != NULL` stops when `head` reaches `40`.

Therefore, if the initial count is `1`, it can still correctly count the nodes.

But:

```c
while (head->next != NULL)
```

**cannot safely handle an empty list**, because `head` itself may be `NULL`.

The PDF explicitly assumes at least one node for the initial length question.

---

# 2. Insertion in Singly Linked List

There are three important insertion positions:

1. Beginning
    
2. End
    
3. Middle / after a given node
    

The central idea is always:

> **Create a new node → adjust pointers → preserve the existing chain.**

---

# 3. Insertion at Beginning

Suppose:

```text
HEAD
 ↓
[15] → [7] → [40] → NULL
```

Want to insert `5`.

### Step 1 — Create node

```text
newNode
   ↓
 [5 | ?]
```

### Step 2 — Make new node point to old head

```c
newNode->next = head;
```

Now:

```text
newNode
 ↓
[5] → [15] → [7] → [40] → NULL
        ↑
       head
```

### Step 3 — Move head

```c
head = newNode;
```

Final:

```text
HEAD
 ↓
[5] → [15] → [7] → [40] → NULL
```

The lecture shows exactly this two-stage pointer change on pages 3–6.

### Code

```c
struct node *newNode = malloc(sizeof(struct node));

newNode->data = 5;
newNode->next = head;

head = newNode;
```

### Complexity

```text
Time  = O(1)
Space = O(1) auxiliary
```

---

# 4. Why the Order Matters

Correct:

```c
newNode->next = head;
head = newNode;
```

Think:

```text
newNode ─────→ old list
       ↓
      head
```

Then move `head`.

### Dangerous idea

```c
head = newNode;
newNode->next = head;
```

Now:

```text
head
 ↓
newNode
  ↑
  └──── newNode
```

You have lost the pointer to the old first node.

### GATE Mental Rule

Whenever modifying linked-list pointers:

> **First preserve the old connection, then change the external pointer.**

---

# 5. Insertion at End

Initial:

```text
HEAD
 ↓
[4] → [15] → [7] → NULL
```

Insert `40`.

### Step 1 — Create new node

```c
endNode->data = 40;
endNode->next = NULL;
```

Why?

Because this node will become the **last node**.

### Step 2 — Traverse to last node

```c
while (head->next != NULL)
{
    head = head->next;
}
```

Eventually:

```text
HEAD(original)
 ↓
[4] → [15] → [7] → NULL
               ↑
              head
```

### Step 3 — Connect new node

```c
head->next = endNode;
```

Final:

```text
[4] → [15] → [7] → [40] → NULL
```

This traversal-and-connection process is shown on pages 7–11.

---

## Important: `head` Is Being Used as a Traversal Pointer

If you do:

```c
while (head->next != NULL)
    head = head->next;
```

you have moved `head`.

So in a real function, if the original head must remain available, use:

```c
struct node *temp = head;

while (temp->next != NULL)
    temp = temp->next;

temp->next = endNode;
```

### Complexity

Without a tail pointer:

```text
Time = O(n)
```

With a maintained `tail` pointer:

```text
Time = O(1)
```

---

# 6. Insertion at Middle

Suppose:

```text
[4] → [15] → [7] → [40] → NULL
```

We want:

```text
[4] → [15] → [5] → [7] → [40]
```

The key problem is:

> **Which two links need to change?**

Before:

```text
15 ─────→ 7
```

After:

```text
15 ─────→ 5 ─────→ 7
```

So:

```c
newNode->next = current->next;
current->next = newNode;
```

---

# 7. Middle Insertion — Pointer Logic

Suppose `current` points to `15`.

```text
current
   ↓
 [15] ───→ [7]
```

Create:

```text
newNode
   ↓
 [5]
```

### First:

```c
newNode->next = current->next;
```

Now:

```text
[15] ───→ [7]
   \
    → [5] ───→ [7]
```

### Then:

```c
current->next = newNode;
```

Final:

```text
[15] ───→ [5] ───→ [7]
```

The lecture's pages 12–13 show this exact pointer rearrangement.

---

# 8. The Most Important Linked-List Pointer Pattern

Memorize the **idea**, not the code:

For insertion between `A` and `B`:

```text
A → B
```

becomes:

```text
A → NEW → B
```

Therefore:

```c
NEW->next = A->next;
A->next = NEW;
```

### Why this order?

Because after:

```c
A->next = NEW;
```

you can no longer reach `B` through `A`.

So:

> **Save the old next pointer before overwriting it.**

This is one of the most important linked-list patterns for GATE.

---

# 9. Insertion at a Given Position / After a Given Node

The lecture describes middle insertion as insertion for a **given node**.

General idea:

```text
previous → next
```

Find the required position first.

Then:

```c
newNode->next = previous->next;
previous->next = newNode;
```

### Traversal condition

The lecture uses traversal logic based on the node's data:

```c
while (head->next->data != 7)
{
    head = head->next;
}
```

Conceptually:

> Move until you reach the node **before the target position**, then perform the two-link insertion.

---

# 10. Linked List Insertion — Complexity Cheat Sheet

|Operation|Without Tail|With Tail|
|---|--:|--:|
|Insert at beginning|**O(1)**|**O(1)**|
|Insert at end|**O(n)**|**O(1)**|
|Insert after known node|**O(1)**|**O(1)**|
|Find a particular node|**O(n)**|**O(n)**|

The important distinction is:

> **Pointer manipulation can be O(1), but finding where to manipulate may be O(n).**

Example:

```text
Insert after node 7
```

If you already have a pointer to node `7`:

```text
O(1)
```

If you only know:

```text
"insert after the node containing 7"
```

you must search:

```text
O(n)
```

---

# 11. GATE Pointer Visualization

Whenever you see linked-list code, don't immediately execute the syntax.

Draw:

```text
pointer → node → node → node → NULL
```

Then ask:

### Question 1

**What does each pointer currently point to?**

### Question 2

**Which `next` field is changing?**

### Question 3

**Will I lose access to any node after this assignment?**

### Question 4

**Does `head` itself change?**

### Question 5

**Am I changing a pointer or changing the node's `next` field?**

Example:

```c
head = head->next;
```

means:

```text
Move the pointer.
```

While:

```c
head->next = newNode;
```

means:

```text
Modify the node's link.
```

**These are fundamentally different.**

---

# 12. One Master Mental Model

Almost every singly linked-list operation can be reduced to:

```text
        DATA
NODE ┌─────────┐
     │ data    │
     │ next ───────→ another node
     └─────────┘
```

You manipulate **arrows**, not the nodes themselves.

### Traversal

```c
p = p->next;
```

```text
p
↓
A → B → C

p = p->next

    p
    ↓
A → B → C
```

---

### Insert

Break:

```text
A → B
```

Create:

```text
N
```

Reconnect:

```text
A → N → B
```

Code:

```c
N->next = A->next;
A->next = N;
```

---

### The Golden Rule 🧠

```text
Before changing a link:
        ↓
Ask: "What old node am I about to lose access to?"
        ↓
Save that connection first.
```

This single habit will make linked-list questions **much easier to trace**.

---

# ⚡ GATE Quick Revision

```text
Traversal:
    p = p->next

Length:
    while(p != NULL)
        count++

Insert beginning:
    new->next = head
    head = new

Insert end:
    move to last node
    last->next = new
    new->next = NULL

Insert middle:
    new->next = prev->next
    prev->next = new
```

### Complexity

```text
Beginning insertion      → O(1)
End insertion            → O(n)
Middle after known node  → O(1)
Middle by searching      → O(n)
Traversal                → O(n)
Length                   → O(n)
```

### 🚨 Most likely GATE traps

- `head = head->next` **moves a pointer**.
    
- `head->next = X` **changes a link inside a node**.
    
- `while(head != NULL)` visits every node.
    
- `while(head->next != NULL)` stops at the last node.
    
- Empty-list handling matters.
    
- In insertion, **preserve the old `next` before overwriting it**.
    
- `head` changes during insertion at beginning.
    
- `head` normally does **not** change when inserting in the middle/end.
    
- Finding a node and inserting after an **already-known node** have different complexities.