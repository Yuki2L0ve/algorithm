## LC898. 子数组按位或操作
[传送门](https://leetcode.cn/problems/bitwise-ors-of-subarrays/description/)
```C++
class Solution {
public:
    #define x first
    #define y second
    int subarrayBitwiseORs(vector<int>& nums) {
        vector<pair<int, int>> ors;  // (或运算结果，得到该结果时的右端点)
        unordered_set<int> S;
        for (int i = nums.size() - 1; i >= 0; -- i) {
            ors.emplace_back(0, i);

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
        return S.size();
    }
};
```

## LC1521. 找到最接近目标值的函数值
[传送门](https://leetcode.cn/problems/find-a-value-of-a-mysterious-function-closest-to-target/description/)
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
                o.x &= nums[i];
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

## LC3171. 找到按位与最接近 K 的子数组
[传送门](https://leetcode.cn/problems/find-a-value-of-a-mysterious-function-closest-to-target/description/)
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
                o.x &= nums[i];
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
    #define x first
    #define y second
    vector<int> smallestSubarrays(vector<int>& nums) {
        int n = nums.size();
        vector<pair<int, int>> ors;
        vector<int> ans(n);
        for (int i = n - 1; i >= 0; -- i) {
            ors.emplace_back(0, i);

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

            ans[i] = ors[0].y - i + 1;
        }
        return ans;
    }
};
```
