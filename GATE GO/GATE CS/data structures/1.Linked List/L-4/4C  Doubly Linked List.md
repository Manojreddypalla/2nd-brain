Yep. Pages **59–74** are the **Doubly Linked List (DLL)** section. I’ll keep these notes grounded in exactly what the lecture covers, including the pointer manipulations and the GATE-style questions.

# Doubly Linked List — Notes

### Pages 59–74

## 1. What is a Doubly Linked List?

A **Doubly Linked List (DLL)** is a linked list in which every node contains:

```c
struct node {
    int value;
    struct node *prev;
    struct node *next;
};
```

So each node knows:

- `prev` → previous node
    
- `next` → next node
    
- `value` → data
    

### Mental model

```text
NULL ← [prev | 3 | next] ↔ [prev | 7 | next] ↔ [prev | 5 | next] → NULL
          ↑
         head
```

Unlike a singly linked list, we can move:

```text
forward  → using next
backward ← using prev
```

The first node has:

```c
head->prev = NULL;
```

and the last node has:

```c
last->next = NULL;
```

This is the key advantage of DLL: **bidirectional traversal**.

---

# 2. Understanding `prev` and `next`

Suppose:

```text
NULL ← [1] ↔ [2] ↔ [3] ↔ [4] → NULL
         ↑              ↑
        head           q
```

For node `2`:

```c
p->value
```

gives `2`.

Its neighbors can be accessed using:

```c
p->prev
p->next
```

For example, if `p` points to `2`:

```c
p->prev->value = 1
p->next->value = 3
```

### Important relationship

For two adjacent nodes:

```text
[2] ↔ [3]
```

the connections are:

```c
node2->next = node3;
node3->prev = node2;
```

So **every connection in DLL has two pointers that must agree with each other**.

The lecture demonstrates this using pointers `p` and `q`; if `p` points to `2` and `q` points to `3`, then `q->prev->value` and `p->next->value` both refer to the same neighboring relationship.

---

# 3. Insertion at the Beginning

Suppose we have:

```text
NULL ← [3] ↔ [7] ↔ [5] → NULL
        ↑
       head
```

We want to insert a new node `X` at the beginning.

Final structure:

```text
NULL ← [X] ↔ [3] ↔ [7] ↔ [5] → NULL
         ↑
        head
```

The new node needs:

```c
newNode->prev = NULL;
newNode->next = head;
```

Then the old first node must point backward to the new node:

```c
head->prev = newNode;
```

Finally:

```c
head = newNode;
```

### Pointer changes

Think in this order:

```text
newNode
   ↓
 NULL ← X ↔ oldHead
          ↑
        oldHead->prev
```

So:

```c
newNode->prev = NULL;
newNode->next = head;
head->prev = newNode;
head = newNode;
```

The lecture's page 61 diagram shows exactly this structural change.

---

# 4. GATE Question — Insert at Beginning

The lecture gives a function:

```c
struct node *insertDing(struct node *head, int val)
```

A new node is created using:

```c
struct node *newNode = malloc(sizeof(struct node));
newNode->value = val;
```

The important question is: **which pointer updates correctly create the new first node?**

Correct logic:

```c
newNode->prev = NULL;
newNode->next = head;
head->prev = newNode;
head = newNode;
```

### Why?

Because after insertion:

```text
NULL ← newNode ↔ oldHead
```

Therefore:

```text
newNode.prev = NULL
newNode.next = oldHead
oldHead.prev = newNode
head = newNode
```

The lecture's question and pointer analysis are on pages 62–63.

---

# 5. Insertion at the End

Suppose:

```text
NULL ← [3] ↔ [7] ↔ [5] → NULL
```

We want:

```text
NULL ← [3] ↔ [7] ↔ [5] ↔ [X] → NULL
```

The new node should have:

```c
newNode->next = NULL;
```

and its previous node should be the old last node:

```c
newNode->prev = last;
```

Then:

```c
last->next = newNode;
```

### Core idea

At the end, we need:

```text
oldLast ↔ newNode → NULL
```

Therefore:

```c
newNode->prev = oldLast;
newNode->next = NULL;
oldLast->next = newNode;
```

The lecture shows the end insertion structure on page 64.

---

# 6. Finding the Last Node

To insert at the end, first reach the last node.

For a normal DLL:

```c
while (head->next != NULL)
    head = head->next;
```

After the loop:

```text
head
 ↓
[LAST]
  ↓
 NULL
```

So now `head` temporarily points to the last node.

Then:

```c
newNode->prev = head;
newNode->next = NULL;
head->next = newNode;
```

### Critical distinction

Do **not** confuse:

```c
head->next != NULL
```

with

```c
head->prev != NULL
```

For moving toward the end, use:

```c
next
```

because:

```text
head → next → next → next → NULL
```

The lecture's end-insertion question tests precisely these pointer conditions.

---

# 7. Deleting the First Node

Suppose:

```text
NULL ← [4] ↔ [15] ↔ [7] ↔ [40] → NULL
        ↑
       head
```

We want to delete `4`.

After deletion:

```text
NULL ← [15] ↔ [7] ↔ [40] → NULL
        ↑
       head
```

The old first node is:

```c
head
```

The new first node is:

```c
head->next
```

So first:

```c
head = head->next;
```

Now make the new first node's `prev` point to `NULL`:

```c
head->prev = NULL;
```

Then free the old node.

### But remember the old address!

If you overwrite `head` first:

```c
head = head->next;
```

you've lost the pointer to the old first node.

So use a temporary pointer:

```c
struct node *temp = head;

head = head->next;
head->prev = NULL;

free(temp);
```

The lecture specifically illustrates this deletion process on page 69.

---

# 8. GATE Question — Delete First Node

The lecture asks which code correctly deletes the first node.

The important pointer sequence is:

```text
temp = old head
       ↓
head = head->next
       ↓
head->prev = NULL
       ↓
free(temp)
```

Why `head->prev = NULL`?

Because after deletion, the new first node must satisfy:

```text
NULL ← [new first]
```

not:

```text
[deleted node] ← [new first]
```

The question and its pointer analysis appear on pages 70–71.

---

# 9. Deleting the Last Node

Suppose:

```text
NULL ← [4] ↔ [15] ↔ [7] ↔ [40] → NULL
                                  ↑
                                last
```

We want to delete `40`.

The new last node becomes:

```text
[7]
```

So:

```c
last = last->prev;
```

Then:

```c
last->next = NULL;
```

Finally:

```c
free(oldLast);
```

### Pointer picture

Before:

```text
[7] ↔ [40] → NULL
```

After:

```text
[7] → NULL
```

Therefore the two essential changes are:

```c
head = head->prev;   // move to previous node
head->next = NULL;   // make it last
```

with the old last node freed.

The lecture introduces this operation on page 72.

---

# 10. GATE Question — Delete Last Node

The lecture's final question asks which implementation correctly deletes the last node.

The correct traversal condition is:

```c
while (head->next != NULL)
    head = head->next;
```

This gets us to the last node.

Then:

```text
oldLast
   ↓
 [40]
  ↑
prev
  |
 [7]
```

Move back:

```c
head = head->prev;
```

Then:

```c
free(head->next);
head->next = NULL;
```

So the fundamental idea is:

```text
Reach last
   ↓
Move to previous
   ↓
Free old last
   ↓
Set new last's next = NULL
```

The answer choices and pointer analysis are shown on page 73.

---

# 11. DLL Pointer Rules — THE IMPORTANT PART

This is the part I'd memorize **conceptually**, not as code.

### Insertion at beginning

```text
NULL ← NEW ↔ OLD
```

Changes:

```c
new->prev = NULL;
new->next = old;
old->prev = new;
head = new;
```

### Insertion at end

```text
OLD ↔ NEW → NULL
```

Changes:

```c
new->prev = old;
new->next = NULL;
old->next = new;
```

### Delete beginning

```text
OLD ↔ NEW
```

becomes:

```text
NULL ← NEW
```

Changes:

```c
head = head->next;
head->prev = NULL;
free(oldHead);
```

### Delete end

```text
OLD ↔ LAST → NULL
```

becomes:

```text
OLD → NULL
```

Changes:

```c
last = last->prev;
free(last->next);
last->next = NULL;
```

---

# 12. The DLL Invariant

This is the **big mental model** for GATE questions.

For every pair of neighboring nodes:

```text
A ↔ B
```

we must have:

```c
A->next == B
B->prev == A
```

And at the boundaries:

```c
head->prev == NULL
last->next == NULL
```

So whenever you modify a DLL, ask:

> **Did I repair BOTH directions?**

For example, inserting `X` between `A` and `B`:

```text
Before:

A ↔ B
```

After:

```text
A ↔ X ↔ B
```

You need **four pointer updates**:

```c
A->next = X;
X->prev = A;

X->next = B;
B->prev = X;
```

That four-pointer pattern is the heart of DLL insertion.

---

# 13. DLL vs SLL

|Feature|SLL|DLL|
|---|---|---|
|Forward traversal|✅|✅|
|Backward traversal|❌|✅|
|Node pointers|`next`|`prev + next`|
|Memory per node|Lower|Higher|
|Pointer updates|Fewer|More|
|Easier deletion of known node|Less convenient|More convenient|
|First node|`prev` doesn't exist|`prev = NULL`|
|Last node|`next = NULL`|`next = NULL`|

---

# 14. GATE Traps from These Pages

### Trap 1 — Forgetting `prev`

If you insert at beginning:

```c
newNode->next = head;
head = newNode;
```

is **not enough**.

You also need:

```c
oldHead->prev = newNode;
```

Otherwise backward traversal is broken.

---

### Trap 2 — Wrong direction while finding last node

To reach the last node:

```c
while (current->next != NULL)
```

NOT:

```c
while (current->prev != NULL)
```

---

### Trap 3 — Losing the node before `free()`

Bad:

```c
head = head->next;
free(head);
```

Now you've changed `head` and are potentially freeing the **new** first node.

Use:

```c
temp = head;
head = head->next;
free(temp);
```

---

### Trap 4 — Boundary conditions

Always check:

```text
First node:
prev = NULL

Last node:
next = NULL
```

These two conditions tell you whether you're at a boundary.

---

# 15. Final Revision Sheet

```text
             DOUBLY LINKED LIST

Node:
[value | prev | next]

Traversal:
Forward  → next
Backward ← prev

Invariant:
A->next = B
B->prev = A

Boundary:
head->prev = NULL
last->next = NULL
```

### Insert Beginning

```c
new->prev = NULL;
new->next = head;
head->prev = new;
head = new;
```

### Insert End

```c
new->prev = last;
new->next = NULL;
last->next = new;
```

### Delete Beginning

```c
temp = head;
head = head->next;
head->prev = NULL;
free(temp);
```

### Delete End

```c
temp = last;
last = last->prev;
last->next = NULL;
free(temp);
```

### GATE thought process

Whenever you see a DLL pointer question:

**1. Draw the nodes.**  
**2. Mark `prev` and `next`.**  
**3. Identify which node each pointer currently holds.**  
**4. Apply statements one by one.**  
**5. Check the DLL invariant:**

```text
A->next == B
B->prev == A
```

**6. Check boundary:**

```text
head->prev == NULL
last->next == NULL
```

That is basically the entire pointer game behind pages **59–74**. The lecture ends by consolidating the three linked-list variants: **Singly Linked List, Doubly Linked List, and Circular Linked List**.