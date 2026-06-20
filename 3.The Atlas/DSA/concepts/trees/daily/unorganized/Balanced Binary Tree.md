
Use recursion to get the height of the left and right subtrees. Compare both heights and if the difference is greater than 1, return false because the current node is not balanced. Then recursively check whether the left subtree and right subtree are balanced too. If every node satisfies this condition, return true.

### Logic Flow

```
Get Left Height
Get Right Height
Check Difference <= 1
Check Left Subtree
Check Right Subtree
Return True if all are balanced
```
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

    int height(TreeNode *root)

    {

        if(root==nullptr)

        {

            return 0;

        }

        int left ,right;

        left = height(root->left);

        right=height(root->right);

        return max(left,right)+1;

  

    }

    bool isBalanced(TreeNode* root)

    {

        if(root==nullptr)

        {

            return true;

        }

        int left,right;

        left=height(root->left);

        right=height(root->right);

        if(abs(left-right)>1){

            return false;

        }

  

        bool lb=isBalanced(root->left);

        bool rb=isBalanced(root->right);

  

        return lb&&rb;

  
  

  
  

    }

};
```





![[Pasted image 20260610153951.png]]



