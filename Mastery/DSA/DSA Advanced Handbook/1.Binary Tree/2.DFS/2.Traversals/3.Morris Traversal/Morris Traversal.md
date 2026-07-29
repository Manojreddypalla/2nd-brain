# 🌳 DFS — Morris Traversal (Theory + Notes)

---

# 📌 What is Morris Traversal?

**Morris Traversal** is a **DFS traversal technique** that performs **Inorder** or **Preorder** traversal **without using recursion or a stack**.

Instead, it temporarily modifies the tree by creating **threads (temporary links)** between nodes.

> **Key Idea:** Reuse the tree's NULL pointers instead of using extra memory.

---

# 📌 Why do we need Morris Traversal?

Normally,

* Recursive DFS → uses **Call Stack**
* Iterative DFS → uses **Stack**

Both require extra memory.

Morris Traversal removes this extra space requirement.

| Traversal | Extra Space |
| --------- | ----------- |
| Recursive | O(h)        |
| Iterative | O(h)        |
| Morris    | **O(1)**    |

where `h` is the height of the tree.

---

# 📌 Core Idea

Whenever a node has a **left subtree**, we need a way to come back after finishing it.

Instead of storing the parent in a stack,

we temporarily connect the **rightmost node of the left subtree (predecessor)** back to the current node.

This temporary connection is called a **Thread**.

---

# 📌 What is a Thread?

Example Tree

```text
        1
       / \
      2   3
     / \
    4   5
```

Normally

```text
5 -> NULL
```

Morris temporarily changes it to

```text
5 -----> 1
```

This lets us return to node `1` after finishing its left subtree.

After returning,

the thread is removed.

The original tree remains unchanged.

---

# 📌 Predecessor

The predecessor of a node in Morris Traversal is:

> **The rightmost node in its left subtree.**

Example

```text
        10
       /
      5
     / \
    2   8
         \
          9
```

For node `10`

Predecessor is

```text
9
```

because it is the rightmost node of the left subtree.

---

# 📌 Morris Traversal Algorithm

For every node:

### Case 1

No left child

```text
Visit node

↓

Move Right
```

---

### Case 2

Left child exists

Find predecessor.

---

If predecessor's right is NULL

```text
Create Thread

↓

Go Left
```

---

If predecessor already points to current

```text
Remove Thread

↓

Visit Current

↓

Go Right
```

---

# 📌 Why does it work?

Without recursion,

we would normally lose the path back to the parent.

The temporary thread acts like a bookmark.

Instead of storing the parent in memory,

the tree itself remembers where to return.

---

# 📌 Morris Inorder

Traversal Order

```text
Left

↓

Root

↓

Right
```

Visit node **after** removing the thread.

---

# 📌 Morris Preorder

Traversal Order

```text
Root

↓

Left

↓

Right
```

Visit node **before** creating the thread.

---

# 📌 Morris Postorder

There is **no commonly used Morris Postorder**.

It exists but is much more complicated and is rarely asked in interviews.

Most interviews only expect:

* Morris Inorder
* Morris Preorder

---

# 📌 Example

Tree

```text
        1
       / \
      2   3
     / \
    4   5
```

---

### Morris Inorder

Output

```text
4 2 5 1 3
```

---

### Morris Preorder

Output

```text
1 2 4 5 3
```

---

# 📌 Time Complexity

Each edge is traversed at most twice.

```text
O(n)
```

---

# 📌 Space Complexity

```text
O(1)
```

No recursion.

No stack.

---

# 📌 Advantages

* Constant extra memory.
* No recursion.
* No explicit stack.
* Very useful for memory-constrained environments.
* Common interview optimization question.

---

# 📌 Disadvantages

* Temporarily modifies the tree.
* Harder to understand than recursion.
* Easy to make mistakes while creating/removing threads.
* Mostly limited to Inorder and Preorder.

---

# 📌 Common Mistakes

### ❌ Forgetting to remove the thread

The tree remains modified, leading to incorrect traversals or infinite loops.

---

### ❌ Visiting the node at the wrong time

* **Inorder:** Visit after removing the thread.
* **Preorder:** Visit before creating the thread.

---

### ❌ Not checking whether the thread already exists

Always distinguish between:

```text
predecessor->right == NULL
```

and

```text
predecessor->right == current
```

---

# 📌 Pattern Recognition

| If the question asks...  | Use              |
| ------------------------ | ---------------- |
| Inorder with O(1) space  | Morris Inorder   |
| Preorder with O(1) space | Morris Preorder  |
| No recursion allowed     | Morris Traversal |
| No stack allowed         | Morris Traversal |

---

# 📌 Interview Tips

Think of Morris Traversal as:

```text
Recursion
      ↓
Remember parent using Call Stack

Iteration
      ↓
Remember parent using Stack

Morris
      ↓
Remember parent using Temporary Thread
```

---

# 📌 Revision Box

```text
Recursive DFS
↓

Call Stack

Space O(h)
```

```text
Iterative DFS
↓

Stack

Space O(h)
```

```text
Morris DFS
↓

Temporary Threads

Space O(1)
```

```text
Inorder
Visit After Removing Thread
```

```text
Preorder
Visit Before Creating Thread
```

---

## ✅ Next Topic

We'll cover the **Morris Traversal Codes**:

1. Morris Inorder
2. Morris Preorder
3. Dry runs
4. Interview tricks to remember exactly **when to visit** and **when to create/remove threads**.
