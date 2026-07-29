Perfect. Let's do it the way you'd learn trees: start from the simplest representation and build intuition.

# Graph Representation #1: Edge List

Forget graphs for a second.

Imagine you're a road department.

Your job is not to store cities.

Your job is only to store roads.

So instead of:

```text
City A has roads to B and C
```

you simply write:

```text
A -- B
A -- C
B -- D
```

That's an Edge List.

---

## Example Graph

```text
    0
   / \
  1---2
```

Edges are:

```text
(0,1)
(0,2)
(1,2)
```

---

## C++ Representation

```cpp
vector<pair<int,int>> edges = {
    {0,1},
    {0,2},
    {1,2}
};
```

Think of it as:

```text
Road Register

Road 1 : 0 <-> 1
Road 2 : 0 <-> 2
Road 3 : 1 <-> 2
```

---

# What Is Stored In Memory?

```cpp
edges[0] = {0,1}
edges[1] = {0,2}
edges[2] = {1,2}
```

Memory looks roughly like:

```text
+-----+-----+
| 0   | 1   |
+-----+-----+

+-----+-----+
| 0   | 2   |
+-----+-----+

+-----+-----+
| 1   | 2   |
+-----+-----+
```

Only edges.

No neighbor lists.

No matrix.

Just connections.

---

# Why Is It Called Edge List?

Because we're literally making a list of edges.

```text
Edge
Edge
Edge
Edge
Edge
```

That's it.

---

# Advantages

### 1. Very Little Memory

Suppose:

```text
1000 vertices
5 edges
```

Store only:

```text
5 pairs
```

Memory:

```text
O(E)
```

where

```text
E = number of edges
```

---

### 2. Easy To Read Input

Most competitive programming problems give:

```text
4 5

0 1
0 2
1 2
1 3
2 3
```

This is already an edge list.

No conversion needed.

---

### 3. Great For Algorithms That Work On Edges

Example:

### Kruskal's MST

Kruskal thinks:

```text
Give me all roads.
Sort them.
Pick smallest.
```

It doesn't care about neighbors.

It cares about edges.

So edge list is perfect.

---

# Biggest Problem

Suppose I ask:

> Is there an edge between 0 and 2?

You must search:

```text
(0,1) ?
No

(0,2) ?
Yes
```

Worst case:

```text
O(E)
```

because you may scan every edge.

---

# Even Worse For BFS/DFS

Imagine you're standing at node:

```text
0
```

and want neighbors.

With an edge list you don't know.

You must scan everything:

```text
(0,1) -> neighbor found
(0,2) -> neighbor found
(1,2) -> ignore
```

Every time.

Very expensive.

---

# Mental Model

Think of an edge list as a notebook containing only relationships.

```text
Alice knows Bob
Alice knows Charlie
Bob knows David
```

If I ask:

> Who does Alice know?

You have to read the entire notebook.

That's exactly the weakness of edge lists.

---

# Complexity

|Operation|Cost|
|---|---|
|Store Graph|O(E)|
|Edge Lookup|O(E)|
|Find Neighbors|O(E)|
|Memory|O(E)|

---

# When To Use

Use Edge List when:

✅ Input naturally comes as edges

✅ Using Kruskal's Algorithm

✅ Need minimum memory

❌ Not ideal for BFS

❌ Not ideal for DFS

❌ Not ideal for frequent neighbor lookups

---

