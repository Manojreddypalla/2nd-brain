# GATE CSE — Linked List Notes

## Pages 13–23: Deletion in Singly Linked List

> **Scope:** Pages **13–23 only** of the uploaded lecture. This section covers deletion of the first, last, and intermediate nodes in a singly linked list.

---

# 1. Deletion in Singly Linked List

Deletion means:

> **Remove a node from the chain while keeping the remaining nodes connected.**

Three cases:

1. Delete first node
    
2. Delete last node
    
3. Delete intermediate/middle node
    

The main skill is **pointer manipulation**.

---

# 2. Delete First Node

Suppose:

```text
HEAD
 ↓
[4] → [15] → [7] → [40] → NULL
```

We want to delete `4`.

### Step 1 — Save the old head

```c
struct node *temp = head;
```

Now:

```text
temp
 ↓
[4] → [15] → [7] → [40] → NULL
 ↑
head
```

### Step 2 — Move head

```c
head = head->next;
```

Now:

```text
temp
 ↓
[4] → [15] → [7] → [40] → NULL

head
       ↓
      [15] → [7] → [40] → NULL
```

### Step 3 — Free old node

```c
free(temp);
```

Final:

```text
HEAD
 ↓
[15] → [7] → [40] → NULL
```

The lecture demonstrates exactly this sequence: save `head`, advance `head`, then `free(temp)`.

---

# 3. Code — Delete First Node

```c
void deleteFirst(struct node **head)
{
    if (*head == NULL)
        return;

    struct node *temp = *head;

    *head = (*head)->next;

    free(temp);
}
```

### Mental pattern

```text
OLD HEAD
   ↓
[ A ] → [ B ] → [ C ]

temp = head
head = head->next
free(temp)

        HEAD
         ↓
[ B ] → [ C ]
```

### Complexity

```text
Time  = O(1)
Space = O(1)
```

---

# 4. Why `free()` Is Required

Moving `head`:

```c
head = head->next;
```

only removes the node from the **logical list**.

The old node still occupies dynamically allocated memory.

Therefore:

```c
free(temp);
```

releases that memory.

### Think in two separate concepts

```text
Logical deletion:
    Remove node from chain

Memory deletion:
    Release allocated memory
```

For dynamically allocated nodes, you generally need both.

---

# 5. Edge Case — Empty List

Before deleting:

```text
head == NULL
```

There is no node to delete.

Therefore:

```c
if (head == NULL)
    return;
```

The lecture explicitly handles this case before deletion.

---

# 6. Delete Last Node

Suppose:

```text
HEAD
 ↓
[4] → [15] → [7] → [40] → NULL
```

Want to delete `40`.

The problem:

> We need access to the **node before the last node**.

Why?

Because we need to change:

```text
[7] → [40]
```

into:

```text
[7] → NULL
```

---

# 7. Find the Previous Node of Last Node

Use:

```c
while (head->next->next != NULL)
{
    head = head->next;
}
```

Suppose:

```text
[4] → [15] → [7] → [40] → NULL
```

We stop when:

```text
head
 ↓
[7]

head->next
    ↓
  [40]

head->next->next == NULL
```

Therefore `head` points to the **previous node of the last node**.

The lecture uses this exact idea on pages 17–19.

---

# 8. Delete Last Node — Steps

Suppose:

```text
head
 ↓
[7] → [40] → NULL
```

### Step 1

Save last node:

```c
struct node *temp = head->next;
```

### Step 2

Disconnect it:

```c
head->next = NULL;
```

### Step 3

Free it:

```c
free(temp);
```

Final:

```text
HEAD
 ↓
[4] → [15] → [7] → NULL
```

---

# 9. Special Cases for Last-Node Deletion

There are **two important edge cases**.

## Case 1 — Empty list

```text
head == NULL
```

Nothing to delete.

```c
if (head == NULL)
    return NULL;
```

---

## Case 2 — Only one node

```text
HEAD
 ↓
[10] → NULL
```

There is no "previous node".

So:

```c
if (head->next == NULL)
{
    free(head);
    return NULL;
}
```

The lecture explicitly separates the zero-node and one-node cases before traversing.

---

# 10. Complete Logic — Delete Last Node

```c
if (head == NULL)
    return NULL;

if (head->next == NULL)
{
    free(head);
    return NULL;
}

struct node *temp = head;

while (temp->next->next != NULL)
{
    temp = temp->next;
}

free(temp->next);
temp->next = NULL;

return head;
```

### Complexity

```text
Time  = O(n)
Space = O(1)
```

Why `O(n)`?

Because we may have to walk through the entire list to find the second-last node.

---

# 11. Delete Middle / Intermediate Node

Suppose:

```text
[4] → [15] → [7] → [40] → NULL
```

Delete `7`.

We need:

```text
[4] → [15] → [40] → NULL
```

The important node is **the previous node**:

```text
previous
   ↓
 [15] → [7] → [40]
          ↑
         delete
```

---

# 12. Find Previous Node

If we want to delete the node containing `7`:

```c
while (head->next->data != 7)
{
    head = head->next;
}
```

When the loop finishes:

```text
head
 ↓
[15] → [7] → [40]
        ↑
       delete
```

So:

```text
head = previous node
head->next = node to delete
```

The lecture demonstrates this traversal on pages 19–20.

---

# 13. Reconnect the List

Before:

```text
[15] → [7] → [40]
         ↑
        temp
```

We want:

```text
[15] → [40]
```

So:

```c
temp = head->next;
head->next = temp->next;
free(temp);
```

### Pointer visualization

Before:

```text
head
 ↓
[15] ─────→ [7] ─────→ [40]
             ↑
            temp
```

After:

```text
head
 ↓
[15] ─────────────────→ [40]

temp → [7]   X
```

The lecture's pages 20–23 show this exact `temp`, reconnect, and `free(temp)` pattern.

---

# 14. Master Pattern for Middle Deletion

For:

```text
A → B → C
```

Delete `B`.

### Step 1

```c
temp = A->next;
```

### Step 2

```c
A->next = temp->next;
```

### Step 3

```c
free(temp);
```

Result:

```text
A → C
```

### 🧠 Remember

Insertion:

```text
A → B

new->next = A->next;
A->next = new;
```

Deletion:

```text
A → B → C

temp = A->next;
A->next = temp->next;
free(temp);
```

They are almost **mirror operations**.

---

# 15. Deletion — Complexity Cheat Sheet

|Operation|Time|Extra Space|
|---|--:|--:|
|Delete first|**O(1)**|O(1)|
|Delete last|**O(n)**|O(1)|
|Delete middle, node known via pointer to previous|**O(1)**|O(1)|
|Delete middle by searching value|**O(n)**|O(1)|

### Key GATE distinction

If the node/location is already known:

```text
Deletion = O(1)
```

If you have to search for it:

```text
Search + deletion = O(n)
```

---

# 16. The `head` Trap 🚨

Consider:

```c
head = head->next;
```

This **does not delete a node**.

It only moves the pointer.

```text
Before:

head
 ↓
[4] → [15]

After:

       head
        ↓
[4] → [15]
```

The `[4]` node still exists in memory.

To actually release it:

```c
temp = head;
head = head->next;
free(temp);
```

---

# 17. Another Important Trap — `free()` Doesn't Rearrange Links

Suppose:

```text
A → B → C
```

You do:

```c
free(B);
```

The link:

```text
A → B
```

doesn't magically become:

```text
A → C
```

You must first reconnect:

```c
A->next = B->next;
free(B);
```

Correct order:

```text
Reconnect → Free
```

---

# 18. GATE Pointer Pattern

For **deletion**, always think:

```text
previous
   ↓
[ A ] → [ B ] → [ C ]
          ↑
         temp
```

Then:

```c
temp = previous->next;
previous->next = temp->next;
free(temp);
```

Final:

```text
[ A ] → [ C ]
```

This is the fundamental deletion pattern.

---

# 19. Three Cases — One Table

|Case|Main pointer operation|Time|
|---|---|--:|
|Delete first|`head = head->next`|**O(1)**|
|Delete last|Find second-last → `prev->next = NULL`|**O(n)**|
|Delete middle|`prev->next = temp->next`|**O(1)** if previous known|

---

# ⚡ GATE Quick Revision

```text
DELETE FIRST
────────────────────────
temp = head
head = head->next
free(temp)


DELETE LAST
────────────────────────
if head == NULL
    return

if head->next == NULL
    free(head)
    return NULL

find second-last node

temp = head->next
head->next = NULL
free(temp)


DELETE MIDDLE
────────────────────────
find previous node

temp = previous->next
previous->next = temp->next
free(temp)
```

## 🧠 One-line mental model

```text
INSERT:
A → B
A → NEW → B

DELETE:
A → B → C
A → C
```

And the most important pointer rule:

> **Before destroying a node/link, make sure the remaining list still has a path to every node you want to keep.**

That is the core idea behind essentially all the deletion code in pages **13–23**.