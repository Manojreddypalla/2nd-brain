# 🌳 Iterative Traversals — Codes

---

# 1️⃣ Generic Iterative DFS Template

```cpp
stack<TreeNode*> st;

st.push(root);

while (!st.empty()) {

    TreeNode* node = st.top();
    st.pop();

    // Process node

    // Push children
}
```

---

# 🌳 2️⃣ Iterative Preorder Traversal (1 Stack)

### Order

```text
Root → Left → Right
```

### Code

```cpp
class Solution {
public:
    vector<int> preorderTraversal(TreeNode* root) {

        vector<int> ans;

        if (root == nullptr)
            return ans;

        stack<TreeNode*> st;
        st.push(root);

        while (!st.empty()) {

            TreeNode* node = st.top();
            st.pop();

            ans.push_back(node->val);

            if (node->right)
                st.push(node->right);

            if (node->left)
                st.push(node->left);
        }

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

Stack

```text
[1]

↓

Pop 1

Push 3

Push 2

↓

Pop 2

Push 5

Push 4

↓

Pop 4

↓

Pop 5

↓

Pop 3
```

Output

```text
1 2 4 5 3
```

---

# 🌳 3️⃣ Iterative Inorder Traversal (1 Stack)

### Order

```text
Left → Root → Right
```

### Code

```cpp
class Solution {
public:
    vector<int> inorderTraversal(TreeNode* root) {

        vector<int> ans;
        stack<TreeNode*> st;

        TreeNode* curr = root;

        while (curr || !st.empty()) {

            while (curr) {
                st.push(curr);
                curr = curr->left;
            }

            curr = st.top();
            st.pop();

            ans.push_back(curr->val);

            curr = curr->right;
        }

        return ans;
    }
};
```

### Dry Run

Tree

```text
        1
       /
      2
     /
    3
```

Stack

```text
Push 1

↓

Push 2

↓

Push 3

↓

Pop 3

↓

Pop 2

↓

Pop 1
```

Output

```text
3 2 1
```

---

# 🌳 4️⃣ Iterative Postorder Traversal (2 Stacks)

### Order

```text
Left → Right → Root
```

### Code

```cpp
class Solution {
public:
    vector<int> postorderTraversal(TreeNode* root) {

        vector<int> ans;

        if (root == nullptr)
            return ans;

        stack<TreeNode*> st1, st2;

        st1.push(root);

        while (!st1.empty()) {

            TreeNode* node = st1.top();
            st1.pop();

            st2.push(node);

            if (node->left)
                st1.push(node->left);

            if (node->right)
                st1.push(node->right);
        }

        while (!st2.empty()) {

            ans.push_back(st2.top()->val);
            st2.pop();
        }

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
```

Stack 1

```text
1

↓

2 3
```

Stack 2

```text
1

↓

3

↓

2
```

Output

```text
2 3 1
```

---

# 🌳 5️⃣ Iterative Postorder Traversal (1 Stack)

### Order

```text
Left → Right → Root
```

### Code

```cpp
class Solution {
public:
    vector<int> postorderTraversal(TreeNode* root) {

        vector<int> ans;
        stack<TreeNode*> st;

        TreeNode* curr = root;
        TreeNode* lastVisited = nullptr;

        while (curr || !st.empty()) {

            if (curr) {

                st.push(curr);
                curr = curr->left;

            } else {

                TreeNode* node = st.top();

                if (node->right && lastVisited != node->right) {

                    curr = node->right;

                } else {

                    ans.push_back(node->val);
                    lastVisited = node;

                    st.pop();
                }
            }
        }

        return ans;
    }
};
```

### Dry Run

```text
Go Left

↓

Reach NULL

↓

Check Right

↓

Visit Right

↓

Process Parent
```

Output

```text
Left Right Root
```

---

# 🌳 Comparison

|Traversal|Stack Used|Idea|
|---|---|---|
|Preorder|1 Stack|Process → Right → Left|
|Inorder|1 Stack|Go Left → Process → Right|
|Postorder|2 Stacks|Reverse Modified Preorder|
|Postorder|1 Stack|Track `lastVisited`|

---

# 🌳 Interview Pattern

|If asked...|Use|
|---|---|
|Iterative Preorder|1 Stack|
|Iterative Inorder|1 Stack|
|Easy Iterative Postorder|2 Stacks|
|Optimized Iterative Postorder|1 Stack|

---

# 🌳 Cheat Sheet

```text
PREORDER
---------
Process
Push Right
Push Left

INORDER
--------
Go Left
Process
Go Right

POSTORDER (2 STACKS)
--------------------
Root Right Left
Reverse

POSTORDER (1 STACK)
-------------------
Go Left
Check Right
Process Parent
```

### 💡 Interview Tip

- **Preorder:** Remember **"Right first, so Left is processed first."**
    
- **Inorder:** Remember **"Keep going left until you can't."**
    
- **Postorder (2 stacks):** Easy to remember and explain in interviews.
    
- **Postorder (1 stack):** More optimized (`O(h)` auxiliary space) but trickier; use it when specifically asked for a one-stack solution or optimized approach.