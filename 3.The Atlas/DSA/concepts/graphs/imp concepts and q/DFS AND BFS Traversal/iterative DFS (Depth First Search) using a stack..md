```cpp
#include <iostream>
#include <unordered_map>
#include <vector>
#include <stack>

using namespace std;

void depthFirstPrint(unordered_map<char, vector<char>>& graph, char source)
{
    stack<char> st;

    st.push(source);

    while (!st.empty())
    {
        char current = st.top();
        st.pop();

        cout << current << " ";

        for (char neighbor : graph[current])
        {
            st.push(neighbor);
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

    depthFirstPrint(graph, 'a');

    return 0;
}
```
Output:

```
a b d f c e
```