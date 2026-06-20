```cpp
#include <iostream>
#include <vector>

using namespace std;

int main()
{
    int n = 3;

    vector<vector<int>> graph(n);

    graph[0].push_back(1);
    graph[1].push_back(0);

    graph[0].push_back(2);
    graph[2].push_back(0);

    graph[1].push_back(2);
    graph[2].push_back(1);

    for(int i = 0; i < n; i++)
    {
        cout << i << " -> ";

        for(int neighbor : graph[i])
        {
            cout << neighbor << " ";
        }

        cout << endl;
    }

    return 0;
}
```

### Output

```
0 -> 1 21 -> 0 22 -> 0 1
```