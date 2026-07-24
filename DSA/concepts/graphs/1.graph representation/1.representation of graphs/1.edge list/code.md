
```cpp
#include <iostream>
#include <vector>

using namespace std;

int main() {

    vector<pair<int,int>> edges;

    edges.push_back({0,1});
    edges.push_back({0,2});
    edges.push_back({1,2});

    for(auto edge : edges)
    {
        cout << edge.first << " -> "
             << edge.second << endl;
    }

    return 0;
}
```

Output:

```
0 -> 10 -> 21 -> 2
```

