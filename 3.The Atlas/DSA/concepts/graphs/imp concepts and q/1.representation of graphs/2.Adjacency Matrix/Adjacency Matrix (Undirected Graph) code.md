```cpp
#include <iostream>
#include <vector>

using namespace std;

int main()
{
    int n = 3;

    vector<vector<int>> graph(
        n,
        vector<int>(n, 0)
    );

    // Add edges
    graph[0][1] = 1;
    graph[1][0] = 1;

    graph[0][2] = 1;
    graph[2][0] = 1;

    graph[1][2] = 1;
    graph[2][1] = 1;

    // Print matrix
    for(int i = 0; i < n; i++)
    {
        for(int j = 0; j < n; j++)
        {
            cout << graph[i][j] << " ";
        }
        cout << endl;
    }

    return 0;
}
```

Output:

```
0 1 11 0 11 1 0
```