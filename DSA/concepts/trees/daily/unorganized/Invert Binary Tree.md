==my note it is a easy level question which just goes to each node then access the left and right nodes to swap them then using recursion we again call the invert function to do the same for child nodes until node becomes null==

![[Pasted image 20260610153208.png]]

```cpp
class Solution {

public:

    void invert(TreeNode *root){

        if(root==nullptr)

        {

            return;

        }

        swap(root->left,root->right);

        invert(root->right);

        invert(root->left);

    }

  

    TreeNode* invertTree(TreeNode* root) {

        invert(root);

        return root;

    }

};
```