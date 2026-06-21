```cpp
#include <iostream>
#include <unordered_map>
#include <vector>
#include <stack>

using namespace std;

bool hasPath(unordered_map<char, vector<char>>& graph,
             char src,
             char dst)
{
    stack<char> st;

    st.push(src);

    while (!st.empty())
    {
        char current = st.top();
        st.pop();

        if (current == dst)
            return true;

        for (char neighbor : graph[current])
        {
            st.push(neighbor);
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