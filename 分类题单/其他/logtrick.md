## LC898. 子数组按位或操作
[传送门](https://leetcode.cn/problems/bitwise-ors-of-subarrays/description/)
```C++
/**
如何理解ors的有序性？
这里的“有序性”并不是指 ors 在整个运算过程中按某种排序标准保持排序，而是指在去重过程中，
我们利用了特定的处理顺序和条件判断来保证 ors 中的元素是按照一定的逻辑有序地更新和维护的。
在每次循环中，我们首先向 ors 中添加一个新的元素，这个新元素表示当前正在处理的数组元素作为一个新子数组的起始。
接下来，对 ors 中每个元素进行按位或操作更新，这些操作不会改变 ors 中元素的原始顺序，但会更新它们的值。
接下来是关键的去重部分：
+ 去重逻辑：去重是通过对 ors 进行线性扫描完成的，检查每个元素的按位或结果是否与前一个元素的按位或结果相同。如果相同，我们更新子数组的起始位置（取最早的位置）。如果不同，则将其视为新的唯一结果，保留在 ors 中。
+ 维持顺序：由于是从数组的末尾开始处理，并且每次都是将新元素加入到 ors 的末尾，然后检查是否与前面的元素有重复的按位或结果，这种处理方式意味着我们始终在处理从当前位置到数组末尾的所有可能的子数组。去重操作确保了每个唯一的按位或结果只保留最靠前的起始位置。

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
