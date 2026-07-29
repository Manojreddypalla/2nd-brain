# 🌲 DFS (Depth First Search) — Short Notes

---

## 1. What is DFS?

- **DFS (Depth First Search)** explores one branch completely before moving to another.
    
- It follows the **Root → Child → Grandchild** path until no further node exists, then **backtracks**.
    

**Example**

```text
        A
       / \
      B   C
     / \
    D   E
```

DFS (Preorder)

```text
A → B → D → E → C
```

**Key Idea:** Go **Deep First**, then Backtrack.

---

## 2. DFS Intuition

Think of exploring a maze.

```text
Start
 ↓
Go as deep as possible
 ↓
Dead End?
 ↓
Backtrack
 ↓
Explore another path
```

**Tree Example**

```text
A
↓
B
↓
D
↑
B
↓
E
↑
A
↓
C
```

---

## 3. Why Recursion?

Recursion naturally follows the tree structure.

Each recursive call processes one node and then processes its children.

```cpp
dfs(node)
{
    dfs(node->left);
    dfs(node->right);
}
```

**Advantages**

- Simple
    
- Readable
    
- Mirrors tree structure
    

---

## 4. Why Stack?

Recursion internally uses the **Call Stack**.

Iterative DFS uses an explicit **Stack**.

**Reason**

When visiting a child, we must remember the parent to return later.

LIFO (Last In, First Out) is perfect for this.

---

## 5. Call Stack Visualization

Tree

```text
    A
   /
  B
 /
D
```

Recursive Calls

```text
dfs(A)

↓

dfs(B)

↓

dfs(D)

↓

Return

↑

Return

↑

Return
```

Call Stack

```text
Top
-----
dfs(D)
dfs(B)
dfs(A)
-----
Bottom
```

---

## 6. DFS Template

### Recursive

```cpp
void dfs(TreeNode* root)
{
    if(root == NULL)
        return;

    // Process Node

    dfs(root->left);
    dfs(root->right);
}
```

### Iterative

```cpp
stack<TreeNode*> st;
st.push(root);

while(!st.empty())
{
    TreeNode* node = st.top();
    st.pop();

    // Process Node
}
```

---

## 7. Recursive vs Iterative

|Feature|Recursive|Iterative|
|---|---|---|
|Uses|Call Stack|Explicit Stack|
|Code|Short|Longer|
|Easy to Write|✅|❌|
|Stack Overflow Risk|Yes|No|
|Interview Preference|Usually|Sometimes|

---

## 8. Preorder vs Inorder vs Postorder

Tree

```text
        A
       / \
      B   C
     / \
    D   E
```

### Preorder (Root → Left → Right)

```text
A B D E C
```

Used For

- Copy Tree
    
- Serialize
    
- Tree Construction
    

---

### Inorder (Left → Root → Right)

```text
D B E A C
```

Used For

- BST → Sorted Order
    
- Expression Trees
    

---

### Postorder (Left → Right → Root)

```text
D E B C A
```

Used For

- Delete Tree
    
- Height
    
- Diameter
    
- Balanced Tree
    
- Dynamic Programming on Trees
    

---

## 9. When to Use Each Traversal?

|Traversal|Order|Common Uses|
|---|---|---|
|Preorder|Root → Left → Right|Copy Tree, Serialize, Build Tree|
|Inorder|Left → Root → Right|BST, Sorted Output|
|Postorder|Left → Right → Root|Height, Diameter, Delete Tree, DP|
|Level Order (BFS)|Level by Level|Views, Width, Level Problems|

---

# 🎯 Quick Revision

```text
DFS
----
Go Deep → Backtrack

Uses
----
Recursion / Stack

Templates
---------
Recursive
Iterative

Traversals
----------
Preorder  : Root Left Right
Inorder   : Left Root Right
Postorder : Left Right Root

Best Uses
---------
Preorder  → Copy / Serialize
Inorder   → BST
Postorder → Height / Diameter / DP

Complexity
----------
Time  : O(n)
Space : O(h)
```

These notes are designed as a **1–2 page cheat sheet** for quick revision before interviews, GATE, or coding practice.