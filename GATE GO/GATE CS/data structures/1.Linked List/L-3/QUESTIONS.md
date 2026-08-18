Absolutely. I checked the **entire 46-page L-3(3) PDF**. It contains **4 explicit MCQ questions**, plus the final **Swap Nodes in Pairs** coding problem. The earlier pages are explanation/dry-run pages, not separate questions.

# L-3(3) — ALL QUESTIONS + ANSWERS

## Q1. Recursive `insertRear()`

Given:

```text
4 → 7 → 5 → 3 → 2 → NULL
```

Function:

```c
Node* insertRear(Node* l, int data)
{
    if (l == NULL)
    {
        Node *n = createNode(data);
        return n;
    }
    else
    {
        l->next = insertRear(l->next, data);
        return l;
    }
}
```

Call:

```c
insertRear(head, 8);
```

### Options

```text
A) 4 → 7 → 5 → 3 → 8 → 2 → NULL

B) 8 → 4 → 7 → 5 → 3 → 2 → NULL

C) 4 → 7 → 5 → 3 → 2 → 8 → NULL

D) 4 → 7 → 5 → 3 → 2 → NULL
```

### ✅ Answer: C

```text
4 → 7 → 5 → 3 → 2 → 8 → NULL
```

### Why?

Recursion goes all the way to `NULL`:

```text
4 → 7 → 5 → 3 → 2 → NULL
                              ↓
                           create 8
```

Then during **unwinding**, each node reconnects:

```text
2 → 8
3 → 2 → 8
5 → 3 → 2 → 8
7 → 5 → 3 → 2 → 8
4 → 7 → 5 → 3 → 2 → 8
```

**GATE trick:** `l->next = recursive(...)` modifies the link while recursion returns.

---

# Q2. What does `fun(head, 8)` return?

Given:

```text
4 → 7 → 5 → 3 → 2 → NULL
```

Function:

```c
Node* fun(Node* l, int data)
{
    if (l == NULL)
    {
        Node *n = createNode(data);
        return n;
    }
    else
    {
        l->next = fun(l->next, data);
        return l->next;
    }
}
```

Call:

```c
fun(head, 8);
```

### Options

```text
A) 4 → 7 → 5 → 3 → 8 → 2 → NULL

B) 8 → 4 → 7 → 5 → 3 → 2 → NULL

C) 8 → NULL

D) 4 → 7 → 5 → 3 → 2 → NULL
```

### ✅ Answer: C

```text
8 → NULL
```

### The important line:

```c
return l->next;
```

Compare with Q1:

```c
return l;       // Q1
```

Here every recursive frame returns its `l->next`.

Eventually the original call returns the newly created node:

```text
8 → NULL
```

🔥 **This is the main trick of Q2:**  
Don't only track how the list changes. Track **what pointer is returned**.

---

# Q3. `removeFirst(head, 2)`

Given:

```text
1 → 2 → 2 → 8 → 6 → 2 → 2 → NULL
```

Function:

```c
Node* removeFirst(Node* head, int x)
{
    if (head == NULL)
        return NULL;

    if (head->data == x)
    {
        Node* tmp = head->next;
        free(head);
        return tmp;
    }
    else
    {
        head->next = removeFirst(head->next, x);
        return head;
    }
}
```

Call:

```c
removeFirst(head, 2);
```

### Options

```text
A) 1 → 8 → 6 → 2 → 2 → NULL

B) 1 → 2 → 8 → 6 → 2 → NULL

C) 1 → 8 → 6 → NULL

D) 1 → 2 → 8 → 6 → 2 → 2 → NULL
```

### ✅ Answer: D

```text
1 → 2 → 8 → 6 → 2 → 2 → NULL
```

### Why?

The **first** `2` is removed:

```text
1 → [2] → 2 → 8 → 6 → 2 → 2
     ↑
   DELETE
```

The remaining list:

```text
1 → 2 → 8 → 6 → 2 → 2 → NULL
```

It does **not** remove the other `2`s because the function stops once the first matching node is found.

---

# Q4. `removeAll(head, 2)`

Given:

```text
1 → 2 → 2 → 8 → 6 → 2 → 2 → NULL
```

Function:

```c
Node* removeAll(Node* l, int data)
{
    if (l == NULL)
        return NULL;

    if (l->data == data)
        return removeAll(l->next, data);
    else
    {
        l->next = removeAll(l->next, data);
        return l;
    }
}
```

Call:

```c
removeAll(head, 2);
```

### Options

```text
A) 1 → 8 → 6 → 2 → 2 → NULL

B) 1 → 2 → 8 → 6 → 2 → NULL

C) 1 → 8 → 6 → NULL

D) None of the above
```

### ✅ Answer: C

```text
1 → 8 → 6 → NULL
```

### Why?

Here **every occurrence** of `2` is removed.

```text
1 → [2] → [2] → 8 → 6 → [2] → [2]
```

Final:

```text
1 → 8 → 6 → NULL
```

The key difference from Q3:

```text
removeFirst → stop after FIRST match
removeAll   → recursively continue removing matches
```

---

# Q5. Swap Nodes in Pairs

Final pages introduce the **Swap Nodes in Pairs** problem.

Given:

```text
1 → 2 → 3 → 4 → NULL
```

Swap every adjacent pair.

### Expected output

```text
2 → 1 → 4 → 3 → NULL
```

The lecture's recursive solution uses:

```c
struct ListNode* swapPairs(struct ListNode* l)
{
    if (l == NULL || l->next == NULL)
        return l;

    int temp = l->val;
    l->val = l->next->val;
    l->next->val = temp;

    swapPairs(l->next->next);

    return l;
}
```

### Important observation

This particular implementation **swaps the values**, not the actual node pointers.

So:

```text
1 → 2 → 3 → 4
```

becomes:

```text
2 → 1 → 4 → 3
```

by swapping:

```text
1 ↔ 2
3 ↔ 4
```

Then recursion jumps two nodes:

```text
swapPairs(l->next->next)
             ↑
             3
```

Base case:

```c
if (l == NULL || l->next == NULL)
    return l;
```

So recursion stops when there are **0 or 1 nodes remaining**.

---

# 🧠 FINAL ANSWER SHEET

|Q|Topic|Answer|
|---|---|---|
|**1**|`insertRear(head,8)`|**C** → `4→7→5→3→2→8`|
|**2**|`fun(head,8)`|**C** → `8→NULL`|
|**3**|`removeFirst(head,2)`|**D** → `1→2→8→6→2→2`|
|**4**|`removeAll(head,2)`|**C** → `1→8→6`|
|**5**|Swap Nodes in Pairs|`1→2→3→4` → **`2→1→4→3`**|

### 🔥 What this lecture is really testing

```text
Recursive Linked List
        ↓
   Go forward
        ↓
 Reach base case
        ↓
   COME BACK
        ↓
Track RETURN VALUE
        ↓
Modify/reconnect pointers
```

**For GATE, Q1/Q2 are especially important:** a tiny difference between `return l` and `return l->next` can completely change the answer.