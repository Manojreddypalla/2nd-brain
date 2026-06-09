# Core Idea

You are standing at:

```
top-left corner
```

and want to reach:

```
bottom-right corner
```

Allowed moves:

```
RIGHT
DOWN
```

```
Grid DP Pattern
```

then you can adapt to:

|Problem|Same Core Idea|
|---|---|
|Unique Paths|count paths|
|Min Path Sum|min cost|
|Obstacle Paths|blocked cells|
|Cherry Pickup|maximize score|
|Dungeon Game|survival transitions|

```cpp
class Solution {
public:

    long long gridTraveler(int m, int n,
                           map<pair<int,int>, long long>& memo)
    {
        // already computed
        if(memo.count({m,n}))
            return memo[{m,n}];

        // reached destination
        if(m == 1 && n == 1)
            return 1;

        // invalid grid
        if(m == 0 || n == 0)
            return 0;

        // store answer
        memo[{m,n}] =
            gridTraveler(m-1, n, memo) +
            gridTraveler(m, n-1, memo);

        return memo[{m,n}];
    }

    long long solve(int m, int n)
    {
        map<pair<int,int>, long long> memo;

        return gridTraveler(m, n, memo);
    }
};
```




```cpp
class Solution {
public:

    int solve(
        int m,
        int n,
        vector<vector<int>>& grid,
        map<pair<int,int>,int>& memo
    )
    {
        // invalid grid
        if(m == 0 || n == 0)
        {
            return 0;
        }

        // obstacle cell
        if(grid[m-1][n-1] == 1)
        {
            return 0;
        }

        // smallest valid grid
        if(m == 1 && n == 1)
        {
            return 1;
        }

        // already computed
        if(memo.count({m,n}))
        {
            return memo[{m,n}];
        }

        // top + left
        memo[{m,n}] =
            solve(m-1, n, grid, memo)
            +
            solve(m, n-1, grid, memo);

        return memo[{m,n}];
    }

    int uniquePathsWithObstacles(
        vector<vector<int>>& obstacleGrid
    )
    {
        int m = obstacleGrid.size();

        int n = obstacleGrid[0].size();

        map<pair<int,int>,int> memo;

        return solve(
            m,
            n,
            obstacleGrid,
            memo
        );
    }
};
```