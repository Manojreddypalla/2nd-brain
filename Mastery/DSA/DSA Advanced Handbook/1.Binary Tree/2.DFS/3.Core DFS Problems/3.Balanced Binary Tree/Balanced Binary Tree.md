Great! The next problem naturally builds on **Height** and **Diameter**.

---

# 🌳 DFS Problem 3 — Balanced Binary Tree (Theory + Notes)

> **LeetCode 110: Balanced Binary Tree**

---

# 📌 Problem Statement

Given the root of a binary tree, determine whether it is **height-balanced**.

A binary tree is **height-balanced** if:

> For **every node**, the difference between the heights of its left and right subtrees is **at most 1**.

Mathematically,

```text
|Height(Left) - Height(Right)| ≤ 1
```

---

# 📌 What is a Balanced Binary Tree?

Imagine you're checking whether a tree is **leaning too much** to one side.

A small difference is okay.

A large difference means the tree is unbalanced.

### ✅ Balanced Tree

```text
        1
       / \
      2   3
     / \
    4   5
```

Heights:

```text
Left Height = 2
Right Height = 1

Difference = 1
```

Balanced ✅

---

### ❌ Unbalanced Tree

```text
        1
       /
      2
     /
    3
```

Heights:

```text
Left Height = 2
Right Height = 0

Difference = 2
```

Not Balanced ❌

---

# 📌 The Important Rule

Many beginners think:

> "Just compare the height of the root."

❌ Wrong.

We must check **every node**.

Example:

```text
        1
       / \
      2   3
     /
    4
   /
  5
```

At the root:

```text
Left Height = 3
Right Height = 1

Difference = 2
```

Already unbalanced.

But even if the root were balanced, **a child subtree could still be unbalanced**.

---

# 📌 Example

```text
          1
         / \
        2   3
       /
      4
     /
    5
```

Node 4

```text
Left Height = 1
Right Height = 0

Difference = 1
```

Balanced ✅

Node 2

```text
Left Height = 2
Right Height = 0

Difference = 2
```

Unbalanced ❌

Since one node is unbalanced,

the entire tree is unbalanced.

---

# 📌 Key Observation

To know whether a node is balanced,

we need:

- Height of the left subtree
    
- Height of the right subtree
    

Then,

```text
Difference = |Left Height - Right Height|
```

If

```text
Difference > 1
```

Tree is not balanced.

---

# 📌 Why Height is Needed?

Without knowing the subtree heights,

we cannot compare them.

So this problem is another extension of the **Height** problem.

---

# 📌 Brute Force Idea

For every node:

1. Compute left height.
    
2. Compute right height.
    
3. Check the difference.
    
4. Recursively check left subtree.
    
5. Recursively check right subtree.
    

Problem?

The same heights are calculated repeatedly.

Time Complexity:

```text
O(n²)
```

---

# 📌 Optimal Idea

During one DFS:

- Compute the height.
    
- If any subtree is unbalanced, stop and propagate that information upward.
    

One traversal solves everything.

---

# 📌 Trick Used in Interviews

Instead of returning only the height,

the recursive function returns:

- Height if the subtree is balanced.
    
- **-1** if the subtree is unbalanced.
    

Why `-1`?

Because a height can never be negative.

So `-1` becomes a special signal meaning:

```text
This subtree is already unbalanced.
```

This avoids checking the same subtree again.

---

# 📌 Recursive Logic

Suppose you're at a node.

1. Get the left height.
    
2. If left returns `-1`, immediately return `-1`.
    
3. Get the right height.
    
4. If right returns `-1`, immediately return `-1`.
    
5. If the height difference is greater than 1, return `-1`.
    
6. Otherwise, return the current height.
    

---

# 📌 Dry Run

Tree

```text
        1
       /
      2
     /
    3
```

Node 3

```text
Height = 1
```

↓

Node 2

```text
Left Height = 1
Right Height = 0

Difference = 1

Return Height = 2
```

↓

Node 1

```text
Left Height = 2
Right Height = 0

Difference = 2

Return -1
```

Final Answer

```text
False
```

---

# 📌 Why Postorder DFS?

Can we decide if a node is balanced before visiting its children?

❌ No.

We first need:

```text
Height(left)

Height(right)
```

Then we compare them.

Information flows:

```text
Leaves

↑

Parent

↑

Root
```

Again, this is **Postorder DFS**.

---

# 📌 Complexity

### Brute Force

```text
Time  : O(n²)

Space : O(h)
```

### Optimal

```text
Time  : O(n)

Space : O(h)
```

---

# 📌 Common Mistakes

### ❌ Checking only the root

Every node must satisfy the balance condition.

---

### ❌ Computing heights repeatedly

This leads to O(n²).

---

### ❌ Forgetting to stop early

If a subtree is already unbalanced, return `-1` immediately instead of continuing.

---

# 📌 Pattern Recognition

Whenever you hear:

- Height Balanced
    
- AVL-like condition
    
- Difference between subtree heights
    

Think:

```text
Return Height

OR

Return -1 if unbalanced
```

---

# 📌 Interview Tips

Ask yourself:

> **"What should recursion return?"**

Answer:

```text
Height
```

But if something goes wrong,

return a special value:

```text
-1
```

This is called a **sentinel value**—a special value that signals an exceptional condition.

---

# 📌 Revision Box

```text
Return

Height

OR

-1 (Unbalanced)
```

```text
Check

|Left Height - Right Height| ≤ 1
```

```text
Traversal

Postorder DFS
```

```text
Time : O(n)

Space : O(h)
```

---

# 🎯 Connection with Previous Problems

Notice the progression:

|Problem|Recursion Returns|Extra Work|
|---|---|---|
|Height|Height|None|
|Diameter|Height|Update global diameter|
|Balanced Tree|Height or `-1`|Check height difference|

The **same Height DFS template** is reused again. The only thing that changes is **what extra information is computed or returned**.

➡️ **Next:** **Balanced Binary Tree — Code (Brute Force + Optimal O(n))**