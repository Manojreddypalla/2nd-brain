```cpp
#include <iostream>
#include <unordered_map>
#include <vector>

using namespace std;

void depthFirstPrint(unordered_map<char, vector<char>>& graph, char source)
{
    cout << source << " ";

    for (char neighbor : graph[source])
    {
        depthFirstPrint(graph, neighbor);
    }
}

int main()
{
    unordered_map<char, vector<char>> graph;

    graph['a'] = {'c', 'b'};
    graph['b'] = {'d'};
    graph['c'] = {'e'};
    graph['d'] = {'f'};
    graph['e'] = {};
    graph['f'] = {};

    depthFirstPrint(graph, 'a');

    return 0;
}
```

Output:

```
a c e b d f
```

Now VERY important understanding: