```cpp
#include <iostream>
#include <unordered_map>
#include <vector>
#include <queue>

using namespace std;

void bfs(unordered_map<char, vector<char>>& graph, char source)
{
    queue<char> q;

    q.push(source);

    while (!q.empty())
    {
        char current = q.front();
        q.pop();

        cout << current << " ";

        for (char neighbor : graph[current])
        {
            q.push(neighbor);
        }
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

    bfs(graph, 'a');

    return 0;
}
```

Output:

```
a c b e d f
```