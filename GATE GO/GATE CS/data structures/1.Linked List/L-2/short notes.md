# L-2(5) — Linked List Operations + Recursion

### Full Short Notes / GATE Cheat Sheet

The lecture covers **Singly Linked List (SLL) insertion, deletion, and recursion-based operations**.

---

## 1. Length of Linked List

### Iterative

For a non-empty list:

```c
int count = 1;

while (head->next != NULL) {
    count++;
    head = head->next;
}

return count;
```

Alternative:

```c
int count = 0;

while (head != NULL) {
    count++;
    head = head->next;
}
```

### GATE Trap

- `while(head != NULL)` → visit **every node**
    
- `while(head->next != NULL)` → stops at **last node**
    
- If using `count = 1`, it assumes **at least one node**.
    

**Time:** `O(n)`  
**Space:** `O(1)`

---

# 2. Insertion in SLL

## A. Insert at Beginning

### Logic

```text
new node
   ↓
new->next = head
head = new
```

```c
struct node *current = malloc(sizeof(struct node));

current->data = 5;
current->next = head;
head = current;
```

### Key pattern

> **Connect new node to old head → move head to new node.**

**Time:** `O(1)`

---

# 3. Insert at End

### Logic

```text
new->next = NULL

last node
   ↓
last->next = new
```

Traversal:

```c
while (head->next != NULL)
    head = head->next;

head->next = endnode;
```

If the list can be empty:

```c
if (head == NULL) {
    head = endnode;
    return;
}
```

**Time:** `O(n)` without tail pointer  
**With tail pointer:** `O(1)`

---

# 4. Insert in Middle / After a Given Node

Suppose:

```text
A → B → C
```

Insert `X` after `B`:

```text
A → B → X → C
```

### Pointer order is IMPORTANT

```c
newnode->next = node->next;
node->next = newnode;
```

### Mental model

First preserve the old connection:

```text
X → C
```

Then attach:

```text
B → X
```

**Time:**

- Finding position: `O(n)`
    
- Actual insertion: `O(1)`
    

---

# 5. Deletion in SLL

Three cases:

1. Delete first node
    
2. Delete last node
    
3. Delete middle node
    

---

## A. Delete First Node

```c
if (head == NULL)
    return;

struct node *temp = head;

head = head->next;

free(temp);
```

### Pattern

```text
OLD HEAD → second

temp = old head
head = second
free(old head)
```

**Time:** `O(1)`

### GATE Trap

Always save the node before changing `head`:

```c
temp = head;
head = head->next;
free(temp);
```

---

# 6. Delete Last Node

### Important cases

### Empty list

```c
if (head == NULL)
    return;
```

### Only one node

```c
if (head->next == NULL) {
    free(head);
    head = NULL;
    return;
}
```

### General case

Find the **second-last node**:

```c
while (head->next->next != NULL)
    head = head->next;
```

Then:

```c
free(head->next);
head->next = NULL;
```

### Why `head->next->next`?

You need to stop at:

```text
A → B → C → NULL
    ↑
 second-last
```

because `B->next = NULL` must be performed.

**Time:** `O(n)`

---

# 7. Delete Middle Node

Suppose:

```text
A → B → C → D
```

Delete `C`.

Need to find **previous node B**.

```c
while (head->next->data != value)
    head = head->next;

temp = head->next;

head->next = temp->next;

free(temp);
```

Result:

```text
A → B → D
```

### Core pattern

> To delete a node in SLL, usually find its **previous node**, bypass target, then `free()` target.

**Time:** `O(n)`

---

# 8. Insertion vs Deletion — Pointer Cheat Sheet

### Insert after `node`

```c
new->next = node->next;
node->next = new;
```

### Delete `node` when `prev` is available

```c
prev->next = node->next;
free(node);
```

### Remember

```text
INSERT:
new → next first
then previous → new

DELETE:
previous → next
then free deleted
```

This pointer-order thinking is the main skill.

---

# 9. Recursion in Linked List

General recursive structure:

```c
function(node)
{
    if (node == NULL)
        return;

    // work

    function(node->next);
}
```

Think:

```text
head
 ↓
node1 → node2 → node3 → NULL
          ↓
       recursive call
```

Each recursive call moves one node forward.

---

# 10. Print Linked List Using Recursion

```c
void printLL(Node *head)
{
    if (head == NULL)
        return;

    printf("%d ", head->data);

    printLL(head->next);
}
```

For:

```text
4 → 7 → 5 → 3 → 2
```

Output:

```text
4 7 5 3 2
```

### Pattern

```text
WORK
↓
RECURSE
```

---

# 11. Print Linked List in Reverse

```c
void printLLReverse(Node *head)
{
    if (head == NULL)
        return;

    printLLReverse(head->next);

    printf("%d ", head->data);
}
```

For:

```text
4 → 7 → 5
```

Output:

```text
5 7 4
```

### Why?

Recursion goes forward first:

```text
4 → 7 → 5 → NULL
```

Then stack unwinds:

```text
5
7
4
```

So:

> **Work after recursive call = reverse order.**

---

# 12. Sum of All Nodes

```c
int sum(Node *head)
{
    if (head == NULL)
        return 0;

    return head->data + sum(head->next);
}
```

For:

```text
4 → 7 → 5
```

```text
sum(4)
= 4 + sum(7)
= 4 + 7 + sum(5)
= 4 + 7 + 5
= 16
```

**Time:** `O(n)`  
**Recursive stack:** `O(n)`

---

# 13. Length Using Recursion

```c
int length(Node *head)
{
    if (head == NULL)
        return 0;

    return 1 + length(head->next);
}
```

### Mental formula

```text
length(current)
=
1 + length(rest)
```

Example:

```text
4 → 7 → 5 → NULL

1 + 1 + 1 + 0 = 3
```

---

# 14. Recursive Search

Given:

```c
struct node *search(struct node *head, int value)
{
    if (head == NULL)
        return NULL;

    if (head->data == value)
        return head;

    return search(head->next, value);
}
```

### Pattern

```text
Is current node target?
        ↓ NO
Search remaining list
```

For:

```text
4 → 7 → 5 → NULL
```

Search `5`:

```text
search(4)
 → search(7)
    → search(5)
       → return node 5
```

The lecture's MCQ asks for the recursive call after the current node doesn't match; **option A** is correct:

```c
return search(head->next, value);
```

---

# 15. Recursion + Linked List Master Pattern

Most recursive linked-list problems reduce to:

```c
if (head == NULL)
    return BASE_CASE;

return CURRENT_WORK +/or recursive(head->next);
```

### Examples

|Problem|Recursive idea|
|---|---|
|Print|`print(head)` → work → `print(head->next)`|
|Reverse print|`print(head->next)` → work|
|Sum|`head->data + sum(head->next)`|
|Length|`1 + length(head->next)`|
|Search|check current → search `head->next`|

---

# ⚡ GATE Quick Cheat Sheet

```text
INSERT BEGINNING
new->next = head
head = new
O(1)
```

```text
INSERT END
while(head->next != NULL)
    head = head->next;
head->next = new;
O(n)
```

```text
INSERT AFTER NODE
new->next = node->next
node->next = new
O(1) after locating node
```

```text
DELETE FIRST
temp = head
head = head->next
free(temp)
O(1)
```

```text
DELETE LAST
find second-last
free(last)
secondLast->next = NULL
O(n)
```

```text
DELETE MIDDLE
find previous
prev->next = target->next
free(target)
O(n)
```

```text
LENGTH
if NULL → 0
else → 1 + length(head->next)
```

```text
SUM
if NULL → 0
else → head->data + sum(head->next)
```

```text
FORWARD PRINT
work → recursion
```

```text
REVERSE PRINT
recursion → work
```

```text
SEARCH
if NULL → NULL
if match → head
else → search(head->next, value)
```

### ⭐ Most Important GATE Patterns

**1. `head->next` vs `head->next->next`**

```text
head->next == NULL
→ head is LAST

head->next->next == NULL
→ head is SECOND-LAST
```

**2. Recursion position**

```text
work → recursive call
= forward processing

recursive call → work
= reverse processing
```

**3. Pointer modification**

```text
Insertion:
new->next = old_next
previous->next = new

Deletion:
previous->next = target->next
free(target)
```

These are the core patterns from this lecture; the final slide only labels **“Tricky Recursion on Linkedlist” as optional**, without providing its content.