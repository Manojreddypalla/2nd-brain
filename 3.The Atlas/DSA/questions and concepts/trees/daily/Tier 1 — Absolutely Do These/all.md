# Binary Tree Revision Sheet (High ROI)

These 5 problems teach almost every important recursion pattern in trees.

Think of them as:

```
Diameter           -> Collect info from children
Balanced Tree      -> Collect info + fail early
LCA                -> Search and backtrack
Validate BST       -> Pass constraints downward
Kth Smallest BST   -> Use inorder property
```

---

# 1. Diameter of Binary Tree

## Intuition

Diameter = longest path between any two nodes.

The longest path passing through a node is:

```
left height + right height
```

Example:

```
      1
     / \
    2   3
   / \
  4   5
```

At node 2:

```
left height = 1
right height = 1

diameter through 2 = 2
```

At node 1:

```
left height = 2
right height = 1

diameter through 1 = 3
```

Answer = maximum of all such values.

---

## What recursion returns

Return:

```
height
```

Use:

```
leftHeight
rightHeight
```

to update diameter.

---

## Mental Model

Every node asks:

> "How tall is my left subtree?"
> 
> "How tall is my right subtree?"

Then:

```
candidateDiameter =
leftHeight + rightHeight
```

Store maximum.

---

## Skeleton

```cpp
int diameter = 0;

int height(TreeNode* root)
{
    if(!root) return 0;

    int left = height(root->left);
    int right = height(root->right);

    diameter = max(diameter, left + right);

    return max(left, right) + 1;
}
```

---

# 2. Balanced Binary Tree

## Intuition

Tree is balanced if:

```
abs(leftHeight - rightHeight) <= 1
```

for EVERY node.

---

## Brute Force

At every node:

```
calculate left height
calculate right height
```

Again and again.

Cost:

```
O(N²)
```

---

## Optimization

Calculate height while checking balance.

Return:

```
height
```

or

```
-1
```

if subtree already unbalanced.

---

## Mental Model

A subtree tells parent:

```
"I'm balanced and my height is 5"
```

or

```
"I'm broken (-1)"
```

---

## Skeleton

```cpp
int check(TreeNode* root)
{
    if(!root) return 0;

    int left = check(root->left);
    if(left == -1) return -1;

    int right = check(root->right);
    if(right == -1) return -1;

    if(abs(left-right) > 1)
        return -1;

    return max(left,right)+1;
}
```

---

## Pattern Learned

**Early termination**

Don't continue if answer is already impossible.

---

# 3. Lowest Common Ancestor (LCA)

## Intuition

Find first node where:

```
p exists in one side
q exists in another side
```

---

Example

```
        3
       / \
      5   1
     / \
    6   2
```

Find:

```
6 and 2
```

Answer:

```
5
```

---

## What recursion returns

Return:

```
nullptr
```

or

```
p
```

or

```
q
```

or

```
LCA
```

---

## Mental Model

Each subtree reports:

```
Did I find p?
Did I find q?
```

When both come together:

```
Current node = LCA
```

---

## Skeleton

```cpp
TreeNode* lca(TreeNode* root,
              TreeNode* p,
              TreeNode* q)
{
    if(!root || root==p || root==q)
        return root;

    TreeNode* left =
        lca(root->left,p,q);

    TreeNode* right =
        lca(root->right,p,q);

    if(left && right)
        return root;

    return left ? left : right;
}
```

---

## Pattern Learned

### Backtracking Search

Information travels upward.

Children tell parent what they found.

---

# 4. Validate Binary Search Tree

## Intuition

Most common mistake:

Checking only:

```
left < root
right > root
```

is NOT enough.

---

Bad Example

```
      10
     /  \
    5    15
        /
       6
```

Local checks pass.

But:

```
6 < 10
```

so tree is invalid.

---

## Correct Idea

Every node receives:

```
minimum allowed value
maximum allowed value
```

---

Example

```
root = 10

left subtree:
(-∞ , 10)

right subtree:
(10 , +∞)
```

Ranges keep shrinking.

---

## Mental Model

Parent says:

```
You may only live inside this range.
```

Children inherit tighter ranges.

---

## Skeleton

```cpp
bool valid(TreeNode* root,
           long long low,
           long long high)
{
    if(!root)
        return true;

    if(root->val <= low ||
       root->val >= high)
        return false;

    return valid(root->left,
                 low,
                 root->val)
        &&
           valid(root->right,
                 root->val,
                 high);
}
```

---

## Pattern Learned

### Constraint Propagation

Rules flow downward.

---

# 5. Kth Smallest Element in BST

## Key BST Property

Inorder traversal of BST:

```
Left
Root
Right
```

gives sorted order.

---

Example

```
      5
     / \
    3   7
   / \
  2   4
```

Inorder:

```
2 3 4 5 7
```

Sorted!

---

## Intuition

Visit nodes in inorder.

Count:

```
1st
2nd
3rd
...
kth
```

When count becomes k:

```
answer found
```

---

## Mental Model

Imagine BST is secretly an already sorted array.

Inorder traversal simply reveals elements one by one.

---

## Skeleton

```cpp
int cnt = 0;
int ans = 0;

void inorder(TreeNode* root,int k)
{
    if(!root) return;

    inorder(root->left,k);

    cnt++;

    if(cnt==k)
    {
        ans=root->val;
        return;
    }

    inorder(root->right,k);
}
```

---

# Pattern Recognition Table

|Problem|Recursion Returns|Main Idea|
|---|---|---|
|Diameter|Height|Use child information|
|Balanced Tree|Height / -1|Early termination|
|LCA|Node pointer|Search + backtracking|
|Validate BST|Bool|Pass ranges downward|
|Kth Smallest|Nothing (count)|Inorder = sorted|

---

# The 5 Core Tree Recursion Patterns

Almost every tree problem is one of these:

### 1. Information Upward

```
child → parent
```

Examples:

- Diameter
    
- Max Depth
    
- Balanced Tree
    
- Path Sum
    

---

### 2. Constraints Downward

```
parent → child
```

Examples:

- Validate BST
    
- Path Sum
    
- Tree Coloring
    

---

### 3. Search + Backtracking

```
go down
find
return upward
```

Examples:

- LCA
    
- Path to Node
    
- Distance K
    

---

### 4. Traversal Order

```
Preorder
Inorder
Postorder
```

Examples:

- Kth Smallest
    
- BST problems
    
- Serialization
    

---

### 5. Tree DP

Each node combines answers from children.

Examples:

- Diameter
    
- Maximum Path Sum
    
- House Robber III
    
- Longest Univalue Path
    

If you deeply understand these 5 problems, you'll recognize the pattern behind a huge portion of medium-level binary tree questions.