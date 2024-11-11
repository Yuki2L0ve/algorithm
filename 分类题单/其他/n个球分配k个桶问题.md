# LC473. 火柴拼正方形
[传送门](https://leetcode.cn/problems/matchsticks-to-square/description/)
```C++
class Solution {
public:
    vector<int> c;
    int n, k, target;

    bool dfs(vector<int>& cur, int start) {
        if (start >= n) return true;

        int zeros = 0;
        for (int i = 0; i < k; ++ i)    zeros += cur[i] == 0;
        if (zeros > n - start)  return false;

        for (int i = 0; i < k; ++ i) 
            if (cur[i] > target)
                return false;
        
        for (int i = 0; i < k; ++ i) {
            if (i && cur[i] == cur[i - 1])  continue;
            if (cur[i] + c[start] > target)  continue;
            cur[i] += c[start];
            if (dfs(cur, start + 1))    return true;
            cur[i] -= c[start];
        }

        return false;
    }

    bool makesquare(vector<int>& matchsticks) {
        c = matchsticks;
        int s = accumulate(c.begin(), c.end(), 0);
        n = c.size(), k = 4, target = s / k;
        if (s % k)  return false;

        sort(c.begin(), c.end(), greater<int>());

        vector<int> cur(k);
        return dfs(cur, 0);
    }
};
```

# LC698. 划分为k个相等的子集
[传送门](https://leetcode.cn/problems/partition-to-k-equal-sum-subsets/description/?envType=daily-question&envId=2024-08-25)
```C++
class Solution {
public:
    int n, k, target;
    vector<int> c;

    bool dfs(vector<int>& cur, int start) {
        if (start >= n)  return true;

        int zeros = 0;
        for (int i = 0; i < k; ++ i)    zeros += cur[i] == 0;
        if (zeros > n - start)  return false;

        for (int i = 0; i < k; ++ i) 
            if (cur[i] > target) 
                return false;
        
        for (int i = 0; i < k; ++ i) {
            if (i && cur[i] == cur[i - 1])  continue;
            if (cur[i] + c[start] > target)  continue;
            cur[i] += c[start];
            if (dfs(cur, start + 1))   return true;
            cur[i] -= c[start];
        }
        
        return false;
    }
    
    bool canPartitionKSubsets(vector<int>& nums, int k) {
        c = nums;
        int s = accumulate(c.begin(), c.end(), 0);
        this->k = k, n = c.size(), target = s / k;
        if (s % k)   return false;
        
        sort(c.begin(), c.end(), greater<int>());

        vector<int> cur(k);
        return dfs(cur, 0);
    }
};
```

# LC2305. 公平分发饼干
[传送门](https://leetcode.cn/problems/fair-distribution-of-cookies/description/)
```C++
class Solution {
public:
    vector<int> c;
    int target, n, k;

    void dfs(vector<int>& cur, int start) {
        if (start >= n) {
            int t = INT_MIN;
            for (int i = 0; i < k; ++ i) {
                t = max(t, cur[i]);
            }
            target = min(target, t);
            return ;
        }

        int zeros = 0;
        for (int i = 0; i < k; ++ i)    zeros += cur[i] == 0;
        if (zeros > n - start)  return ;

        for (int i = 0; i < k; ++ i)
            if (cur[i] > target)
                return ;
        
        for (int i = 0; i < k; ++ i) {
            if (i && cur[i] == cur[i - 1])   continue;
            if (cur[i] + c[start] > target)    continue;
            cur[i] += c[start];
            dfs(cur, start + 1);
            cur[i] -= c[start];
        }
    }

    int distributeCookies(vector<int>& cookies, int k) {
        this->c = cookies;
        this->k = k, this->n = c.size(), target = INT_MAX;

        sort(c.begin(), c.end(), greater<int>());

        vector<int> cur(k);
        dfs(cur, 0);

        return target;
    }
};
```
