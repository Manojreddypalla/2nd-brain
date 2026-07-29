# 🌳 DFS — Recursive Traversals (Theory + Notes)

---

# 📌 What is Recursive Traversal?

A **Recursive Traversal** is a **Depth First Search (DFS)** technique in which a function **calls itself** to visit every node of a binary tree.

Instead of maintaining a stack manually, recursion uses the **system's call stack** to remember where to return after exploring a subtree.

---

# 📌 Why is it called DFS?

DFS stands for **Depth First Search**.

The algorithm explores **one branch completely** before moving to another branch.

Example:

```text
        1
       / \
      2   3
     / \
    4   5
```

DFS visits

```text
1

↓

2

↓

4

↑

2

↓

5

↑

1

↓

3
```

Notice it goes **deep first**, then backtracks.

---

# 📌 Why use Recursion?

Every subtree of a binary tree is itself a binary tree.

Therefore, the same function can solve the left subtree and the right subtree.

Instead of writing separate logic for every level, recursion naturally repeats the same process.

---

# 📌 How Recursion Works

Consider

```text
        1
       /
      2
     /
    3
```

Calling

```cpp
dfs(root);
```

creates the following function calls.

```text
dfs(1)

↓

dfs(2)

↓

dfs(3)

↓

dfs(NULL)

↓

Return

↓

dfs(NULL)

↓

Return

↓

Return
```

The compiler stores these calls inside the **Call Stack**.

---

# 📌 System Call Stack

Every recursive call is pushed into memory.

```text
Top

dfs(3)

dfs(2)

dfs(1)

Bottom
```

When a function finishes,

it is removed from the stack.

This process is called **Backtracking**.

---

# 📌 General Recursive Template

Every recursive traversal follows the same template.

```cpp
void dfs(TreeNode* root){

    if(root == nullptr)
        return;

    // Work

    dfs(root->left);

    // Work

    dfs(root->right);

    // Work
}
```

The only difference is **where you perform the work**.

---

# 📌 Types of Recursive Traversals

There are **three recursive traversals**.

|Traversal|Order|
|---|---|
|Preorder|Root → Left → Right|
|Inorder|Left → Root → Right|
|Postorder|Left → Right → Root|

The traversal changes only by changing **when the current node is processed**.

---

# 🌳 Preorder Traversal

## Order

```text
Root

↓

Left

↓

Right
```

## Idea

Visit the current node **before** exploring its children.

Think:

> **"Parent First."**

## Applications

- Copy Tree
    
- Serialize Tree
    
- Prefix Expression
    
- Build Tree
    

---

# 🌳 Inorder Traversal

## Order

```text
Left

↓

Root

↓

Right
```

## Idea

Completely finish the left subtree,

then visit the current node,

then explore the right subtree.

Think:

> **"Middle First."**

## Applications

- BST Traversal
    
- Validate BST
    
- Kth Smallest
    
- Sorted Order in BST
    

---

# 🌳 Postorder Traversal

## Order

```text
Left

↓

Right

↓

Root
```

## Idea

Finish both children before processing the parent.

Think:

> **"Children First."**

## Applications

- Height
    
- Diameter
    
- Balanced Tree
    
- Delete Tree
    
- Maximum Path Sum
    
- Tree DP
    

---

# 📌 Example

```text
        1
      /   \
     2     3
    / \
   4   5
```

### Preorder

```text
1 2 4 5 3
```

---

### Inorder

```text
4 2 5 1 3
```

---

### Postorder

```text
4 5 2 3 1
```

---

# 📌 Time Complexity

Every node is visited exactly once.

```text
Time = O(n)
```

---

# 📌 Space Complexity

The recursive calls occupy memory proportional to the tree height.

```text
Space = O(h)
```

where

```text
h = Height of Tree
```

Worst Case (Skewed Tree)

```text
O(n)
```

Balanced Tree

```text
O(log n)
```

---

# 📌 Common Mistakes

### ❌ Forgetting the Base Case

Always write

```cpp
if(root == nullptr)
    return;
```

Without it, recursion never stops.

---

### ❌ Thinking Inorder Always Gives Sorted Output

Only **Binary Search Trees (BSTs)** produce sorted output during inorder traversal.

---

### ❌ Using Preorder for Height or Diameter

Height and Diameter require information from child nodes.

Use **Postorder** because children are processed first.

---

### ❌ Forgetting Recursion Uses Memory

Recursive DFS uses the system call stack.

Very deep trees may cause **Stack Overflow**.

---

# 📌 Pattern Recognition

|If the question says...|Use|
|---|---|
|Process Parent First|Preorder|
|BST / Sorted Order|Inorder|
|Need Child Results First|Postorder|

---

# 📌 Interview Tips

When solving a tree problem, ask yourself:

### Should I process the current node before its children?

➡️ **Preorder**

---

### Should I process the current node between its children?

➡️ **Inorder**

---

### Should I process the current node after both children?

➡️ **Postorder**

---

# 📌 Revision Box

```text
DFS
│
├── Preorder
│      Root Left Right
│
├── Inorder
│      Left Root Right
│
└── Postorder
       Left Right Root
```

```text
Preorder
Parent First
```

```text
Inorder
Root in Middle
```

```text
Postorder
Children First
```

```text
Recursive DFS
↓

Uses System Call Stack
```

---

## ✅ Next Topic

The theory is complete. The next step is **Recursive Traversal Codes**, where we'll cover:

- Preorder (Recursive)
    
- Inorder (Recursive)
    
- Postorder (Recursive)
    
- Generic DFS template
    
- Interview patterns for choosing the right traversal
    

without repeating the theory.