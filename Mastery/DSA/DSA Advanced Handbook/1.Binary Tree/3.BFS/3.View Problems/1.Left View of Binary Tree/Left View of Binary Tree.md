Perfect. We'll start with the easiest and build patterns gradually.

---

# 👁️ Problem 1 — Left View of Binary Tree

## Problem Statement

Given the root of a binary tree, return the nodes visible when the tree is viewed **from the left side**.

Example

```text
        1
      /   \
     2     3
    / \     \
   4   5     6
```

Output

```text
1 2 4
```

Because these are the first nodes visible from the left.

---

# Intuition

During **Level Order Traversal**, every level has one node that appears first.

The **first node of every level** is the Left View.

Example

```text
Level 0 : 1

Level 1 : 2 3

Level 2 : 4 5 6
```

Take

```text
1

↓

2

↓

4
```

Done.

---

# Logic

Perform normal BFS.

For every level:

- Find the number of nodes (`size`).
    
- Traverse the level.
    
- If `i == 0`, store the node.
    

Everything else is the normal BFS template.

---

# Algorithm

```text
Push Root

↓

While Queue Not Empty

↓

size = Queue Size

↓

for i = 0 → size-1

↓

Pop Node

↓

If i == 0

Store Node

↓

Push Left

↓

Push Right
```

---

# Dry Run

Tree

```text
        1
      /   \
     2     3
    / \     \
   4   5     6
```

Queue

```text
[1]
```

size = 1

```
i = 0

Answer = [1]
```

Push

```
2 3
```

---

Queue

```text
[2 3]
```

size = 2

```
i = 0

Answer = [1 2]
```

Process

```
3
```

Push

```
4 5 6
```

---

Queue

```text
[4 5 6]
```

size = 3

```
i = 0

Answer = [1 2 4]
```

Finished.

---

# C++ Code (BFS)

```cpp
class Solution {
public:
    vector<int> leftView(TreeNode* root) {

        vector<int> ans;

        if(root == NULL)
            return ans;

        queue<TreeNode*> q;
        q.push(root);

        while(!q.empty()) {

            int size = q.size();

            for(int i = 0; i < size; i++) {

                TreeNode* node = q.front();
                q.pop();

                // First node of current level
                if(i == 0)
                    ans.push_back(node->val);

                if(node->left)
                    q.push(node->left);

                if(node->right)
                    q.push(node->right);
            }
        }

        return ans;
    }
};
```

---

# Complexity

```
Time  : O(n)

Space : O(n)
```

---

# Pattern

Only one line is added to the normal Level Order template.

```cpp
if(i == 0)
    ans.push_back(node->val);
```

Everything else remains exactly the same.

---

# Interview Tip

Whenever the question says:

- Left View
    
- First node of every level
    
- Visible from left
    

Think immediately:

```text
Level Order Traversal

+

Take First Node
```

---

# Alternate DFS Solution (Interview Bonus)

Use **Preorder (Root → Left → Right)**.

If you're visiting a level for the **first time**, store that node.

```cpp
class Solution {
public:
    void dfs(TreeNode* root, int level, vector<int>& ans) {

        if(root == NULL)
            return;

        if(level == ans.size())
            ans.push_back(root->val);

        dfs(root->left, level + 1, ans);
        dfs(root->right, level + 1, ans);
    }

    vector<int> leftView(TreeNode* root) {

        vector<int> ans;
        dfs(root, 0, ans);

        return ans;
    }
};
```

---

# Revision Cheat Sheet

```text
Problem
-------
Return nodes visible from the left.

Observation
-----------
First node of every level.

Logic
-----
Level Order Traversal

Take i == 0

Pattern
-------
Level Processing Pattern

Code Change
-----------
if(i == 0)
    ans.push_back(node->val);

Complexity
----------
Time  : O(n)
Space : O(n)

DFS Alternative
---------------
Preorder
(Root → Left → Right)
Store first node at each level.
```

This is one of the easiest BFS view problems. The **Right View** is almost identical—only a **single condition changes**.