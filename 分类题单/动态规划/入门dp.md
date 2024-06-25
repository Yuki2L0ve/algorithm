## LC70. 爬楼梯
```C++
// 记忆化搜索
class Solution {
public:
    int climbStairs(int n) {
        int f[n + 1];
        memset(f, -1, sizeof f);

        auto dfs = [&](auto& dfs, int i) -> int {
            if (i <= 1) return 1;
            int &ans = f[i];
            if (~ans)   return ans;
            return ans = dfs(dfs, i - 1) + dfs(dfs, i - 2);
        };

        return dfs(dfs, n);
    }
};
```
```C++
// 动态规划
class Solution {
public:
    int climbStairs(int n) {
        vector<int> f(n + 1);
        f[0] = f[1] = 1;
        for (int i = 2; i <= n; ++ i) {
            f[i] = f[i - 1] + f[i - 2];
        }
        return f[n];
    }
};
```


```C++
// 矩阵快速幂
class Solution {
public:
    using LL = long long ;
    using VVL = vector<vector<LL>>;
    VVL mul(const VVL& a, const VVL& b) {
        int n = a.size(), m = b[0].size(), c = a[0].size();
        VVL ans(n, vector<LL>(m, 0));
        for (int i = 0; i < n; ++ i)
            for (int j = 0; j < m; ++ j)
                for (int k = 0; k < c; ++ k)
                    ans[i][j] += a[i][k] * b[k][j];
        return ans;
    }

    VVL matrixPow(VVL& a, int b) {
        int n = a.size();
        VVL ans(n, vector<LL>(n, 0));
        for (int i = 0; i < n; ++ i)    ans[i][i] = 1;
        while (b) {
            if (b & 1)  ans = mul(ans, a);
            a = mul(a, a);
            b >>= 1;
        }
        return ans;
    }

    int climbStairs(int n) {
        if (n == 0) return 1;
        if (n == 1) return 1;
        VVL start = {{1, 1}};
        VVL base = {{1, 1}, {1, 0}};
        VVL ans = mul(start, matrixPow(base, n - 1));
        return ans[0][0];
    }
};
```

## ABC129_C
[传送门](https://atcoder.jp/contests/abc129/tasks/abc129_c)
```C++
#include <bits/stdc++.h>
using namespace std;
const int N = 1e5 + 10, MOD = 1e9 + 7;
int f[N], n, m, x;
bool st[N];

int main() {
    scanf("%d%d", &n, &m);
    while (m -- ) {
        scanf("%d", &x);
        st[x] = true;
    }
    
    st[0] = true;   f[0] = 1;
    if (!st[1]) f[1] = 1;
    for (int i = 2; i <= n; ++ i) 
        if (!st[i])
            f[i] = (f[i - 1] + f[i - 2]) % MOD;
    printf("%d\n", f[n]);
}
```

## LC746. 使用最小花费爬楼梯
[传送门](https://leetcode.cn/problems/min-cost-climbing-stairs/description/)
```C++
// 记忆化搜索
class Solution {
public:
    int minCostClimbingStairs(vector<int>& cost) {
        int n = cost.size(), f[n + 1];
        memset(f, -1, sizeof f);

        auto dfs = [&](auto& dfs, int i) -> int {
            if (i <= 1) return 0;
            int &ans = f[i];
            if (~ans)   return ans;
            return ans = min(dfs(dfs, i - 1) + cost[i - 1], dfs(dfs, i - 2) + cost[i - 2]);
        };

        return dfs(dfs, n);
    }
};
```
```C++
// 动态规划
class Solution {
public:
    int minCostClimbingStairs(vector<int>& cost) {
        int n = cost.size(), f[n + 1];
        f[0] = f[1] = 0;
        for (int i = 2; i <= n; ++ i) {
            f[i] = min(f[i - 1] + cost[i - 1], f[i - 2] + cost[i - 2]);
        }
        return f[n];
    }
};
```

## LC377. 组合总和 Ⅳ
[传送门](https://leetcode.cn/problems/combination-sum-iv/description/)
```C++
// 记忆化搜索
class Solution {
public:
    int combinationSum4(vector<int>& nums, int target) {
        int n = nums.size(), f[target + 1];
        memset(f, -1, sizeof f);

        auto dfs = [&](auto& dfs, int i) -> int {
            if (i == 0) return 1;
            if (~f[i])  return f[i];

            int ans = 0;
            for (auto& x : nums)
                if (x <= i)
                    ans += dfs(dfs, i - x);
            
            return f[i] = ans;
        };

        return dfs(dfs, target);
    }
};
```
```C++
// 动态规划
class Solution {
public:
    int combinationSum4(vector<int>& nums, int target) {
        vector<unsigned> f(target + 1);
        f[0] = 1;
        for (int i = 1; i <= target; ++ i) {
            for (auto& x : nums)
                if (x <= i)
                    f[i] += f[i - x];
        }
        return f[target];
    }
};
```
