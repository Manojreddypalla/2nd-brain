==Use recursion to go left and right. Get the depth from both sides and take the maximum of them. This same process happens for every recursive call. Then add +1 for the current node and return it. When the recursion reaches back to the root, the final returned value is the maximum depth of the tree.==


```cpp
class Solution {

public:

    int maxDepth(TreeNode* root) {

        if(root==nullptr)

        {

            return NULL;

        }

  

       int a= maxDepth(root->left);

       int b=maxDepth(root->right);

       return 1+max(a,b);

    }

};
```
