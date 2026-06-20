```cpp
#include <bits/stdc++.h>
class Solution {
public:
int dp[1001][1001];

    int solve(int i,int j,string &a , string &b)
    {
        if(i==a.size()||j==b.size())
        {
            return 0;
        }
        if(dp[i][j]!=-1)
        {
            return dp[i][j];
        }

        if(a[i]==b[j])
        {
            return dp[i][j]=1+solve(i+1,j+1,a,b);
        }
        dp[i][j]=max(solve(i,j+1,a,b),solve(i+1,j,a,b));
    
    return dp[i][j];
    
    }

 
    int longestCommonSubsequence(string text1, string text2) {
       
        memset(dp,-1,sizeof(dp));
         return solve(0,0,text1,text2);
        
    }
};

```


```cpp
#include <bits/stdc++.h>
using namespace std;

class Solution {
public:

    int dp[1001][1001];

    int solve(int i, int j, string &a, string &b)
    {
        // base case
        if(i == a.size() || j == b.size())
        {
            return 0;
        }

        // already computed
        if(dp[i][j] != -1)
        {
            return dp[i][j];
        }

        // match
        if(a[i] == b[j])
        {
            return dp[i][j] =
                1 + solve(i + 1, j + 1, a, b);
        }

        // mismatch
        dp[i][j] = max(
            solve(i, j + 1, a, b),
            solve(i + 1, j, a, b)
        );

        return dp[i][j];
    }

    int longestCommonSubsequence(string text1, string text2)
    {
        memset(dp, -1, sizeof(dp));

        return solve(0, 0, text1, text2);
    }
};
```
