# GATE CSE — Linked List

## Pages 24–33: Recursion in Linked List

> **Pages 24–33 only.** The lecture introduces recursion, applies it to linked lists, and ends with a recursive search question.

---

## 1. Recursion in Linked List

A linked list is naturally recursive because:

```text
Linked List = Node + remaining Linked List
```

For example:

```text
4 → 7 → 5 → 3 → 2 → NULL
```

can be viewed as:

```text
4 → (7 → 5 → 3 → 2 → NULL)
```

So `head->next` represents a **smaller linked-list problem**.

---

# 2. Recursion vs Induction

The lecture connects recursion with mathematical induction.

### Mathematical Induction

To prove:

```text
f(n) = ...
```

1. **Base case:** prove for smallest case.
    
2. **Induction hypothesis:** assume true for `k`.
    
3. **Induction step:** prove true for `k+1`.
    

### Recursion

Similarly:

1. **Base case** → stop recursion.
    
2. **Recursive case** → solve a smaller problem.
    
3. The smaller result helps solve the original problem.
    

### Mental connection

```text
Induction:
small case → larger case

Recursion:
large problem → smaller problem
```

---

# 3. Fibonacci Example

Lecture shows:

```text
fib(n)
 ↙    ↘
fib(n-1) fib(n-2)
```

with:

```text
fib(n) = fib(n-1) + fib(n-2)
```

Each recursive call creates a **smaller problem**.

---

# 4. Printing a Linked List Using Recursion

Given:

```text
4 → 7 → 5 → 3 → 2 → NULL
```

Code:

```c
void printLL(Node *head)
{
    if (head == NULL)
        return;

    printf("%d ", head->data);

    printLL(head->next);
}
```

The lecture demonstrates this on pages 27–28.

### Execution idea

```text
print(4)
  print(7)
    print(5)
      print(3)
        print(2)
          print(NULL)
```

Then recursion stops.

Output:

```text
4 7 5 3 2
```

### Pattern

```text
process current node
        ↓
recurse on next node
```

---

# 5. Recursive Call Stack

For:

```c
printLL(head);
```

calls become:

```text
printLL(4)
    ↓
printLL(7)
    ↓
printLL(5)
    ↓
printLL(3)
    ↓
printLL(2)
    ↓
printLL(NULL)
```

Each function call is stored in the **call stack**.

This is why recursion naturally follows the linked-list chain.

---

# 6. Print Linked List in Reverse

To print:

```text
4 → 7 → 5
```

as:

```text
5 7 4
```

change the order:

```c
void printLLReverse(Node *head)
{
    if (head == NULL)
        return;

    printLLReverse(head->next);

    printf("%d ", head->data);
}
```

Lecture page 29 shows this exact idea.

### Why does it reverse?

Because:

```c
printLLReverse(head->next);
```

executes **before**:

```c
printf(...)
```

So printing happens while recursion **returns/unwinds**.

```text
CALL ↓

4
7
5
NULL

RETURN ↑

print 5
print 7
print 4
```

### 🔥 GATE pattern

```text
print → recursive call
        ↓
        forward order

recursive call → print
        ↓
        reverse order
```

---

# 7. Sum of All Nodes

For:

```text
4 → 7 → 5 → NULL
```

we want:

```text
4 + 7 + 5 = 16
```

Recursive definition:

```c
int sum(Node *head)
{
    if (head == NULL)
        return 0;

    return head->data + sum(head->next);
}
```

Lecture page 30 shows this formulation.

### Mental model

```text
sum(4)
= 4 + sum(7)

= 4 + 7 + sum(5)

= 4 + 7 + 5 + sum(NULL)

= 16
```

### Pattern

```text
current value + answer for remaining list
```

---

# 8. Length of Linked List Using Recursion

Question:

> How many nodes are present?

Code:

```c
int length(Node *head)
{
    if (head == NULL)
        return 0;

    return 1 + length(head->next);
}
```

Lecture page 31 gives this exact recursive formulation.

For:

```text
4 → 7 → 5 → NULL
```

```text
length(4)
= 1 + length(7)
= 1 + 1 + length(5)
= 1 + 1 + 1 + length(NULL)
= 3
```

### Pattern

```text
Length = 1 + length(rest)
```

---

# 9. Recursive Linked-List Pattern

Three lecture examples:

### Print

```c
printf("%d", head->data);
printLL(head->next);
```

### Sum

```c
return head->data + sum(head->next);
```

### Length

```c
return 1 + length(head->next);
```

All three follow:

```text
                  current node
                       ↓
                solve something
                       +
                 smaller list
                       ↓
                  head->next
```

### 🔥 Important abstraction

Whenever you see:

```c
function(head->next)
```

think:

> **"The same problem, but for the remaining list."**

---

# 10. Recursive Search in Linked List

The lecture ends with a question asking you to complete a recursive `search()` function.

Given:

```c
struct node *search(struct node *head, int value)
{
    if (head == NULL)
        return NULL;

    if (head->data == value)
        return head;

    // recursive case
}
```

The correct recursive idea is:

```c
return search(head->next, value);
```

### Why?

If current node doesn't contain the value:

```text
search current node
        ↓
not found
        ↓
search remaining list
```

So:

```c
return search(head->next, value);
```

is the correct option.

---

# 11. Recursive Search Dry Run

List:

```text
4 → 7 → 5 → 3
```

Search for `5`.

```text
search(4,5)
 ↓
4 != 5
 ↓
search(7,5)
 ↓
7 != 5
 ↓
search(5,5)
 ↓
5 == 5
 ↓
return node 5
```

The returned pointer travels back through the recursive calls.

---

# 12. Complete GATE Revision Sheet

## Recursive Linked List

```text
Base case:
    head == NULL

Recursive smaller problem:
    head->next
```

### Print forward

```c
printf("%d", head->data);
printLL(head->next);
```

### Print reverse

```c
printLL(head->next);
printf("%d", head->data);
```

### Sum

```c
return head->data + sum(head->next);
```

### Length

```c
return 1 + length(head->next);
```

### Search

```c
if (head->data == value)
    return head;

return search(head->next, value);
```

---

## 🧠 The One Pattern You Should Recognize

For a linked list:

```text
HEAD
 ↓
[DATA | NEXT] → [DATA | NEXT] → ...
```

`head->next` is literally:

> **the same linked-list problem starting from the next node.**

Therefore:

```text
Linked List
     ↓
Current node + smaller linked list
     ↓
             head
               ↓
         ┌────────────┐
         │ process it │
         └─────┬──────┘
               ↓
          head->next
               ↓
       same function again
```

### GATE trigger 🚨

If you see:

```c
f(head->next)
```

immediately think:

**"Recursion is moving through the linked list."**

If `printf` is **before** recursion → **forward order**.

If `printf` is **after** recursion → **reverse order**.

If the function returns:

```c
head->data + f(head->next)
```

→ **aggregation/sum pattern**.

If it returns:

```c
1 + f(head->next)
```

→ **count/length pattern**.

If it checks:

```c
head->data == value
```

then recursively calls on `head->next` → **search pattern**.

---

### Lecture coverage: Pages 24–33

|Page|Topic|
|---|---|
|24|Recursion in Linked List|
|25|Recursion + induction idea|
|26|Fibonacci recursive structure|
|27–28|Print linked list using recursion|
|29|Print linked list in reverse|
|30|Sum of all nodes|
|31|Length using recursion|
|32|Recursive search question|
|33|Optional: tricky recursion on linked list|

The final page labels **tricky recursion on linked list as optional**, so it isn't part of the required core material.