# Adjacency Matrix

### Definition

An Adjacency Matrix is a **2D array (V × V)** where:

```text
matrix[i][j] = 1
```

means an edge exists between vertex `i` and vertex `j`.

```text
matrix[i][j] = 0
```

means no edge exists.

---

## Example Graph

```text
    0
   / \
  1---2
```

Matrix:

```text
    0 1 2

0   0 1 1
1   1 0 1
2   1 1 0
```

---

## C++ Representation

```cpp
vector<vector<int>> graph(
    n,
    vector<int>(n, 0)
);
```

Add an edge:

```cpp
graph[0][1] = 1;
graph[1][0] = 1;   // undirected graph
```

---

## Weighted Adjacency Matrix

Instead of storing `0` or `1`, store the edge weight.

```text
0 --5-- 1
 \2
  \
   2
```

Matrix:

```text
    0 1 2

0   0 5 2
1   5 0 0
2   2 0 0
```

---

## Complexities

|Operation|Complexity|
|---|---|
|Add Edge|O(1)|
|Check Edge|O(1)|
|Space|O(V²)|

---

## Advantages

- Very fast edge lookup.
    
- Simple implementation.
    
- Good for dense graphs.
    
- Useful when frequent edge existence checks are needed.
    

---

## Disadvantages

- High memory usage.
    
- Wastes space for sparse graphs.
    
- Not ideal for BFS/DFS compared to adjacency lists.
    

---

## Mental Model

Think of it as a **lookup table**:

```text
"Can node i directly reach node j?"
```

Instead of searching through edges, simply check:

```cpp
graph[i][j]
```

and get the answer instantly.

---

## Use Cases

- Dense graphs
    
- Network connectivity tables
    
- Floyd-Warshall Algorithm
    
- Graphs requiring frequent edge existence checks
    

---

### One-Line Summary

> **Adjacency Matrix trades memory (O(V²)) for extremely fast edge lookup (O(1)).**