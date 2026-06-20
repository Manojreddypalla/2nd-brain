# Adjacency List

### Definition

An Adjacency List stores the **neighbors of each vertex**.

Instead of asking:

```text
Can i reach every node?
```

it asks:

```text
Who are my direct neighbors?
```

---

## Example Graph

```text
    0
   / \
  1---2
```

Adjacency List:

```text
0 -> 1, 2
1 -> 0, 2
2 -> 0, 1
```

---

## C++ Representation

```cpp
vector<vector<int>> graph(n);
```

Add edges:

```cpp
graph[0].push_back(1);
graph[1].push_back(0);

graph[0].push_back(2);
graph[2].push_back(0);

graph[1].push_back(2);
graph[2].push_back(1);
```

---

## Complete Structure

```cpp
graph[0] = {1,2};
graph[1] = {0,2};
graph[2] = {0,1};
```

Memory:

```text
0 -> [1,2]
1 -> [0,2]
2 -> [0,1]
```

---

## Complexities

|Operation|Complexity|
|---|---|
|Add Edge|O(1)|
|Traverse Neighbors|O(degree)|
|Edge Lookup|O(degree)|
|Space|O(V + E)|

---

## Advantages

- Very memory efficient.
    
- Best for sparse graphs.
    
- Perfect for BFS.
    
- Perfect for DFS.
    
- Most commonly used graph representation.
    

---

## Disadvantages

- Checking if a specific edge exists is slower than a matrix.
    
- Need to search the neighbor list.
    

---

## Why BFS/DFS Love It

Suppose you're at node:

```text
0
```

Neighbors are immediately available:

```text
0 -> [1,2]
```

Traversal becomes:

```cpp
for(auto neighbor : graph[node])
{
    // visit neighbor
}
```

No need to scan the entire graph.

---

## Mental Model

Think of a social media friends list:

```text
Alice -> Bob, Charlie
Bob -> Alice, David
Charlie -> Alice
```

Each person stores only their friends.

That's exactly an adjacency list.

---

## Use Cases

- BFS
    
- DFS
    
- Topological Sort
    
- Dijkstra's Algorithm
    
- Most Competitive Programming Graph Problems
    

---

### One-Line Summary

> **Adjacency List stores neighbors of each vertex, uses O(V + E) memory, and is the most commonly used graph representation for traversal algorithms like BFS and DFS.**

---

Notice the progression:

```text
Edge List
↓
Store edges

Adjacency Matrix
↓
Store all possible relationships

Adjacency List
↓
Store only actual neighbors
```

The final representation you'll usually see is **Weighted Adjacency List**, which is just an adjacency list that also stores the cost/weight of each connection.