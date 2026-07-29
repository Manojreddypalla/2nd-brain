# 🌳 Balanced Binary Tree — Code

> **LeetCode 110: Balanced Binary Tree**

---

# ✅ Approach 1: Brute Force (O(n²))

## 💡 Idea

For every node:

1. Find the height of the left subtree.
    
2. Find the height of the right subtree.
    
3. Check if the difference is more than `1`.
    
4. Recursively check the left subtree.
    
5. Recursively check the right subtree.
    

The problem is that **height is calculated repeatedly**, making it **O(n²)**.

---

## Code

```cpp
class Solution {
public:

    int height(TreeNode* root) {

        if (root == nullptr)
            return 0;

        int leftHeight = height(root->left);
        int rightHeight = height(root->right);

        return 1 + max(leftHeight, rightHeight);
    }

    bool isBalanced(TreeNode* root) {

        if (root == nullptr)
            return true;

        int leftHeight = height(root->left);
        int rightHeight = height(root->right);

        if (abs(leftHeight - rightHeight) > 1)
            return false;

        return isBalanced(root->left) &&
               isBalanced(root->right);
    }
};
```

---

# 🌳 Dry Run

```text
        1
       /
      2
     /
    3
```

At node **1**

```text
Left Height = 2
Right Height = 0

Difference = 2

Return false
```

Works correctly, but `height()` is called many times.

---

# ❌ Why O(n²)?

Suppose

```text
        1
       /
      2
     /
    3
```

Height of node `3` is calculated

- while checking node `2`
    
- again while checking node `1`
    

The same subtree is traversed repeatedly.

---

# ✅ Approach 2: Optimal DFS (O(n))

## 💡 Idea

Instead of only returning the height,

return

- Height if the subtree is balanced.
    
- `-1` if it is unbalanced.
    

As soon as a subtree becomes unbalanced,

stop immediately.

---

## Flow

```text
Leaf

↓

Return Height

↓

Parent

↓

Check Balance

↓

Return Height

OR

Return -1
```

---

## Code

```cpp
class Solution {
public:

    int dfs(TreeNode* root) {

        if (root == nullptr)
            return 0;

        int leftHeight = dfs(root->left);

        // Left subtree already unbalanced
        if (leftHeight == -1)
            return -1;

        int rightHeight = dfs(root->right);

        // Right subtree already unbalanced
        if (rightHeight == -1)
            return -1;

        // Current node becomes unbalanced
        if (abs(leftHeight - rightHeight) > 1)
            return -1;

        // Return height
        return 1 + max(leftHeight, rightHeight);
    }

    bool isBalanced(TreeNode* root) {

        return dfs(root) != -1;
    }
};
```

---

# 🌳 Dry Run

```text
        1
       /
      2
     /
    3
```

---

### Node 3

```text
Left = 0
Right = 0

Difference = 0

Return 1
```

---

### Node 2

```text
Left = 1
Right = 0

Difference = 1

Return 2
```

---

### Node 1

```text
Left = 2
Right = 0

Difference = 2

Return -1
```

---

### Final

```text
dfs(root) = -1

Answer = false
```

---

# 📌 Another Example

```text
        1
       / \
      2   3
     / \
    4   5
```

### Node 4

```text
Return 1
```

### Node 5

```text
Return 1
```

### Node 2

```text
Difference = 0

Return 2
```

### Node 3

```text
Return 1
```

### Node 1

```text
Difference = 1

Return 3
```

Result

```text
Balanced = true
```

---

# 📌 Complexity

|Approach|Time|Space|
|---|---|---|
|Brute Force|O(n²)|O(h)|
|Optimal DFS|O(n)|O(h)|

---

# 🎯 Interview Pattern

This problem introduces an important DFS pattern:

```text
Recursion returns one of two things:

1. A valid answer (Height)

OR

2. A special value (-1) indicating failure
```

This special value is called a **sentinel value**.

---

# 📝 Cheat Sheet

```text
Return
------
Height

OR

-1 (Unbalanced)

Condition
---------
abs(leftHeight - rightHeight) > 1

Return
------
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

# ⭐ Connection with Previous Problems

Notice how all three problems use almost the **same DFS skeleton**:

### 1. Height

```cpp
int left = dfs(root->left);
int right = dfs(root->right);

return 1 + max(left, right);
```

---

### 2. Diameter

```cpp
int left = dfs(root->left);
int right = dfs(root->right);

diameter = max(diameter, left + right);

return 1 + max(left, right);
```

---

### 3. Balanced Tree

```cpp
int left = dfs(root->left);
if (left == -1) return -1;

int right = dfs(root->right);
if (right == -1) return -1;

if (abs(left - right) > 1)
    return -1;

return 1 + max(left, right);
```

## 🔥 Pattern to remember

Almost every DFS tree problem follows this template:

```cpp
int dfs(TreeNode* root) {

    if (root == nullptr)
        return BASE_VALUE;

    int left = dfs(root->left);
    int right = dfs(root->right);

    // Do problem-specific work here

    return SOME_VALUE;
}
```

The only thing that changes from one problem to another is the **"problem-specific work"** between computing the left/right results and returning the value. This template will keep appearing in upcoming problems like **Maximum Path Sum**, **Largest BST**, and many tree DP questions.