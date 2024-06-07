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

