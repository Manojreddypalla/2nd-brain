# Weighted Adjacency List

### Why Do We Need It?

Normal adjacency list stores:

```
0 -> 1, 21 -> 0, 22 -> 0, 1
```

It tells us:

```
Who is connected?
```

But not:

```
How much does it cost?How far is it?How much time does it take?
```

```cpp
#include <iostream>
#include <vector>

using namespace std;

int main()
{
    int V = 3;

    vector<vector<pair<int,int>>> graph(V);

    graph[0].push_back({1,5});
    graph[1].push_back({0,5});

    graph[0].push_back({2,2});
    graph[2].push_back({0,2});

    for(int i = 0; i < V; i++)
    {
        cout << i << " -> ";

        for(auto edge : graph[i])
        {
            cout << "("
                 << edge.first << ","
                 << edge.second
                 << ") ";
        }

        cout << endl;
    }

    return 0;
}
```

