# 🌳 DFS — Iterative Traversals (Theory + Notes)

---

# 📌 What is an Iterative Traversal?

An **Iterative Traversal** is a **Depth First Search (DFS)** technique that **does not use recursion**.

Instead of relying on the system's call stack, we create and manage our **own stack**.

> **Recursion = System Stack**  
> **Iteration = Programmer's Stack**

---

# 📌 Why do we need Iterative Traversals?

Recursive solutions are simple, but they have limitations.

- Deep trees can cause **Stack Overflow**.
    
- Sometimes recursion is not allowed or not preferred.
    
- Interviews often ask for the iterative version to test your understanding of DFS.
    

---

# 📌 Recursive vs Iterative

|Recursive DFS|Iterative DFS|
|---|---|
|Uses function calls|Uses a stack|
|System manages memory|You manage memory|
|Short and clean|Slightly longer|
|Easy to understand|Better control|
|Can overflow for deep trees|No recursion limit|

---

# 📌 Why a Stack?

DFS always visits the **most recently discovered node first**.

That is exactly how a **Stack (LIFO)** works.

```text
Push 1
Push 2
Push 4

Stack

Top
4
2
1
Bottom
```

The last inserted node (`4`) is visited first.

---

# 📌 General Iterative DFS Pattern

Every iterative DFS follows this structure.

```cpp
stack<TreeNode*> st;

st.push(root);

while(!st.empty()){

    TreeNode* node = st.top();
    st.pop();

    // Process node

    // Push children
}
```

The difference between preorder, inorder, and postorder lies in **when and how** nodes are pushed or processed.

---

# 📌 Types of Iterative Traversals

There are three iterative DFS traversals.

|Traversal|Data Structure|
|---|---|
|Preorder|One Stack|
|Inorder|One Stack|
|Postorder|One Stack or Two Stacks|

---

# 🌳 Iterative Preorder

## Order

```text
Root → Left → Right
```

## Idea

Process the node immediately.

Since the stack is LIFO:

- Push **Right**
    
- Push **Left**
    

This makes the left child come out first.

---

### Visualization

Tree

```text
      1
     / \
    2   3
```

Stack

```text
Push 1

Pop 1

Push 3

Push 2

Pop 2

Pop 3
```

Output

```text
1 2 3
```

---

## Key Observation

Always remember:

> **Push Right before Left.**

Because the stack reverses the order.

---

## Applications

- Copy Tree
    
- Serialization
    
- Prefix Expression
    
- N-ary Tree Traversal
    

---

# 🌳 Iterative Inorder

## Order

```text
Left → Root → Right
```

## Idea

Keep moving left until no left child exists.

Then:

- Process the node
    
- Move to its right child
    

Unlike preorder, nodes are **not processed immediately**.

---

### Visualization

```text
        1
       /
      2
     /
    3
```

Traversal

```text
Push 1

Push 2

Push 3

NULL

Pop 3

Pop 2

Pop 1
```

Output

```text
3 2 1
```

---

## Key Observation

Go as far left as possible before visiting any node.

---

## Applications

- BST Traversal
    
- Validate BST
    
- Kth Smallest
    
- Sorted Order in BST
    

---

# 🌳 Iterative Postorder (Two Stacks)

## Order

```text
Left → Right → Root
```

## Idea

Postorder is difficult because the parent must be processed **after both children**.

A simple trick:

Perform a modified preorder

```text
Root → Right → Left
```

Store it in another stack.

When reversed,

```text
Left → Right → Root
```

---

### Visualization

```text
Stack 1

1

↓

3 2

↓

2

↓

4 5
```

Second stack stores reversed order.

---

## Applications

- Tree Deletion
    
- Height
    
- Diameter
    
- Tree DP
    

---

# 🌳 Iterative Postorder (One Stack)

## Order

```text
Left → Right → Root
```

## Idea

This is the hardest iterative traversal.

We must know whether the right subtree has already been visited.

For that, we maintain:

```cpp
TreeNode* lastVisited;
```

If the right child has already been processed,

then we can safely process the parent.

---

## Key Observation

Never process a node until:

- Left subtree is finished.
    
- Right subtree is finished.
    

---

## Applications

Same as recursive postorder.

---

# 📌 Time Complexity

Each node is visited once.

```text
O(n)
```

---

# 📌 Space Complexity

The stack stores at most one path from root to leaf.

```text
O(h)
```

where

```text
h = Height of Tree
```

Worst Case

```text
O(n)
```

Balanced Tree

```text
O(log n)
```

---

# 📌 Common Mistakes

### ❌ Forgetting to check if root is NULL

Always handle the empty tree first.

---

### ❌ Preorder

Pushing Left before Right.

This produces:

```text
Root Right Left
```

instead of

```text
Root Left Right
```

---

### ❌ Inorder

Processing the node before reaching the leftmost node.

---

### ❌ Postorder

Processing the parent before the right subtree.

---

### ❌ Mixing Stack and Queue

DFS always uses a **Stack**.

BFS always uses a **Queue**.

---

# 📌 Pattern Recognition

|Question asks...|Traversal|
|---|---|
|Parent First|Preorder|
|BST / Sorted Order|Inorder|
|Children First|Postorder|

---

# 📌 Interview Tips

Think about **when** the current node should be processed.

- Before children → Preorder
    
- Between children → Inorder
    
- After children → Postorder
    

The iterative version changes **how** the traversal is implemented, but the traversal order remains exactly the same.

---

# 📌 Revision Box

```text
Iterative DFS

↓

Uses Stack
```

```text
Preorder

Process

↓

Push Right

↓

Push Left
```

```text
Inorder

Go Left

↓

Process

↓

Go Right
```

```text
Postorder

Children

↓

Parent
```

```text
DFS

Recursive → System Stack

Iterative → Explicit Stack
```

---

## ✅ Next Topic

**Iterative Traversal Codes**, including:

1. Iterative Preorder (1 Stack)
    
2. Iterative Inorder (1 Stack)
    
3. Iterative Postorder (2 Stacks)
    
4. Iterative Postorder (1 Stack)
    
5. Comparison of all iterative approaches