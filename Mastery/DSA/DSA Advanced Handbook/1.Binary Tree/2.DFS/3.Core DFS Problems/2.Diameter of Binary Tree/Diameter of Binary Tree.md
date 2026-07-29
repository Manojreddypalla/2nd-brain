# 🌳 DFS Problem 2 — Diameter of Binary Tree (Theory + Notes)

> **LeetCode 543: Diameter of Binary Tree**

---

# 📌 Problem Statement

Given the root of a binary tree, return the **diameter** of the tree.

The **diameter** is the **length of the longest path between any two nodes**.

> **Important:** The path **does not have to pass through the root**.

---

# 📌 What is Diameter?

Think of the diameter as the **longest possible path** inside the tree.

Example

```text
        1
       / \
      2   3
     / \
    4   5
```

Longest path

```text
4 → 2 → 1 → 3
```

Diameter (in edges)

```text
3
```

---

# 📌 Common Misconception

Many beginners think:

> Diameter = Height

❌ Wrong.

Height means

```text
Root
↓

Deepest Leaf
```

Diameter means

```text
Any Node

↓

Any Node
```

The longest path can start and end anywhere.

---

# 📌 Example 1

```text
        1
       /
      2
     /
    3
```

Height

```text
3
```

Diameter

```text
2 edges
```

Longest path

```text
3 → 2 → 1
```

---

# 📌 Example 2

```text
          1
         /
        2
       /
      3
     /
    4
```

Height

```text
4
```

Diameter

```text
3 edges
```

---

# 📌 Example 3

```text
          1
         / \
        2   3
       /     \
      4       5
```

Height

```text
3
```

Diameter

```text
4 → 2 → 1 → 3 → 5
```

Diameter

```text
4 edges
```

Notice

The longest path is

```text
Leaf → Leaf
```

not

```text
Root → Leaf
```

---

# 📌 Important Observation

Suppose we are standing at a node.

```text
        ?
       / \
```

If we already know

```text
Left Height = 4

Right Height = 2
```

then

the longest path passing through this node is

```text
4 + 2 = 6 edges
```

Why?

```text
Left Height

↓

Current Node

↓

Right Height
```

This gives one possible diameter.

---

# 📌 Can Every Node Be the Center?

Yes.

The diameter may pass through:

- Root
    
- Left subtree
    
- Right subtree
    
- Any internal node
    

Therefore,

we must check **every node**.

---

# 📌 Key Formula

For every node

```text
Diameter Through Current Node

=

Left Height + Right Height
```

Global Answer

```text
Maximum of all

Left Height + Right Height
```

---

# 📌 Why is Height Needed?

To calculate the diameter at a node,

we first need

- Left Height
    
- Right Height
    

Without heights,

we cannot know the longest path through that node.

So,

Diameter is actually built on top of the **Height** problem.

---

# 📌 Why Postorder DFS?

Can we calculate diameter before visiting children?

❌ No.

We first need

```text
Height(left)

Height(right)
```

Only then can we compute

```text
Left Height + Right Height
```

So the information flows

```text
Leaves

↑

Parent

↑

Root
```

This is exactly **Postorder DFS**.

---

# 📌 Dry Run

Tree

```text
        1
       / \
      2   3
     / \
    4   5
```

---

### Node 4

```text
Left = 0

Right = 0

Height = 1

Diameter Through = 0
```

---

### Node 5

```text
Height = 1

Diameter Through = 0
```

---

### Node 2

```text
Left Height = 1

Right Height = 1

Height = 2

Diameter Through = 2
```

Current Best

```text
2
```

---

### Node 3

```text
Height = 1

Diameter Through = 0
```

---

### Root

```text
Left Height = 2

Right Height = 1

Diameter Through = 3
```

Current Best

```text
3
```

Answer

```text
3
```

---

# 📌 Visualization

```text
        1
       / \
      2   3
     / \
    4   5
```

Returned Heights

```text
        3
       / \
      2   1
     / \
    1   1
```

Diameter checked at each node

```text
Node 4 → 0

Node 5 → 0

Node 2 → 2

Node 3 → 0

Node 1 → 3
```

Maximum

```text
3
```

---

# 📌 Brute Force Idea

For every node

1. Compute left height
    
2. Compute right height
    
3. Calculate diameter
    
4. Repeat for every node
    

Problem?

Height is computed again and again.

Time Complexity

```text
O(n²)
```

---

# 📌 Optimal Idea

During one DFS,

return the height,

and simultaneously update the diameter.

So,

one traversal gives

- Height
    
- Diameter
    

Time

```text
O(n)
```

---

# 📌 Time Complexity

```text
O(n)
```

Every node is visited once.

---

# 📌 Space Complexity

```text
O(h)
```

Recursive stack.

Worst

```text
O(n)
```

Balanced

```text
O(log n)
```

---

# 📌 Common Mistakes

### ❌ Returning Diameter Instead of Height

The recursive function should return

```text
Height
```

not

```text
Diameter
```

Diameter is stored separately.

---

### ❌ Forgetting Global Maximum

Every node can produce a larger diameter.

Always update

```text
answer = max(answer, leftHeight + rightHeight)
```

---

### ❌ Confusing Nodes and Edges

LeetCode 543 expects the answer in **edges**.

At a node:

```text
Diameter = Left Height + Right Height
```

not

```text
1 + Left Height + Right Height
```

because heights are measured in **nodes**, but the edge count across the current node is simply the sum of the left and right subtree heights.

---

# 📌 Pattern Recognition

Whenever you hear

- Longest Path
    
- Diameter
    
- Maximum Distance
    

Think immediately

```text
Return Height

+

Update Global Answer
```

---

# 📌 Interview Tips

Ask yourself

> **"What should recursion return?"**

Answer

```text
Height
```

Ask again

> **"What extra information should I maintain?"**

Answer

```text
Maximum Diameter
```

This is a classic DFS pattern:

- **Return one value** (height)
    
- **Update another value** (diameter)
    

---

# 📌 Revision Box

```text
Return

Height
```

```text
Height

=

1 + max(left, right)
```

```text
Diameter

=

leftHeight + rightHeight
```

```text
Update

ans = max(ans, left + right)
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

## 🎯 Key Connection

Notice how **Diameter is just Height with one extra line**.

For Height, you returned:

```cpp
return 1 + max(leftHeight, rightHeight);
```

For Diameter, you'll do the **same return**, but before returning you'll also update:

```cpp
diameter = max(diameter, leftHeight + rightHeight);
```

That's why **Height must be mastered first**—Diameter is simply the next layer built on top of it.

➡️ **Next:** Diameter of Binary Tree — Code (Brute Force + Optimal).