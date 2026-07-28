## Top View of Binary Tree (Reference Notes)

### Idea

- View the tree from the **top**.
    
- Only the **first node** seen at each **Horizontal Distance (HD)** is included.
    

### Horizontal Distance (HD)

- Root → `HD = 0`
    
- Left Child → `HD - 1`
    
- Right Child → `HD + 1`
    

### Data Structures

```cpp
map<int, int> mpp;              // HD -> Node Value
queue<pair<Node*, int>> q;      // {Node, HD}
```

### Algorithm

1. If root is `NULL`, return empty answer.
    
2. Push `{root, 0}` into the queue.
    
3. Perform BFS.
    
4. For each node:
    
    - If its HD is not in the map, store it.
        
    - Push left child with `HD - 1`.
        
    - Push right child with `HD + 1`.
        
5. Traverse the map and store values in the answer.
    

### Complexity

- **Time:** `O(N log N)`
    
- **Space:** `O(N)`
    

---

## Code

```cpp
class Solution {
public:
    vector<int> topView(Node *root) {

        vector<int> ans;

        if (root == NULL)
            return ans;

        map<int, int> mpp;                  // HD -> Node value
        queue<pair<Node*, int>> q;          // {Node, HD}

        q.push({root, 0});

        while (!q.empty()) {

            auto it = q.front();
            q.pop();

            Node* node = it.first;
            int hd = it.second;

            // Store first node for this HD
            if (mpp.find(hd) == mpp.end())
                mpp[hd] = node->data;

            if (node->left)
                q.push({node->left, hd - 1});

            if (node->right)
                q.push({node->right, hd + 1});
        }

        for (auto it : mpp)
            ans.push_back(it.second);

        return ans;
    }
};
```

### Pattern to Remember

- **Top View** → First node at each HD
    
- **Traversal** → BFS (Queue)
    
- **Ordering** → `map` keeps HDs sorted (left → right)