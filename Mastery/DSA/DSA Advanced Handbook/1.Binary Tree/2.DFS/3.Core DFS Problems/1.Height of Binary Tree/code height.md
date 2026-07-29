# 🌳 Height of Binary Tree — Code

> **LeetCode 104: Maximum Depth of Binary Tree**

---

# ✅ Approach 1: Recursive DFS (Recommended)

### Idea

Every recursive call returns the **height of its subtree**.

Formula:

```text
Height = 1 + max(Left Height, Right Height)
```

---

### Code

```cpp
class Solution {
public:
    int maxDepth(TreeNode* root) {

        // Base Case
        if (root == nullptr)
            return 0;

        // Height of left subtree
        int leftHeight = maxDepth(root->left);

        // Height of right subtree
        int rightHeight = maxDepth(root->right);

        // Current node height
        return 1 + max(leftHeight, rightHeight);
    }
};
```

---

### Dry Run

Tree

```text
        1
       / \
      2   3
     /
    4
```

### Step 1

```text
maxDepth(4)

Left = 0
Right = 0

Return 1
```

---

### Step 2

```text
maxDepth(2)

Left = 1
Right = 0

Return 2
```

---

### Step 3

```text
maxDepth(3)

Left = 0
Right = 0

Return 1
```

---

### Step 4

```text
maxDepth(1)

Left = 2
Right = 1

Return 3
```

Final Answer

```text
3
```

---

# 📌 Recursive Call Tree

```text
maxDepth(1)
│
├── maxDepth(2)
│   │
│   ├── maxDepth(4)
│   │   ├── 0
│   │   └── 0
│   │
│   └── 0
│
└── maxDepth(3)
    ├── 0
    └── 0
```

Returned values

```text
4 → 1

2 → 2

3 → 1

1 → 3
```

---

# ✅ Complexity

**Time**

```text
O(n)
```

Every node is visited once.

**Space**

```text
O(h)
```

where `h` is the tree height.

Worst case:

```text
O(n)
```

Balanced tree:

```text
O(log n)
```

---

# ✅ Approach 2: Iterative BFS (Level Order)

### Idea

Every level increases the height by **1**.

Count how many levels exist.

---

### Code

```cpp
class Solution {
public:
    int maxDepth(TreeNode* root) {

        if (root == nullptr)
            return 0;

        queue<TreeNode*> q;
        q.push(root);

        int height = 0;

        while (!q.empty()) {

            int size = q.size();

            height++;

            while (size--) {

                TreeNode* node = q.front();
                q.pop();

                if (node->left)
                    q.push(node->left);

                if (node->right)
                    q.push(node->right);
            }
        }

        return height;
    }
};
```

---

### Dry Run

Tree

```text
        1
       / \
      2   3
     /
    4
```

Queue

```text
Level 1

1

Height = 1
```

↓

```text
Level 2

2 3

Height = 2
```

↓

```text
Level 3

4

Height = 3
```

↓

Queue becomes empty.

Answer

```text
3
```

---

# 📌 Comparison

|Approach|Technique|Time|Space|
|---|---|---|---|
|Recursive|DFS|O(n)|O(h)|
|Iterative|BFS|O(n)|O(n)|

---

# 🎯 Interview Tip

If the interviewer asks:

> **"Find the height using DFS."**

Use the **recursive solution**.

If they ask:

> **"Find the height level by level."**

Use the **BFS solution**.

---

# 📝 Pattern Summary

```text
DFS

Return height from children

↓

height = 1 + max(left, right)
```

```text
BFS

Count number of levels

↓

Each level increases height by 1
```

---

# ⭐ Template to Remember

```cpp
int dfs(TreeNode* root) {

    if (root == nullptr)
        return 0;

    int left = dfs(root->left);
    int right = dfs(root->right);

    return 1 + max(left, right);
}
```

This template is the foundation for the next three problems:

- ✅ Diameter of Binary Tree
    
- ✅ Balanced Binary Tree
    
- ✅ Maximum Path Sum
    

The only thing that changes is **what each recursive call returns and what extra information you compute**.