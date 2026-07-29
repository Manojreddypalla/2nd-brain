# 🌳 Diameter of Binary Tree — Code

> **LeetCode 543: Diameter of Binary Tree**

---

# ✅ Approach 1: Brute Force (O(n²))

### Idea

For every node:

1. Find the height of the left subtree.
    
2. Find the height of the right subtree.
    
3. Compute the diameter through the current node.
    
4. Recursively compute the diameter of the left subtree.
    
5. Recursively compute the diameter of the right subtree.
    
6. Return the maximum.
    

Since height is recomputed many times, this approach is **O(n²)**.

---

### Code

```cpp
class Solution {
public:
    int height(TreeNode* root) {

        if (root == nullptr)
            return 0;

        return 1 + max(height(root->left), height(root->right));
    }

    int diameterOfBinaryTree(TreeNode* root) {

        if (root == nullptr)
            return 0;

        int leftHeight = height(root->left);
        int rightHeight = height(root->right);

        int currentDiameter = leftHeight + rightHeight;

        int leftDiameter = diameterOfBinaryTree(root->left);
        int rightDiameter = diameterOfBinaryTree(root->right);

        return max(currentDiameter, max(leftDiameter, rightDiameter));
    }
};
```

---

# ❌ Why is this Slow?

Example

```text
        1
       / \
      2   3
     /
    4
```

Height of node `2` is computed:

- While solving node `1`
    
- Again while solving node `2`
    

Many subtree heights are recomputed.

Hence,

```text
Time = O(n²)
```

---

# ✅ Approach 2: Optimal DFS (O(n))

## Idea

While calculating the height,

also update the diameter.

This avoids recomputing heights.

---

### Code

```cpp
class Solution {
public:

    int diameter = 0;

    int height(TreeNode* root) {

        if (root == nullptr)
            return 0;

        int leftHeight = height(root->left);
        int rightHeight = height(root->right);

        // Update diameter
        diameter = max(diameter, leftHeight + rightHeight);

        // Return height
        return 1 + max(leftHeight, rightHeight);
    }

    int diameterOfBinaryTree(TreeNode* root) {

        height(root);

        return diameter;
    }
};
```

---

# 🌳 Dry Run

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
Left Height = 0
Right Height = 0

Diameter = 0

Return Height = 1
```

---

### Node 5

```text
Left Height = 0
Right Height = 0

Diameter = 0

Return Height = 1
```

---

### Node 2

```text
Left Height = 1
Right Height = 1

Diameter = 2

Return Height = 2
```

Current Answer

```text
2
```

---

### Node 3

```text
Left Height = 0
Right Height = 0

Diameter = 0

Return Height = 1
```

---

### Node 1

```text
Left Height = 2
Right Height = 1

Diameter = 3

Return Height = 3
```

Final Answer

```text
3
```

---

# 📌 Recursive Call Tree

```text
height(1)
│
├── height(2)
│   │
│   ├── height(4)
│   │   └── returns 1
│   │
│   └── height(5)
│       └── returns 1
│
│   Update Diameter = 2
│   Return Height = 2
│
└── height(3)
    └── returns 1

Update Diameter = 3
Return Height = 3
```

---

# 📌 Complexity

|Approach|Time|Space|
|---|---|---|
|Brute Force|O(n²)|O(h)|
|Optimal DFS|O(n)|O(h)|

---

# 🎯 Interview Pattern

The recursive function returns:

```text
Height
```

The global variable stores:

```text
Maximum Diameter
```

This is the pattern:

```cpp
int left = dfs(root->left);
int right = dfs(root->right);

answer = max(answer, left + right);

return 1 + max(left, right);
```

---

# 📝 Cheat Sheet

```text
Return
------
Height

Update
------
Diameter = leftHeight + rightHeight

Return Height
-------------
1 + max(leftHeight, rightHeight)

Traversal
---------
Postorder DFS

Time
----
O(n)

Space
-----
O(h)
```

---

# ⭐ Connection with Height

Compare the **Height** solution:

```cpp
return 1 + max(leftHeight, rightHeight);
```

Now compare **Diameter**:

```cpp
diameter = max(diameter, leftHeight + rightHeight);

return 1 + max(leftHeight, rightHeight);
```

👉 **Only one extra line is added!**

This is why Diameter is considered an extension of the Height problem. Once you understand Height, Diameter becomes straightforward.