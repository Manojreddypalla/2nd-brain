```CPP
#include <iostream>
#include <unordered_map>
#include <vector>
#include <queue>

using namespace std;

bool hasPath(unordered_map<char, vector<char>>& graph,
             char src,
             char dst)
{
    queue<char> q;

    q.push(src);

    while (!q.empty())
    {
        char current = q.front();
        q.pop();

        if (current == dst)
            return true;

        for (char neighbor : graph[current])
        {
            q.push(neighbor);
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