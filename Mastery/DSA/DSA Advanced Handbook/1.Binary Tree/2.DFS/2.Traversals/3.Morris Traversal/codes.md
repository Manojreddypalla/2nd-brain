# 🌳 Morris Traversal — Codes

---

# 1️⃣ Morris Inorder Traversal

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
        TreeNode* curr = root;

        while (curr) {

            // Case 1: No left child
            if (curr->left == nullptr) {
                ans.push_back(curr->val);
                curr = curr->right;
            }

            // Case 2: Left child exists
            else {

                // Find inorder predecessor
                TreeNode* pred = curr->left;

                while (pred->right && pred->right != curr)
                    pred = pred->right;

                // Create thread
                if (pred->right == nullptr) {
                    pred->right = curr;
                    curr = curr->left;
                }

                // Thread already exists
                else {
                    pred->right = nullptr;      // Remove thread
                    ans.push_back(curr->val);   // Visit node
                    curr = curr->right;
                }
            }
        }

        return ans;
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
     / \
    4   5
```

Steps

```text
1

↓

Create Thread 5 → 1

↓

2

↓

Create Thread 4 → 2

↓

4

↓

Visit 4

↓

Return to 2

↓

Visit 2

↓

5

↓

Visit 5

↓

Return to 1

↓

Visit 1

↓

3

↓

Visit 3
```

Output

```text
4 2 5 1 3
```

---

# 2️⃣ Morris Preorder Traversal

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
        TreeNode* curr = root;

        while (curr) {

            // Case 1: No left child
            if (curr->left == nullptr) {
                ans.push_back(curr->val);
                curr = curr->right;
            }

            // Case 2: Left child exists
            else {

                // Find predecessor
                TreeNode* pred = curr->left;

                while (pred->right && pred->right != curr)
                    pred = pred->right;

                // Create thread
                if (pred->right == nullptr) {

                    ans.push_back(curr->val);   // Visit before thread

                    pred->right = curr;
                    curr = curr->left;
                }

                // Thread exists
                else {

                    pred->right = nullptr;      // Remove thread
                    curr = curr->right;
                }
            }
        }

        return ans;
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
     / \
    4   5
```

Steps

```text
Visit 1

↓

Create Thread

↓

Visit 2

↓

Create Thread

↓

Visit 4

↓

Return

↓

Visit 5

↓

Return

↓

Visit 3
```

Output

```text
1 2 4 5 3
```

---

# 🌳 Comparison

|Traversal|Visit Node|Create Thread|Remove Thread|
|---|---|---|---|
|Morris Inorder|After removing thread|Yes|Yes|
|Morris Preorder|Before creating thread|Yes|Yes|

---

# 🌳 Cheat Sheet

```text
MORRIS INORDER
--------------
No Left
    Visit
    Right

Left Exists
    Find Predecessor

    Thread == NULL
        Create Thread
        Go Left

    Thread Exists
        Remove Thread
        Visit
        Go Right
```

```text
MORRIS PREORDER
---------------
No Left
    Visit
    Right

Left Exists
    Find Predecessor

    Thread == NULL
        Visit
        Create Thread
        Go Left

    Thread Exists
        Remove Thread
        Go Right
```

---

# 🌳 Memory Trick

```text
Recursive
---------
Call Stack

Iterative
---------
Stack

Morris
-------
Temporary Thread
```

### ⭐ The Only Difference to Remember

```text
Morris Inorder
--------------
Create Thread
↓
Visit Later
↓
Remove Thread

Morris Preorder
---------------
Visit First
↓
Create Thread
↓
Remove Thread
```

If you remember **when to visit the current node**, you've essentially remembered the entire Morris Traversal algorithm.