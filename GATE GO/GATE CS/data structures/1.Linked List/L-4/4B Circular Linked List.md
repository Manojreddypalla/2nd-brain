# L-4(4) — Pages 34–58 Notes

## Circular Linked List + Insertion Operations

These notes cover **pages 34–58** of the lecture, preserving the lecture's scope and terminology.

---

# 1. Circular Linked List (CLL)

A **Circular Linked List** is a linked list where the **last node points back to the first node** instead of pointing to `NULL`.

### Singly Linked List

```text
head
 ↓
10 → 20 → 30 → 40 → NULL
```

### Circular Linked List

```text
       ┌──────────────────────┐
       ↓                      │
10 → 20 → 30 → 40 ───────────┘
↑
head
```

So:

```text
last->next = head
```

There is **no `NULL` at the end**.

---

# 2. Why Normal Traversal Doesn't Work

For a normal singly linked list:

```c
curr != NULL
```

is the stopping condition.

But in a CLL:

```text
last → head → ... → last → head → ...
```

So `curr != NULL` is **never false**.

Therefore, traversal must detect when we have returned to `head`.

### Correct condition

```c
curr != head
```

Typical traversal:

```c
struct node *curr = head;

if (head != NULL)
{
    do
    {
        // process curr
        curr = curr->next;
    }
    while (curr != head);
}
```

---

# 3. Length of Circular Linked List

For a normal linked list:

```c
for (curr = head; curr != NULL; curr = curr->next)
```

For CLL:

```c
for (curr = head; curr != head; curr = curr->next)
```

would be wrong because the condition is initially false.

Instead, the lecture uses a traversal that counts nodes until `curr` comes back to `head`.

### Concept

```text
head
 ↓
1 → 2 → 3 → 4
↑           │
└───────────┘
```

Start:

```text
curr = head
count = 0
```

Visit:

```text
1 → count = 1
2 → count = 2
3 → count = 3
4 → count = 4
```

Then:

```text
curr == head
```

Stop.

### Complexity

```text
Time  = O(n)
Space = O(1)
```

---

# 4. Empty Circular Linked List

Special case:

```text
head == NULL
```

There are **zero nodes**.

Therefore:

```c
if (head == NULL)
    return 0;
```

is required before circular traversal.

Otherwise, trying to access:

```c
head->next
```

when `head == NULL` causes invalid access.

---

# 5. Important CLL Traversal Trap 🔥

Consider:

```c
int lengthCLL(struct node *head)
{
    int result = 0;

    for (struct node *curr = head;
         curr != head;
         curr = curr->next)
    {
        result++;
    }

    return result;
}
```

This does **not work**.

Why?

At the beginning:

```text
curr = head
```

Therefore:

```text
curr != head
```

is immediately:

```text
FALSE
```

So the loop executes **zero times**.

---

# 6. Correct CLL Length Logic

Use a `do-while` structure:

```c
int lengthCLL(struct node *head)
{
    if (head == NULL)
        return 0;

    int result = 0;
    struct node *current = head;

    do
    {
        current = current->next;
        result++;
    }
    while (current != head);

    return result;
}
```

### Why `do-while`?

Because the first node must be processed **before checking whether we have returned to `head`**.

This is a very useful pattern for circular structures.

---

# 7. Printing a Circular Linked List

A similar issue occurs while printing.

For SLL:

```c
while (current != NULL)
```

For CLL:

```c
do
{
    printf("%d ", current->value);
    current = current->next;
}
while (current != head);
```

### Example

```text
head
 ↓
1 → 2 → 3 → 4
↑           │
└───────────┘
```

Output:

```text
1 2 3 4
```

Then `current` becomes `head`, so traversal stops.

---

# 8. Question — Find Last Node of CLL

The lecture asks:

> Which code prints the last node of a circular (non-empty) linked list?

Correct logic:

```c
struct node *current = head;

while (current->next != head)
    current = current->next;

printf("%d", current->value);
```

### ✅ Answer: A

### Why?

The **last node** is the node whose:

```text
next == head
```

So keep moving until:

```c
current->next == head
```

Then:

```text
current = last node
```

---

# 9. CLL — Identifying the Last Node

Remember this mental picture:

```text
head
 ↓
1 → 2 → 3 → 4
↑           │
└───────────┘
            ↑
          last
```

The last node isn't identified by:

```c
current->next == NULL
```

because **there is no NULL**.

Instead:

```c
current->next == head
```

means:

> `current` is the last node.

🔥 **GATE trigger:**  
For CLL, replace the mental idea of **"NULL marks the end"** with **"head marks the end of one complete cycle."**

---

# 10. Inserting at End of Circular Linked List

Suppose:

```text
head
 ↓
4 → 15 → 7 → 40
↑              │
└──────────────┘
```

We want to insert:

```text
5
```

---

## Step 1 — Create New Node

```c
struct node *newNode =
    malloc(sizeof(struct node));

newNode->value = val;
newNode->next = NULL;
```

Initially:

```text
newNode
   ↓
 [ 5 | NULL ]
```

---

# 11. Step 2 — Find the Last Node

Start:

```c
struct node *current = head;
```

Move while:

```c
while (current->next != head)
    current = current->next;
```

Eventually:

```text
head
 ↓
4 → 15 → 7 → 40
              ↑
            current
```

Because:

```text
40->next == head
```

`40` is the last node.

---

# 12. Step 3 — Adjust Pointers

Before insertion:

```text
40 → head
```

We want:

```text
40 → 5 → head
```

Therefore:

```c
newNode->next = head;
current->next = newNode;
```

Final:

```text
head
 ↓
4 → 15 → 7 → 40 → 5
↑                    │
└────────────────────┘
```

### Key pointer changes

```text
old:
last → head

new:
last → newNode
newNode → head
```

---

# 13. Inserting at Beginning of CLL

Given:

```text
head
 ↓
4 → 15 → 7 → 40
↑              │
└──────────────┘
```

Insert:

```text
5
```

Expected:

```text
head
 ↓
5 → 4 → 15 → 7 → 40
↑                    │
└────────────────────┘
```

### Important

Inserting at beginning requires changing **two things**:

1. Last node's `next`
    
2. `head`
    

---

# 14. Step 1 — Create New Node

```c
struct node *firstNode =
    malloc(sizeof(struct node));

firstNode->next = head;
```

So:

```text
5 → old head
```

At this point:

```text
5 → 4 → 15 → 7 → 40
              ↑     │
              └─────┘
```

But the last node still points to the **old head**.

---

# 15. Step 2 — Find Last Node

```c
struct node *curr = head;

while (curr->next != head)
    curr = curr->next;
```

After traversal:

```text
4 → 15 → 7 → 40
              ↑
             curr
```

because:

```text
40->next == head
```

---

# 16. Step 3 — Connect Last Node to New Head

```c
curr->next = firstNode;
head = firstNode;
```

Now:

```text
        head
         ↓
5 → 4 → 15 → 7 → 40
↑                    │
└────────────────────┘
```

The circular property is restored.

---

# 17. CLL Insertion — Core Pattern

## Insert at END

```text
Find last
   ↓
last->next = newNode
newNode->next = head
```

```text
last → newNode → head
```

---

## Insert at BEGINNING

```text
Create new node
       ↓
newNode->next = head
       ↓
Find last
       ↓
last->next = newNode
       ↓
head = newNode
```

```text
last → newHead → oldHead
             ↑
            head
```

---

# 🔥 GATE Revision Table

|Operation|Important condition / pointer|
|---|---|
|SLL end|`current->next == NULL`|
|CLL end|`current->next == head`|
|SLL traversal|Stop at `NULL`|
|CLL traversal|Stop when `current == head`|
|CLL empty|`head == NULL`|
|CLL insert-end|`last → newNode → head`|
|CLL insert-begin|`last → newHead`, `newHead → oldHead`|

### Most important mental model

```text
SLL:
head → 1 → 2 → 3 → NULL
                  ↑
                STOP

CLL:
head → 1 → 2 → 3
      ↑         │
      └─────────┘
                ↑
              STOP when back at head
```

**The fundamental difference is just this:**

> **SLL uses `NULL` as the boundary; CLL uses `head` as the boundary.**