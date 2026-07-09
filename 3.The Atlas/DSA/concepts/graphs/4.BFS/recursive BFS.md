```cpp
#include <iostream>
#include <unordered_map>
#include <vector>
#include <queue>

using namespace std;

void bfs(unordered_map<char, vector<char>>& graph, queue<char>& q)
{
    if (q.empty())
        return;

    char current = q.front();
    q.pop();

    cout << current << " ";

    for (char neighbor : graph[current])
    {
        q.push(neighbor);
    }

    bfs(graph, q);
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

    queue<char> q;
    q.push('a');

    bfs(graph, q);

    return 0;
}
```

Output:

```
a c b e d f
```
