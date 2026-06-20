## Weighted Adjacency Matrix

Instead of storing `1`, store weight.

```cpp
vector<vector<int>> graph(
    V,
    vector<int>(V, 0)
);

graph[0][1] = 5;
graph[1][0] = 5;

graph[0][2] = 2;
graph[2][0] = 2;
```

Result:

```
0 5 25 0 02 0 0
```