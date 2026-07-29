This is one of the **highest ROI topics** in Trees. Most interview tree problems are just variations of **7 DFS patterns**. If you master these patterns, you won't memorize 50 solutions—you'll recognize which pattern to apply.

---

# 🌲 DFS Patterns

## 1. Return Value Pattern

### Idea

Each recursive call **returns some information** to its parent.

Instead of printing or storing globally, every node computes a value and returns it upward.

```text
Child

↓

Compute

↓

Return

↓

Parent
```

### Generic Template

```cpp
Type dfs(TreeNode* root)
{
    if(root == NULL)
        return baseValue;

    Type left = dfs(root->left);
    Type right = dfs(root->right);

    // Compute answer

    return result;
}
```

### Used In

- Height
    
- Diameter
    
- Balanced Tree
    
- Maximum Path Sum
    
- Count Nodes
    
- Sum of Nodes
    

### Example (Height)

```cpp
int height(TreeNode* root)
{
    if(root == NULL)
        return 0;

    return 1 + max(height(root->left),
                   height(root->right));
}
```

### Recognition

If the problem asks:

> "Return the height/sum/count/value of each subtree"

➡️ Use **Return Value Pattern**.

---

# 2. Bottom-Up Pattern

### Idea

Children solve the problem first.

Parent uses children's answers.

```text
      Parent
      /    \
     /      \
 Left        Right

↑            ↑

Children finish first
```

### Flow

```text
Leaf

↓

Parent

↓

Grandparent

↓

Root
```

### Template

```cpp
dfs(node)
{
    left = dfs(left);

    right = dfs(right);

    return combine(left,right);
}
```

### Used In

- Height
    
- Diameter
    
- Balanced Tree
    
- Maximum Path Sum
    
- Largest BST
    
- Tree DP
    

### Recognition

If parent needs children's answers first,

➡️ Bottom-Up.

---

# 3. Top-Down Pattern

### Idea

Information flows **from parent to child**.

Instead of receiving answers,

children receive information.

```text
Root

↓

Parent Value

↓

Children
```

### Template

```cpp
dfs(node, currentAnswer)
{
    update(currentAnswer);

    dfs(left,newAnswer);

    dfs(right,newAnswer);
}
```

### Used In

- Path Sum
    
- Root-to-Leaf Paths
    
- Tree Paths
    
- Prefix Sum
    
- Tree Coloring
    

### Example

```cpp
dfs(node,sum)
{
    sum += node->val;

    dfs(left,sum);

    dfs(right,sum);
}
```

### Recognition

If every child needs information from parent,

➡️ Top-Down.

---

# Bottom-Up vs Top-Down

|Bottom-Up|Top-Down|
|---|---|
|Children → Parent|Parent → Children|
|Returns values|Passes values|
|Postorder|Preorder|
|Height|Path Sum|

---

# 4. Height Pattern

Probably the most common Tree pattern.

### Core Formula

```cpp
answer = f(left,right)
```

where

```cpp
left = dfs(left);

right = dfs(right);
```

### Generic Template

```cpp
dfs(node)
{
    if(node==NULL)
        return base;

    left = dfs(left);

    right = dfs(right);

    return combine(left,right);
}
```

### Problems

- Height
    
- Diameter
    
- Balanced Tree
    
- Maximum Path Sum
    
- Longest Univalue Path
    

### Recognition

If solution needs

```text
Left Answer

+

Right Answer
```

➡️ Height Pattern.

---

# 5. Tree DP Basics

Tree DP = Dynamic Programming on Trees.

Every subtree is treated as a subproblem.

### Idea

```text
Solve Left

↓

Solve Right

↓

Combine
```

Exactly like DP.

### Template

```cpp
dfs(node)
{
    left = dfs(left);

    right = dfs(right);

    return best(left,right);
}
```

### Used In

- Maximum Path Sum
    
- House Robber III
    
- Diameter
    
- Binary Tree Cameras
    
- Largest BST
    

### Recognition

If every subtree has an optimal answer,

➡️ Tree DP.

---

# 6. Construction Pattern

### Idea

Build a tree using recursive rules.

Usually split arrays.

```text
Root

↓

Left Subtree

↓

Right Subtree
```

### Generic Template

```cpp
build(...)
{
    root = ...

    root->left = build(...);

    root->right = build(...);

    return root;
}
```

### Used In

- Build Tree from Preorder + Inorder
    
- Build Tree from Inorder + Postorder
    
- BST from Sorted Array
    

### Recognition

Question says

> Construct  
> Build  
> Create

➡️ Construction Pattern.

---

# 7. Modification Pattern

### Idea

Instead of returning answers,

modify the existing tree.

```text
Visit

↓

Change

↓

Return
```

### Template

```cpp
dfs(node)
{
    modify(node);

    dfs(left);

    dfs(right);
}
```

### Used In

- Invert Tree
    
- Flatten Tree
    
- Children Sum Property
    
- Delete Leaves
    
- Trim BST
    

### Recognition

Question says

> Modify  
> Change  
> Update  
> Convert

➡️ Modification Pattern.

---

# Pattern Recognition Table

|Pattern|Information Flow|Returns?|Typical Traversal|Example Problems|
|---|---|---|---|---|
|Return Value|Child → Parent|✅ Yes|Postorder|Height, Count, Sum|
|Bottom-Up|Child → Parent|✅ Yes|Postorder|Diameter, Balanced, Max Path|
|Top-Down|Parent → Child|❌ No (passes values)|Preorder|Path Sum, Root-to-Leaf Paths|
|Height Pattern|Left + Right → Parent|✅ Yes|Postorder|Height, Diameter, Balanced|
|Tree DP|Subtree → Parent|✅ Yes|Postorder|House Robber III, Cameras|
|Construction|Build Child Trees|Returns Node|Preorder|Build Tree, BST Construction|
|Modification|Visit & Change|Usually No|Preorder/Postorder|Flatten, Invert, Children Sum|

---

# 🧠 How to Identify the Pattern

```text
Need a value from each subtree?
        │
        ▼
Return Value Pattern

Need left + right answers?
        │
        ▼
Height Pattern / Bottom-Up

Need to pass information downward?
        │
        ▼
Top-Down Pattern

Need optimal answer for every subtree?
        │
        ▼
Tree DP

Need to build a new tree?
        │
        ▼
Construction Pattern

Need to modify the existing tree?
        │
        ▼
Modification Pattern
```

---

# 🎯 Cheat Sheet

```text
Return Value
------------
Return something to parent.

Bottom-Up
----------
Children solve first.

Top-Down
---------
Parent passes information.

Height Pattern
--------------
left + right → answer

Tree DP
-------
Subtree = DP state.

Construction
------------
Create new tree recursively.

Modification
------------
Change existing tree.
```

---

# ⭐ The 90% Interview Rule

Most Binary Tree interview questions can be classified into one of these patterns:

|Problem|Pattern|
|---|---|
|Height|Return Value + Height|
|Diameter|Bottom-Up + Height|
|Balanced Tree|Bottom-Up + Height|
|Same Tree|Return Value|
|Symmetric Tree|Return Value|
|Path Sum|Top-Down|
|Root-to-Leaf Paths|Top-Down|
|Maximum Path Sum|Bottom-Up + Tree DP|
|Lowest Common Ancestor|Return Value|
|Build Tree|Construction|
|Flatten Tree|Modification|
|Invert Tree|Modification|
|Children Sum Property|Modification|
|Serialize/Deserialize|Construction + Traversal|

**If you can recognize these 7 patterns, you'll solve most Binary Tree problems by adapting a familiar template rather than starting from scratch every time.**