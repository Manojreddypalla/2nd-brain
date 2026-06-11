![[Pasted image 20260610152230.png]]
### Inorder Traversal Notes

- Use **recursion** to visit every node in the tree.
- For each node, first go to the **left subtree**, then process the **current node**, and finally go to the **right subtree**.
- The base case is when the node is `nullptr`; simply return.
- While returning from recursion, store the current node's value in the answer vector.
- Traversal order:
- it also works for pre order and post order too 

```
Left → Root → Right
```

### Short Memory Note

> Use recursion to reach the leftmost node, print/store the current node while coming back, then recursively visit the right subtree.



```cpp
/**

 * Definition for a binary tree node.

 * struct TreeNode {

 *     int val;

 *     TreeNode *left;

 *     TreeNode *right;

 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}

 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}

 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}

 * };

 */

class Solution {

public:

    void inorder(vector<int>&ans,TreeNode* root)

    {

        if(root==nullptr)

        {

            return;

        }

        inorder(ans,root->left);

        ans.push_back(root->val);

        inorder(ans,root->right);

  

    }

    vector<int> inorderTraversal(TreeNode* root) {

        vector<int> ans;

        inorder(ans,root);

        return ans;

  

    }

};
```