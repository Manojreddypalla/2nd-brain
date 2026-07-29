note: we check n-1 and n-2  then store the answers to remove repetition 



```cpp
class Solution {
public:

    int fib(int n, vector<int>& dp)
    {
        // base case
        if(n <= 1)
            return n;

        // already computed
        if(dp[n] != -1)
            return dp[n];

        // store and return
        return dp[n] =
            fib(n-1, dp) + fib(n-2, dp);
    }

    int fibonacci(int n)
    {
        vector<int> dp(n+1, -1);

        return fib(n, dp);
    }
};
```

# Flow

Suppose:

```
fib(5)
```

Normal recursion repeats:

```
fib(3)
fib(2)
```

many times.

---

# Memoization Fix

This line:

```
if(dp[n]!=-1)
```

means:

```
already calculated before
```

So recursion stops repeating work.

---

# Main DP Idea

fib(n)=fib(n−1)+fib(n−2)

[[8.Fibonacci Series using Recursion - Memoization|reference]]
[[8.1 fib|diagram]]
