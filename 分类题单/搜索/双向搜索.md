## P4799 世界冰球锦标赛
[传送门](https://www.luogu.com.cn/problem/P4799)
```C++
#include <bits/stdc++.h>
using namespace std;
using LL = long long ;
const int N = 55;
LL n, m, ans, w[N], lsum[1 << 21], rsum[1 << 21];

void dfs(int l, int r, LL sum, LL a[], int &cnt) {
    if (sum > m)    return ;
    if (l > r) {
        a[ ++ cnt] = sum;
        return ;
    }
    dfs(l + 1, r, sum + w[l], a, cnt);
    dfs(l + 1, r, sum, a, cnt);
}

int main() {
    scanf("%lld%lld", &n, &m);
    for (int i = 1; i <= n; ++ i)   scanf("%lld", &w[i]);
    int ls = 0, rs = 0, mid = n >> 1;
    dfs(1, mid, 0, lsum, ls);
    dfs(mid + 1, n, 0, rsum, rs);
    sort(lsum + 1, lsum + 1 + ls);
    for (int i =1 ; i <= rs; ++ i) {
        ans += upper_bound(lsum + 1, lsum + 1 + ls, m - rsum[i]) - (lsum + 1);
    }
    printf("%lld\n", ans);
}
```

## LC1755. 最接近目标值的子序列和
[传送门](https://leetcode.cn/problems/closest-subsequence-sum/description/)
```C++
class Solution {
public:
    static const int N = 1 << 20;  
    vector<int> w = vector<int>(N), lsum = vector<int>(N), rsum = vector<int>(N);

    void dfs(int l, int r, int sum, vector<int>& a, int &cnt) {
        if (cnt >= N) return;  // Ensure we do not go out of bounds
        if (l > r) {
            a[cnt ++ ] = sum;
            return;
        }
        dfs(l + 1, r, sum + w[l], a, cnt);
        dfs(l + 1, r, sum, a, cnt);
    }

    int minAbsDifference(vector<int>& nums, int m) {
        int i = 0;
        for (auto& x : nums)    w[i ++ ] = x;

        int ls = 0, rs = 0, n = nums.size(), mid = n / 2;
        dfs(0, mid, 0, lsum, ls);
        dfs(mid + 1, n - 1, 0, rsum, rs);

        sort(lsum.begin(), lsum.begin() + ls);
        sort(rsum.begin(), rsum.begin() + rs);

        int ans = INT_MAX;  
        for (int i = 0, j = rs - 1; i < ls; ++ i) { // 双指针
            while (j > 0 && abs(m - lsum[i] - rsum[j - 1]) <= abs(m - lsum[i] - rsum[j])) {
                -- j;
            }
            ans = min(ans, abs(m - lsum[i] - rsum[j]));
        }

        return ans;
    }
};
```
