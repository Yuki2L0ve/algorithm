记录一些LeetCode周赛题单。

# 第400场周赛
## LC3168. 候诊室中的最少椅子数
[传送门](https://leetcode.cn/problems/minimum-number-of-chairs-in-a-waiting-room/description/)
```C++
class Solution {
public:
    int minimumChairs(string s) {
        int ans = 0, cnt = 0;
        for (auto& c : s) {
            if (c == 'E') {
                ans = max(ans, ++ cnt);
            } else {
                -- cnt;
            }
        }
        return ans;
    }
};
```

## LC3169. 无需开会的工作日
[传送门](https://leetcode.cn/problems/count-days-without-meetings/description/)
```C++
class Solution {
public:
    int countDays(int days, vector<vector<int>>& meetings) {
        sort(meetings.begin(), meetings.end());
        int n = meetings.size();
        int lastL = meetings[0][0], lastR = meetings[0][1];
        vector<pair<int, int>> v;
        for (int i = 1; i < n; ++ i) {
            if (meetings[i][0] > lastR) {
                v.push_back({lastL, lastR});
                lastL = meetings[i][0];
                lastR = meetings[i][1];
            } else {
                lastR = max(lastR, meetings[i][1]);
            }
        }
        v.push_back({lastL, lastR});

        for (auto it : v) 
            days -= it.second - it.first + 1;
        
        return days;
    }
};
```

## LC3170. 删除星号以后字典序最小的字符串
[传送门](https://leetcode.cn/problems/lexicographically-minimum-string-after-removing-stars/description/)
```C++
class Solution {
public:
    string clearStars(string s) {
        int n = s.size();
        vector<int> v[26];
        vector<bool> st(n);

        for (int i = 0; i < n; ++ i) {
            if (s[i] == '*') {
                for (int c = 0; c < 26; ++ c) {
                    if (!v[c].empty()) {
                        st[v[c].back()] = true;
                        v[c].pop_back();
                        break;
                    }
                }
            } else {
                v[s[i] - 'a'].push_back(i);
            }
        }

        string ans;
        for (int i = 0; i < n; ++ i)
            if (s[i] != '*' && !st[i])
                ans += s[i];
        
        return ans;
    }
};
```

## LC3171. 找到按位与最接近 K 的子数组
[传送门](https://leetcode.cn/problems/find-subarray-with-bitwise-and-closest-to-k/description/)
```C++
class Solution {
public:
    int minimumDifference(vector<int>& nums, int k) {
        int ans = INT_MAX;
        for (int i = 0; i < nums.size(); ++ i) {
            int x = nums[i];
            ans = min(ans, abs(x - k));
            for (int j = i - 1; j >= 0 && (nums[j] & x) != nums[j]; -- j) {
                nums[j] &= x;
                ans = min(ans, abs(nums[j] - k));
            }
        }
        return ans;
    }
};
```
