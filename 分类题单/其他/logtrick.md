## LC898. 子数组按位或操作
[传送门](https://leetcode.cn/problems/bitwise-ors-of-subarrays/description/)
```C++
/**
假设我们有一个数组 arr = [1, 2, 3]
 + 初始化：
开始时，我们定义一个 ors 向量来保存每个子数组的按位或结果和起始索引，同时定义一个集合 S 来存储所有不同的按位或结果。
+ 逆序遍历：
首先考虑元素 3。
将 3 作为一个新子数组的起始，ors 初始为：[(0, 2)]（按位或结果为 0，起始位置为 2）。
更新按位或结果，ors 变为：[(3, 2)]。
将结果 3 添加到集合 S。
+ 下一步，考虑元素 2：
将 2 加入 ors，ors 变为：[(3, 2), (0, 1)]。
更新按位或结果，对每个元素进行操作：
第一个元素 (3, 2) 更新为 (3 | 2, 2) = (3, 2)。
第二个元素 (0, 1) 更新为 (2, 1)。
去重并更新起始位置，ors 变为：[(3, 2), (2, 1)]。
将新结果 2 添加到集合 S。
+ 最后，考虑元素 1：
将 1 加入 ors，ors 变为：[(3, 2), (2, 1), (0, 0)]。
更新按位或结果：
(3, 2) 更新为 (3 | 1, 2) = (3, 2)。
(2, 1) 更新为 (2 | 1, 1) = (3, 1)。
(0, 0) 更新为 (1, 0)。
去重并更新起始位置，因为 3 已经存在，只更新起始位置，ors 更新为：[(3, 1), (1, 0)]。
将新结果 1 添加到集合 S。
+ 结果
最终集合 S = {1, 2, 3}，代表所有可能的不同按位或结果
**/
class Solution {
public:
    #define x first
    #define y second
    int subarrayBitwiseORs(vector<int>& nums) {
        // 存储每个子数组的按位或结果和子数组的起始位置。
        vector<pair<int, int>> ors; // (按位或结果, 子数组的起始位置)
        unordered_set<int> S;   // 存储所有不同的按位或结果

        // 逆序遍历,这样做是为了每次将当前元素加入到之前所有子数组的前面,并计算新的按位或结果
        for (int i = nums.size() - 1; i >= 0; -- i) {
            // 每当访问一个新的元素时,将其作为新的子数组的开始,并初始化其按位或结果为0
            ors.emplace_back(0, i);

            // 对于 ors 中的每个元素, 都将当前元素与之前的按位或结果进行或操作
            for (auto& o : ors) {
                o.x |= nums[i];
            }

            /**
            去重：通过检查当前的按位或结果与之前结果是否重复,来进行去重和合并操作。
            如果发现重复的按位或结果,更新其最新的起始位置,否则将其保留。
            这一步利用了 ors 数组的有序性,只保留最后一个出现的按位或结果及其最早的起始位置。
            **/
            int k = 0;
            for (int j = 0; j < ors.size(); ++ j) {
                if (ors[k].x == ors[j].x) {
                    ors[k].y = ors[j].y;
                } else {
                    ors[++ k] = ors[j];
                }
            }

            // 由于可能存在多个相同的按位或结果,因此通过调整 ors 的大小来去掉重复项
            // 比如某一时刻ors就是{(3,1), (1,0), (1,0)}, k=1, 之所以要去重是因为k是直接在ors中操作的
            // 但是ors[++ k] = ors[j];这行代码会使得与ors中本来的有的重复了
            ors.resize(k + 1);

            // 每次循环的末尾，将 ors 中所有的按位或结果添加到集合 S 中，集合的特性自动去重。
            for (auto& o : ors) {
                S.insert(o.x);
            }
        }

        // 函数最后返回集合 S 的大小，即不同按位或结果的数量。
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

## LC3097. 或值至少为 K 的最短子数组 II
[传送门](https://leetcode.cn/problems/shortest-subarray-with-or-at-least-k-ii/description/)
```C++
class Solution {
public:
    #define x first
    #define y second
    int minimumSubarrayLength(vector<int>& nums, int target) {
        int n = nums.size(), ans = INT_MAX;
        vector<pair<int, int>> ors;
        unordered_set<int> S;
        
        for (int i = n - 1; i >= 0; -- i) {
            ors.emplace_back(0, i);

            for (auto& o : ors) o.x |= nums[i];

            int k = 0;
            for (int j = 0; j < ors.size(); ++ j) {
                if (ors[k].x == ors[j].x)   ors[k].y = ors[j].y;
                else    ors[++ k] = ors[j];
            }

            ors.resize(k + 1);

            for (auto& o : ors)
                if (o.x >= target)
                    ans = min(ans, o.y - i + 1);
        }

        return ans == INT_MAX ? -1 : ans;
    }
};
```
