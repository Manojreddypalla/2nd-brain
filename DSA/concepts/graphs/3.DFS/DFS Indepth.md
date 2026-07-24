![[Pasted image 20260709110318.png]]
# Connected Components using DFS

## Definition

A **Connected Component** is a group of vertices where every vertex is reachable from every other vertex.

If a graph has multiple disconnected groups, each group is called a connected component.

Example:

0 -- 1 -- 2

3 -- 4

5

Components:
{0,1,2}
{3,4}
{5}

Number of Components = 3

---

# Intuition

Think of a graph as several islands.

```
Island A        Island B       Island C

0-1-2           3-4            5
```

A DFS started from one island can never reach another island.

Therefore,

Every time we find an unvisited node,
we have discovered a **new island (component).**

---

# Main Idea

1. Create a visited array.
2. Iterate through every vertex.
3. If a node is unvisited:
   - Increase component count.
   - Run DFS from that node.
4. DFS marks the entire component as visited.

---

# Algorithm

```
visited = false

component = 0

for every vertex

    if vertex is not visited

        component++

        DFS(vertex)
```

DFS

```
mark current node visited

assign component id

for every neighbour

    if neighbour not visited

        DFS(neighbour)
```

---

# Why Outer Loop?

Suppose

```
0--1

3--4

6
```

DFS(0)

can only visit

```
0
1
```

It cannot jump to

```
3
4
6
```

Hence we must check every node.

```
for each vertex

    if unvisited

        DFS(vertex)
```

---

# Dry Run

Graph

```
0--1--2

3--4

5
```

Initially

visited

```
F F F F F F
```

Component = 0

### i = 0

```
component = 1

DFS(0)
```

Visits

```
0
↓
1
↓
2
```

visited

```
T T T F F F
```

---

### i = 1

Already visited

Skip

---

### i = 3

```
component = 2

DFS(3)
```

Visits

```
3
↓
4
```

visited

```
T T T T T F
```

---

### i = 5

```
component = 3

DFS(5)
```

visited

```
T T T T T T
```

Answer

```
Total Components = 3
```

---

# DFS Template (C++)

```cpp
void dfs(int node)
{
    visited[node] = true;

    for (int next : graph[node])
    {
        if (!visited[next])
            dfs(next);
    }
}
```

---

# Connected Components Template

```cpp
int components = 0;

for (int i = 0; i < n; i++)
{
    if (!visited[i])
    {
        components++;
        dfs(i);
    }
}
```

---

# Complexity

Time

```
O(V + E)
```

V = Vertices

E = Edges

Reason

- Every vertex visited once.
- Every edge explored once.

Space

```
O(V)
```

Used by

- visited array
- recursion stack (worst case)

---

# Mental Model

```
Scan graph

↓

Found an unvisited node

↓

New component starts

↓

DFS explores entire group

↓

Continue scanning
```

---

# Applications

- Number of Provinces
- Number of Islands
- Friend Circles
- Network Connectivity
- Social Networks
- Connected Computers
- Cluster Detection
- Image Segmentation
- Flood Fill
- Maze Exploration

---

# Common Interview Questions

### Q1. Why is the outer loop necessary?

Because DFS only visits nodes reachable from the starting node.

---

### Q2. Why use a visited array?

To avoid revisiting nodes and infinite recursion in cyclic graphs.

---

### Q3. Can BFS be used instead?

Yes.

Replace DFS with BFS.

Time Complexity remains

```
O(V + E)
```

---

### Q4. Can this work for directed graphs?

No.

For directed graphs we usually need

- Strongly Connected Components (Kosaraju)
- Tarjan Algorithm

---

# Pattern Recognition

Question says...

```
Groups

Clusters

Networks

Friend circles

Connected cities

Connected computers

Number of islands
```

Immediately think

```
Connected Components

↓

DFS / BFS
```

---

# Related Topics

- DFS
- BFS
- Union Find (DSU)
- Cycle Detection
- Topological Sort
- Strongly Connected Components
- Articulation Points
- Bridges

---

# Revision (30 Seconds)

✔ Iterate over every node.

✔ If node is unvisited,
start DFS.

✔ DFS marks the whole component.

✔ Every new DFS call = One Connected Component.

Time

```
O(V + E)
```

Space

```
O(V)
```