```cpp
#include <iostream>
#include <vector>
#include <queue>
using namespace std;

struct Node {
    int r, c, dist;
};

int main() {

    vector<string> grid = {
        "S..#",
        ".#..",
        ".#.E"
    };

    int R = grid.size();
    int C = grid[0].size();

    queue<Node> q;
    vector<vector<bool>> vis(R, vector<bool>(C, false));

    int sr, sc;

    for (int i = 0; i < R; i++)
        for (int j = 0; j < C; j++)
            if (grid[i][j] == 'S') {
                sr = i;
                sc = j;
            }

    q.push({sr, sc, 0});
    vis[sr][sc] = true;

    int dr[] = {-1, 1, 0, 0};
    int dc[] = {0, 0, -1, 1};

    while (!q.empty()) {

        Node cur = q.front();
        q.pop();

        if (grid[cur.r][cur.c] == 'E') {
            cout << "Escape in " << cur.dist << " minutes\n";
            return 0;
        }

        for (int i = 0; i < 4; i++) {

            int nr = cur.r + dr[i];
            int nc = cur.c + dc[i];

            if (nr < 0 || nr >= R || nc < 0 || nc >= C)
                continue;

            if (grid[nr][nc] == '#')
                continue;

            if (vis[nr][nc])
                continue;

            vis[nr][nc] = true;
            q.push({nr, nc, cur.dist + 1});
        }
    }

    cout << "No escape possible";
}
```