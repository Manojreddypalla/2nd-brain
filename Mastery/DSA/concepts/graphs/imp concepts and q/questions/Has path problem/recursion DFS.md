```cpp
#include <iostream>
#include <unordered_map>
#include <vector>

using namespace std;

bool hasPath(unordered_map<char, vector<char>>& graph,
             char src,
             char dst)
{
    if (src == dst)
        return true;

    for (char neighbor : graph[src])
    {
        if (hasPath(graph, neighbor, dst) == true)
        {
            return true;
        }
    }

    return false;
}

int main()
{
    unordered_map<char, vector<char>> graph;

    graph['a'] = {'b', 'c'};
    graph['b'] = {'d'};
    graph['c'] = {};
    graph['d'] = {'f'};
    graph['f'] = {};

    cout << hasPath(graph, 'a', 'f');

    return 0;
}
```