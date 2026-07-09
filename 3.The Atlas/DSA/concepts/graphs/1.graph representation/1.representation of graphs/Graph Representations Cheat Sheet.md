

# 1. Edge List

## Idea

Store only the edges.

```text
0 --- 1
|   /
|  /
2
```

Stored as:

```text
(0,1)
(0,2)
(1,2)
```

---

## Declaration

```cpp
vector<pair<int,int>> edges;
```

---

## Adding Edges

```cpp
edges.push_back({0,1});
edges.push_back({0,2});
edges.push_back({1,2});
```

---

## Traversal

```cpp
for(auto edge : edges)
{
    cout << edge.first
         << " "
         << edge.second
         << endl;
}
```

---

## Complexity

|Operation|Complexity|
|---|---|
|Space|O(E)|
|Edge Lookup|O(E)|
|Find Neighbors|O(E)|

---

## Use Cases

- Kruskal's Algorithm
    
- Input Storage
    
- Sparse edge data
    

---

# 2. Adjacency Matrix

## Idea

Store every possible connection.

```text
    0 1 2

0   0 1 1
1   1 0 1
2   1 1 0
```

---

## Declaration

```cpp
vector<vector<int>> graph(
    V,
    vector<int>(V,0)
);
```

---

## Adding Edge

### Undirected

```cpp
graph[0][1] = 1;
graph[1][0] = 1;
```

### Directed

```cpp
graph[0][1] = 1;
```

---

## Check Edge

```cpp
if(graph[u][v])
{
    cout << "Edge Exists";
}
```

---

## Print Matrix

```cpp
for(int i=0;i<V;i++)
{
    for(int j=0;j<V;j++)
    {
        cout << graph[i][j] << " ";
    }
    cout << endl;
}
```

---

## Complexity

|Operation|Complexity|
|---|---|
|Add Edge|O(1)|
|Edge Lookup|O(1)|
|Space|O(V²)|

---

## Use Cases

- Dense Graphs
    
- Floyd Warshall
    
- Frequent Edge Checks
    

---

# 3. Adjacency List

## Idea

Store neighbors only.

```text
0 -> 1,2
1 -> 0,2
2 -> 0,1
```

---

## Declaration

```cpp
vector<vector<int>> graph(V);
```

---

## Adding Edge

### Undirected

```cpp
graph[0].push_back(1);
graph[1].push_back(0);
```

### Directed

```cpp
graph[0].push_back(1);
```

---

## Traversal

```cpp
for(int neighbor : graph[node])
{
    cout << neighbor << " ";
}
```

---

## Print Graph

```cpp
for(int i=0;i<V;i++)
{
    cout << i << " -> ";

    for(int neighbor : graph[i])
    {
        cout << neighbor << " ";
    }

    cout << endl;
}
```

---

## Complexity

|Operation|Complexity|
|---|---|
|Add Edge|O(1)|
|Traverse Neighbors|O(degree)|
|Space|O(V+E)|

---

## Use Cases

- BFS
    
- DFS
    
- Topological Sort
    
- Most Graph Problems
    

---

# 4. Weighted Adjacency List

## Idea

Store:

```cpp
(neighbor, weight)
```

instead of:

```cpp
neighbor
```

---

## Example

```text
0 --5--> 1
|
2
|
v
2
```

Stored as:

```text
0 -> (1,5) (2,2)
1 -> (0,5)
2 -> (0,2)
```

---

## Declaration

```cpp
vector<vector<pair<int,int>>> graph(V);
```

---

## Adding Edge

### Undirected

```cpp
graph[0].push_back({1,5});
graph[1].push_back({0,5});
```

### Directed

```cpp
graph[0].push_back({1,5});
```

---

## Traversal

### Traditional

```cpp
for(auto edge : graph[node])
{
    int neighbor = edge.first;
    int weight = edge.second;
}
```

---

### Structured Binding (Modern C++)

```cpp
for(auto [neighbor, weight] : graph[node])
{
}
```

---

## Print Graph

```cpp
for(int i=0;i<V;i++)
{
    cout << i << " -> ";

    for(auto [neighbor, weight] : graph[i])
    {
        cout << "("
             << neighbor
             << ","
             << weight
             << ") ";
    }

    cout << endl;
}
```

---

## Complexity

|Operation|Complexity|
|---|---|
|Add Edge|O(1)|
|Traverse Neighbors|O(degree)|
|Space|O(V+E)|

---

## Use Cases

- Dijkstra
    
- Prim's MST
    
- GPS Routing
    
- Network Routing
    

---

# Quick Comparison

|Representation|Space|Edge Lookup|BFS/DFS|
|---|---|---|---|
|Edge List|O(E)|O(E)|Poor|
|Adjacency Matrix|O(V²)|O(1)|Okay|
|Adjacency List|O(V+E)|O(degree)|Excellent|
|Weighted Adj List|O(V+E)|O(degree)|Excellent|

---

# One-Line Memory Trick

```text
Edge List
→ Store roads

Adjacency Matrix
→ Store every possible relationship

Adjacency List
→ Store neighbors

Weighted Adjacency List
→ Store neighbors + cost
```

# What To Use In Interviews?

```cpp
vector<vector<int>> graph(V);
```

for:

- BFS
    
- DFS
    
- Cycle Detection
    
- Topological Sort
    
- Connected Components
    

and

```cpp
vector<vector<pair<int,int>>> graph(V);
```

for:

- Dijkstra
    
- Prim
    
- Shortest Path Problems
    

These two representations cover about **95% of graph questions** you'll encounter in DSA.