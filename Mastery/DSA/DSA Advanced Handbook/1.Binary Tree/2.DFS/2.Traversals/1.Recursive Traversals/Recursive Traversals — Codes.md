# 🌳 Recursive Traversals — Codes

---

# 1️⃣ Generic DFS Template

This is the foundation of every recursive tree problem.

```cpp
void dfs(TreeNode* root) {

    if (root == nullptr)
        return;

    // Preorder Work

    dfs(root->left);

    // Inorder Work

    dfs(root->right);

    // Postorder Work
}
```

---

# 🌳 2️⃣ Recursive Preorder Traversal

### Traversal Order

```text
Root → Left → Right
```

### Code

```cpp
class Solution {
public:
    vector<int> ans;

    void dfs(TreeNode* root) {

        if (root == nullptr)
            return;

        ans.push_back(root->val);

        dfs(root->left);
        dfs(root->right);
    }

    vector<int> preorderTraversal(TreeNode* root) {

        dfs(root);

        return ans;
    }
};
```

### Dry Run

Tree

```text
        1
       / \
      2   3
     / \
    4   5
```

Visit

```text
1

↓

2

↓

4

↓

5

↓

3
```

Output

```text
1 2 4 5 3
```

---

# 🌳 3️⃣ Recursive Inorder Traversal

### Traversal Order

```text
Left → Root → Right
```

### Code

```cpp
class Solution {
public:
    vector<int> ans;

    void dfs(TreeNode* root) {

        if (root == nullptr)
            return;

        dfs(root->left);

        ans.push_back(root->val);

        dfs(root->right);
    }

    vector<int> inorderTraversal(TreeNode* root) {

        dfs(root);

        return ans;
    }
};
```

### Dry Run

Tree

```text
        1
       / \
      2   3
     / \
    4   5
```

Visit

```text
4

↓

2

↓

5

↓

1

↓

3
```

Output

```text
4 2 5 1 3
```

---

# 🌳 4️⃣ Recursive Postorder Traversal

### Traversal Order

```text
Left → Right → Root
```

### Code

```cpp
class Solution {
public:
    vector<int> ans;

    void dfs(TreeNode* root) {

        if (root == nullptr)
            return;

        dfs(root->left);

        dfs(root->right);

        ans.push_back(root->val);
    }

    vector<int> postorderTraversal(TreeNode* root) {

        dfs(root);

        return ans;
    }
};
```

### Dry Run

Tree

```text
        1
       / \
      2   3
     / \
    4   5
```

Visit

```text
4

↓

5

↓

2

↓

3

↓

1
```

Output

```text
4 5 2 3 1
```

---

# 🌳 5️⃣ Parameter Passing Version (Without Global Variable)

Many interviewers prefer avoiding global variables.

---

### Preorder

```cpp
void preorder(TreeNode* root, vector<int>& ans) {

    if (root == nullptr)
        return;

    ans.push_back(root->val);

    preorder(root->left, ans);
    preorder(root->right, ans);
}
```

---

### Inorder

```cpp
void inorder(TreeNode* root, vector<int>& ans) {

    if (root == nullptr)
        return;

    inorder(root->left, ans);

    ans.push_back(root->val);

    inorder(root->right, ans);
}
```

---

### Postorder

```cpp
void postorder(TreeNode* root, vector<int>& ans) {

    if (root == nullptr)
        return;

    postorder(root->left, ans);

    postorder(root->right, ans);

    ans.push_back(root->val);
}
```

---

# 🌳 Traversal Comparison

|Traversal|Processing Position|Order|
|---|---|---|
|Preorder|Before recursive calls|Root → Left → Right|
|Inorder|Between recursive calls|Left → Root → Right|
|Postorder|After recursive calls|Left → Right → Root|

---

# 🌳 Pattern Summary

```text
Preorder

Process

↓

Left

↓

Right
```

```text
Inorder

Left

↓

Process

↓

Right
```

```text
Postorder

Left

↓

Right

↓

Process
```

---

# 🌳 Interview Tip

You only need to remember **one template**:

```cpp
void dfs(TreeNode* root) {

    if (root == nullptr)
        return;

    // Preorder

    dfs(root->left);

    // Inorder

    dfs(root->right);

    // Postorder
}
```

Move your **processing statement** (e.g., `ans.push_back(root->val)`) to:

- **Before** recursive calls → **Preorder**
    
- **Between** recursive calls → **Inorder**
    
- **After** recursive calls → **Postorder**
    

This single template is the basis for almost every recursive binary tree problem.