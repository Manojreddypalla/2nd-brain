# 🌳 BFS (Breadth First Search) — Short Notes

---

# 1. What is BFS?

### Definition

**Breadth First Search (BFS)** visits nodes **level by level** from top to bottom.

Instead of going deep, BFS explores all nodes of one level before moving to the next.

### Example

```text
        A
       / \
      B   C
     / \   \
    D   E   F
```

Traversal

```text
A → B → C → D → E → F
```

### Key Idea

```text
Visit Every Level Completely
```

---

# 2. BFS Intuition

Imagine water flowing through a tree.

```text
Level 1

      A

↓

Level 2

    B     C

↓

Level 3

  D   E     F
```

Water fills one level first, then the next.

Unlike DFS:

```text
DFS

Go Deep

↓

Backtrack
```

BFS

```text
Finish Current Level

↓

Go to Next Level
```

---

# 3. Why Queue?

BFS processes nodes in the **same order they are discovered**.

A **Queue (FIFO)** naturally supports this.

```text
FIFO

First In

↓

First Out
```

Example

```text
Queue

A

↓

Process A

↓

Insert

B C

↓

Process B

↓

Insert

D E

↓

Process C

↓

Insert

F
```

Oldest node is always processed first.

---

# 4. Queue Visualization

Tree

```text
        A
       / \
      B   C
     / \   \
    D   E   F
```

### Step 1

Queue

```text
[A]
```

Visit

```text
A
```

---

### Step 2

Push children

```text
[B C]
```

Visit

```text
B
```

---

### Step 3

Queue

```text
[C D E]
```

Visit

```text
C
```

---

### Step 4

Queue

```text
[D E F]
```

Continue until queue becomes empty.

---

# 5. BFS Template

### Basic Template

```cpp
vector<int> bfs(TreeNode* root)
{
    vector<int> ans;

    if(root == NULL)
        return ans;

    queue<TreeNode*> q;
    q.push(root);

    while(!q.empty())
    {
        TreeNode* node = q.front();
        q.pop();

        ans.push_back(node->val);

        if(node->left)
            q.push(node->left);

        if(node->right)
            q.push(node->right);
    }

    return ans;
}
```

### Generic Algorithm

```text
Push Root

↓

While Queue Not Empty

↓

Pop Front Node

↓

Process Node

↓

Push Left Child

↓

Push Right Child
```

---

# 6. Level Processing

Many interview problems require processing **one level at a time**.

### Template

```cpp
queue<TreeNode*> q;
q.push(root);

while(!q.empty())
{
    int size = q.size();

    for(int i = 0; i < size; i++)
    {
        TreeNode* node = q.front();
        q.pop();

        // Process current level

        if(node->left)
            q.push(node->left);

        if(node->right)
            q.push(node->right);
    }
}
```

### Why `size = q.size()`?

It stores the **number of nodes in the current level**.

Example

```text
        A
       / \
      B   C
     / \   \
    D   E   F
```

Queue

```text
[A]
```

Size = 1

Process only A.

---

Queue

```text
[B C]
```

Size = 2

Process B and C.

---

Queue

```text
[D E F]
```

Size = 3

Process D, E, F.

Each iteration processes exactly one level.

---

# Applications of Level Processing

Used in

- Level Order Traversal
    
- Zigzag Traversal
    
- Left View
    
- Right View
    
- Average of Levels
    
- Maximum Width
    
- BFS-based Tree Problems
    

---

# Complexity

### Time

Every node is visited once.

```text
O(n)
```

---

### Space

Queue may store an entire level.

Worst case

```text
O(n)
```

---

# BFS Flow

```text
Push Root

↓

Queue

↓

Pop Node

↓

Visit Node

↓

Push Children

↓

Repeat

↓

Queue Empty

↓

Done
```

---

# DFS vs BFS

|DFS|BFS|
|---|---|
|Go Deep|Go Level by Level|
|Stack / Recursion|Queue|
|Backtracking|No Backtracking|
|O(h) Space|O(n) Space|
|Height, Diameter, LCA|Level Order, Views, Width|

---

# 🧠 Memory Tricks

### BFS

```text
B = Breadth

Visit Level by Level
```

### Queue

```text
FIFO

First In

↓

First Out
```

### Template

```text
Push Root

↓

While Queue

↓

Pop

↓

Visit

↓

Push Children
```

### Level Processing

```text
size = q.size()

↓

Current Level

↓

Process Exactly size Nodes
```

---

# 🎯 Interview Cheat Sheet

```text
BFS
---
Level by Level

Uses
----
Queue (FIFO)

Basic Steps
-----------
Push Root
Pop Front
Visit Node
Push Children

Level Processing
----------------
size = q.size()

Process size Nodes

Complexity
----------
Time  : O(n)
Space : O(n)

Common Problems
---------------
Level Order
Zigzag
Left View
Right View
Top View
Bottom View
Vertical Order
Maximum Width
```

These notes are enough as a **2-page revision sheet** for all BFS fundamentals before moving on to BFS problem patterns.