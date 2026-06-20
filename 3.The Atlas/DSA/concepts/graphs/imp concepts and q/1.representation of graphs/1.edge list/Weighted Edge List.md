# Weighted Edge List

Suppose:

```
0 --5-- 1 \2  \   2
```

Now each edge has a weight.

Store:

```
vector<vector<int>> edges;
```

or better:

```
struct Edge{    int u;    int v;    int weight;};
```

Example:

```cpp

#include <iostream>
#include <vector>

using namespace std;

struct Edge
{
    int u;
    int v;
    int weight;
};

int main()
{
    vector<Edge> edges;

    edges.push_back({0,1,5});
    edges.push_back({0,2,2});
    edges.push_back({1,2,1});

    for(auto edge : edges)
    {
        cout << edge.u << " "
             << edge.v << " "
             << edge.weight << endl;
    }

    return 0;
}

```

Output:

```
0 1 5
0 2 2
1 2 16+9++++++++++++++++++++999999999
```