## LC898. 子数组按位或操作
[传送门](https://leetcode.cn/problems/bitwise-ors-of-subarrays/description/)
```C++
class Solution {
public:
    int subarrayBitwiseORs(vector<int>& arr) {
        unordered_set<int> S;
        for (int i = 0; i < arr.size(); ++ i) {
            S.insert(arr[i]);
            for (int j = i - 1; j >= 0 && (arr[j] | arr[i]) != arr[j]; -- j) {
                arr[j] |= arr[i];
                S.insert(arr[j]);
            }
        }
        return S.size();
    }
};
```

## LC1521. 找到最接近目标值的函数值
[传送门](https://leetcode.cn/problems/find-a-value-of-a-mysterious-function-closest-to-target/description/)
```C++
class Solution {
public:
    int closestToTarget(vector<int>& arr, int target) {
        int n = arr.size(), ans = INT_MAX;
        for (int i = 0; i < n; ++ i) {
            ans = min(ans, abs(arr[i] - target));
            for (int j = i - 1; j >= 0 && (arr[j] & arr[i]) != arr[j]; -- j) {
                arr[j] &= arr[i];
                ans = min(ans, abs(arr[j] - target));
            }
        }
        return ans;
    }
};
```

## LC3171. 找到按位与最接近 K 的子数组
[传送门](https://leetcode.cn/problems/find-subarray-with-bitwise-or-closest-to-k/)
```C++
class Solution {
public:
    #define x first
    #define y second
    int closestToTarget(vector<int>& nums, int k) {
        vector<pair<int, int>> ors;
        unordered_set<int> S;
        for (int i = nums.size() - 1; i >= 0; -- i) {
            ors.emplace_back(nums[i], i);

            for (auto& o : ors) {
                o.x |= nums[i];
            }

            int k = 0;
            for (int j = 0; j < ors.size(); ++ j) {
                if (ors[k].x == ors[j].x) {
                    ors[k].y = ors[j].y;
                } else {
                    ors[++ k] = ors[j];
                }
            }

            ors.resize(k + 1);

            for (auto& o : ors) {
                S.insert(o.x);
            }
        }

        int ans = INT_MAX;
        for (auto& x : S) {
            ans = min(ans, abs(x - k));
        }

        return ans;
    }
};
```


## LC2411. 按位或最大的最小子数组长度
[传送门](https://leetcode.cn/problems/smallest-subarrays-with-maximum-bitwise-or/description/)
```C++
class Solution {
public:
    vector<int> smallestSubarrays(vector<int>& nums) {
        int n = nums.size();
        vector<int> ans(n);
        for (int i = 0; i < n; ++ i) {
            ans[i] = 1; // 先单独处理i这个位置的元素
            for (int j = i - 1; j >= 0 && (nums[j] | nums[i]) != nums[j]; -- j) {
                nums[j] |= nums[i];
                ans[j] = i - j + 1;
            }
        }
        return ans;
    }
};
```

## LC3097. 或值至少为 K 的最短子数组 II
[传送门](https://leetcode.cn/problems/shortest-subarray-with-or-at-least-k-ii/description/)
```C++
class Solution {
public:
    int minimumSubarrayLength(vector<int>& nums, int k) {
        int n = nums.size(), ans = INT_MAX;
        for (int i = 0; i < n; ++ i) {
            // 先处理i这个单独元素
            if (nums[i] >= k)   return 1;
            // 这种方式有效避免了对那些 nums[i] 无法改变按位或结果的情况的无用计算。
            for (int j = i - 1; j >= 0 && (nums[j] | nums[i]) != nums[j]; -- j) {
                nums[j] |= nums[i]; // 更新 nums[j] 的值，使其成为从 j 到 i 所有元素的按位或结果
                if (nums[j] >= k) {
                    ans = min(ans, i - j + 1);
                }
            }
        }
        return ans == INT_MAX ? -1 : ans;
    }
};
```

## LC3209. 子数组按位与值为 K 的数目
[传送门](https://leetcode.cn/problems/number-of-subarrays-with-and-value-of-k/description/)
```C++
class Solution {
public:
    long long countSubarrays(vector<int>& nums, int k) {
        long long ans = 0;
        int l = 0, r = 0, n = nums.size();
        for (int i = 0; i < n; ++ i) {
            for (int j = i - 1; j >= 0 && (nums[j] & nums[i]) != nums[j]; -- j) {
                nums[j] &= nums[i];
            }
            while (l <= i && nums[l] < k)   ++ l;
            while (r <= i && nums[r] <= k)  ++ r;
            ans += r - l;
        }
        return ans;
    }
};
```
