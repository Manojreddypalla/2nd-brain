# Graph Representation in C++ (STL)

#DSA #Graphs #CPP #STL

---

# What is a Graph?

A **Graph** is a collection of:

- **Vertices (Nodes)** → Objects
- **Edges** → Connections between objects

Example:

```
0 ----- 1
|       |
|       |
2 ----- 3
```

Vertices = {0,1,2,3}

Edges = {(0,1), (0,2), (1,3), (2,3)}

---

# Why Different Representations?

A graph can be stored in multiple ways.

Common methods:

1. Adjacency Matrix
2. Adjacency List ⭐ (Most Used)

---

# 1. Adjacency Matrix

Store graph as a 2D matrix.

If there is an edge:

```
matrix[u][v] = 1
```

Otherwise:

```
matrix[u][v] = 0
```

Example Graph

```
0 --- 1
|
|
2
```

Matrix

```
    0 1 2

0 [ 0 1 1 ]
1 [ 1 0 0 ]
2 [ 1 0 0 ]
```

### C++ Code

```cpp
int V = 3;

vector<vector<int>> adj(V, vector<int>(V, 0));

adj[0][1] = 1;
adj[1][0] = 1;

adj[0][2] = 1;
adj[2][0] = 1;
```

---

## Time Complexity

Access Edge

```
O(1)
```

Space

```
O(V²)
```

Best for

- Dense graphs
- Floyd Warshall
- Small graphs

---

# 2. Adjacency List ⭐

Instead of storing every possible edge,

Store **only existing neighbors**.

Think of every vertex having a list of friends.

Example

```
0 ---- 1
|
|
2
```

Store

```
0 -> 1,2

1 -> 0

2 -> 0
```

---

# STL Representation

```cpp
vector<vector<int>> adj(V);
```

Meaning

```
vector
|
+-- vector<int>
        |
        +-- neighbours of node
```

Visual

```
adj

0 --> [1,2]

1 --> [0]

2 --> [0]
```

---

# Creating an Undirected Graph

## Graph

```
0 ---- 1
|
|
2
```

Code

```cpp
int V = 3;

vector<vector<int>> adj(V);

adj[0].push_back(1);
adj[1].push_back(0);

adj[0].push_back(2);
adj[2].push_back(0);
```

Output

```
0 : 1 2

1 : 0

2 : 0
```

---

# Printing Adjacency List

```cpp
for(int i = 0; i < V; i++)
{
    cout << i << " -> ";

    for(int neighbour : adj[i])
    {
        cout << neighbour << " ";
    }

    cout << endl;
}
```

Output

```
0 -> 1 2

1 -> 0

2 -> 0
```

---

# Directed Graph

Edges have direction.

Example

```
0 ----> 1

|
|
v

2
```

Store

```
0 -> 1,2

1 ->

2 ->
```

Code

```cpp
adj[0].push_back(1);

adj[0].push_back(2);
```

Notice

We **DO NOT** add reverse edges.

---

# Weighted Graph

Edges have weights.

Example

```
0 --5--> 1

0 --2--> 2
```

Store pair

```
(destination, weight)
```

STL

```cpp
vector<vector<pair<int,int>>> adj(V);
```

Meaning

```
adj[node]

↓

(destination, weight)
```

---

## Example

```cpp
int V = 3;

vector<vector<pair<int,int>>> adj(V);

adj[0].push_back({1,5});

adj[0].push_back({2,2});

adj[1].push_back({2,7});
```

Memory

```
0

↓

(1,5)

↓

(2,2)

1

↓

(2,7)
```

---

# Traversing Weighted Graph

```cpp
for(auto edge : adj[0])
{
    cout << "Node = " << edge.first
         << " Weight = " << edge.second
         << endl;
}
```

Output

```
Node = 1 Weight = 5

Node = 2 Weight = 2
```

---

# Generic Input (Undirected)

```cpp
int V, E;

cin >> V >> E;

vector<vector<int>> adj(V);

for(int i = 0; i < E; i++)
{
    int u, v;

    cin >> u >> v;

    adj[u].push_back(v);

    adj[v].push_back(u);
}
```

Input

```
4 4

0 1

0 2

1 3

2 3
```

Graph

```
0

| \

1  2

 \/

 3
```

---

# Generic Input (Directed)

```cpp
int V, E;

cin >> V >> E;

vector<vector<int>> adj(V);

for(int i = 0; i < E; i++)
{
    int u, v;

    cin >> u >> v;

    adj[u].push_back(v);
}
```

---

# Generic Input (Weighted)

```cpp
int V, E;

cin >> V >> E;

vector<vector<pair<int,int>>> adj(V);

for(int i = 0; i < E; i++)
{
    int u, v, wt;

    cin >> u >> v >> wt;

    adj[u].push_back({v, wt});

    adj[v].push_back({u, wt});
}
```

---

# STL Functions Used

## vector

Dynamic array.

```cpp
vector<int> arr;
```

---

## push_back()

Adds an element to the end.

```cpp
arr.push_back(5);
```

Result

```
[5]
```

---

## pair

Stores two values.

```cpp
pair<int,int> p;

p.first

p.second
```

Example

```cpp
pair<int,int> edge = {3,10};
```

Means

```
Node = 3

Weight = 10
```

---

## Range-Based For Loop

Instead of

```cpp
for(int i = 0; i < adj[node].size(); i++)
```

Use

```cpp
for(int neighbour : adj[node])
```

Cleaner and preferred.

---

# Complexity Comparison

| Representation | Space | Check Edge | Traverse Neighbours |
|---------------|-------|------------|---------------------|
| Adjacency Matrix | O(V²) | O(1) | O(V) |
| Adjacency List | O(V + E) | O(Degree) | O(Degree) |

---

# Which Representation is Used in DSA?

✅ BFS

✅ DFS

✅ Dijkstra

✅ Topological Sort

✅ Prim's Algorithm

✅ Kruskal (with edge list)

✅ Bellman-Ford

Almost every graph interview problem uses an **Adjacency List** because it is memory-efficient and fast for traversing neighbors.

---

# Mental Model 🧠

Think of every node as having a contact list.

```
Person A

↓

Friends

B

C

D
```

Instead of remembering everyone in the world, A remembers **only its friends**.

Adjacency List works the same way.

```
0

↓

1

2

5
```

Node **0** knows only the nodes directly connected to it.

---

# Pattern to Remember

Unweighted Graph

```cpp
vector<vector<int>> adj(V);
```

Weighted Graph

```cpp
vector<vector<pair<int,int>>> adj(V);
```

Undirected Edge

```cpp
adj[u].push_back(v);

adj[v].push_back(u);
```

Directed Edge

```cpp
adj[u].push_back(v);
```

Weighted Edge

```cpp
adj[u].push_back({v, wt});
```

---

# Interview Cheat Sheet

```cpp
// Unweighted
vector<vector<int>> adj(V);

// Weighted
vector<vector<pair<int,int>>> adj(V);

// Undirected
adj[u].push_back(v);
adj[v].push_back(u);

// Directed
adj[u].push_back(v);

// Weighted
adj[u].push_back({v, wt});

// Traverse
for(int neighbour : adj[node])

// Weighted Traverse
for(auto edge : adj[node])
{
    int neighbour = edge.first;
    int weight = edge.second;
}
```