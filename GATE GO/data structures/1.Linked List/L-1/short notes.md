# Linked List — Short Notes

## 1. Linked List

A linked list is a collection of **nodes connected using pointers**.

```text
Head
 ↓
[data|next] → [data|next] → [data|NULL]
```

Each node contains:

- `data` → stores value
    
- `next` → stores address of next node
    

```c
struct node {
    int data;
    struct node *next;
};
```

---

## 2. Node vs Pointer to Node

```c
struct node x;     // actual node
struct node *p;    // pointer to a node
```

```text
p      → address of node
*p     → actual node
p->data → data of node
p->next → address of next node
```

For an actual struct variable:

```c
x.data
x.next
```

For a struct pointer:

```c
p->data
p->next
```

Also:

```c
p->data == (*p).data
```

---

## 3. Dynamic Node Creation

```c
struct node *p = malloc(sizeof(struct node));
```

Equivalent preferred form:

```c
struct node *p = malloc(sizeof(*p));
```

Because:

```text
p   → struct node *
*p  → struct node
```

Therefore:

```text
sizeof(*p) = sizeof(struct node)
```

⚠️ Do not confuse:

```text
sizeof(struct node)    → size of entire node
sizeof(struct node *)  → size of pointer only

sizeof(*p)             → size of node
sizeof(p)              → size of pointer
```

This distinction is part of the lecture's memory-allocation explanation.

---

## 4. Linking Nodes

```c
p->next = q;
```

means:

> Make the node pointed to by `p` point to the node pointed to by `q`.

```text
p              q
↓              ↓
[10| •] ─────→ [20|NULL]
```

```c
p = q;
```

means:

> Make `p` point wherever `q` points.

```c
p = p->next;
```

means:

> Move `p` to the next node.

---

# Why Linked List?

Linked lists are useful when the **number of elements is dynamic** and nodes need to be created as required. The lecture contrasts this with fixed-size arrays.

### Array vs Linked List

|Array|Linked List|
|---|---|
|Contiguous memory|Non-contiguous memory|
|Better cache locality|Poor cache locality|
|Fixed-size array is less flexible|Dynamic size|
|Random access `O(1)`|Random access `O(n)`|
|No pointer per element|Extra `next` pointer|

The lecture develops these comparisons on pages 33–43.

---

# Important Conditions

### Empty Linked List

```c
head == NULL
```

### Single Node

```c
head != NULL && head->next == NULL
```

---

# Traversing a Linked List

Core pattern:

```c
while (head != NULL) {
    // process current node
    head = head->next;
}
```

Remember:

```text
head = head->next
        ↓
move to next node
```

---

# Length of Linked List

```c
int length(struct node *head)
{
    int count = 0;

    while (head != NULL) {
        count++;
        head = head->next;
    }

    return count;
}
```

```text
Time  → O(n)
Space → O(1)
```

The lecture implements this on pages 47–48.

---

# Printing Linked List

```c
void printList(struct node *head)
{
    while (head != NULL) {
        printf("%d ", head->data);
        head = head->next;
    }
}
```

The final pages use the same traversal pattern for printing.

---

## ⭐ Must Remember

```text
struct node x    → actual node
struct node *p   → pointer to node

p                → address
*p               → node

p->data          → node's data
p->next          → next node's address

sizeof(p)        → pointer size
sizeof(*p)       → node size

p = p->next      → move to next node

head == NULL     → empty list
head->next==NULL → single node
```

That's about the amount I'd actually write in notes for this lecture. The lecture lists **insertion and deletion** as upcoming operations, but doesn't teach their algorithms in this PDF yet, so I wouldn't add those to these notes.