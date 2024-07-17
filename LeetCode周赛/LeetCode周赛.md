记录每周的LeetCode周赛题单

# 第401场周赛
[LC3178. 找出 K 秒后拿着球的孩子](https://leetcode.cn/problems/find-the-child-who-has-the-ball-after-k-seconds/description/)
```C++
class Solution {
public:
    int numberOfChild(int n, int k) {
        int d = k / (n - 1), r = k % (n - 1);
        return d & 1 ? n - r - 1 : r;
    }
};
```

[3179. K 秒后第 N 个元素的值](https://leetcode.cn/problems/find-the-n-th-value-after-k-seconds/description/)
```C++
// 暴力
class Solution {
public:
    const int MOD = 1e9 + 7;
    int valueAfterKSeconds(int n, int k) {
        vector<int> a(n + 1, 1), s(n + 2);
        for (int T = 1; T <= k; ++ T) {
            for (int i = 1; i <= n; ++ i) {
                s[i] = (s[i - 1] + a[i]) % MOD;
            }
            a = s;
        }
        return a[n] % MOD;
    }
};
```
```C++
// 组合数学
const int MOD = 1e9 + 7, N = 1e5 + 10;
using LL = long long ;
LL fac[N + 1], inv[N + 1];

LL qmi(LL a, LL b) {
    LL ans = 1;
    while (b) {
        if (b &  1) ans = ans * a % MOD;
        a = a * a % MOD;
        b >>= 1;
    }
    return ans;
}

auto init = []{
    fac[0] = 1;
    for (int i = 1; i <= N; ++ i) {
        fac[i] = fac[i - 1] * i % MOD;
    }
    inv[N] = qmi(fac[N], MOD - 2);
    for (int i = N - 1; i >= 0; -- i) {
        inv[i] = inv[i + 1] * (i + 1) % MOD;
    }
    return 0;
}();

LL solve(int n, int k) {
    return fac[n] * inv[k] % MOD * inv[n - k] % MOD;
}

class Solution {
public:
    int valueAfterKSeconds(int n, int k) {
        return solve(n + k - 1, k);
    }
};
```


# 第132场双周赛
[LC3174. 清除数字](https://leetcode.cn/problems/clear-digits/description/)
```C++
class Solution {
public:
    string clearDigits(string s) {
        string ans;
        for (auto& c : s) {
            if (isdigit(c)) ans.pop_back();
            else    ans += c;
        }
        return ans;
    }
};
```
```java
class Solution {
    public String clearDigits(String s) {
        StringBuilder sb = new StringBuilder();
        for (char c : s.toCharArray()) {
            if (Character.isDigit(c)) {
                sb.deleteCharAt(sb.length() - 1);
            } else {
                sb.append(c);
            }
        }
        return sb.toString();
    }
}
```

[LC3175. 找到连续赢 K 场比赛的第一位玩家](https://leetcode.cn/problems/find-the-first-player-to-win-k-games-in-a-row/description/)
```C++
class Solution {
public:
    int findWinningPlayer(vector<int>& skills, int k) {
        int idx = 0, cnt = 0, n = skills.size();
        for (int i = 1; i < n && cnt < k; ++ i) {
            if (skills[i] > skills[idx]) {
                idx = i;
                cnt = 1;
            } else {
                ++ cnt;
            }
        }
        return idx;
    }
};
```
```java
class Solution {
    public int findWinningPlayer(int[] skills, int k) {
        int idx = 0, cnt = 0, n = skills.length;
        for (int i = 1; i < n && cnt < k; ++ i) {
            if (skills[i] > skills[idx]) {
                idx = i;
                cnt = 1;
            } else {
                ++ cnt;
            }
        }
        return idx;
    }
}
```

[LC3176. 求出最长好子序列 I](https://leetcode.cn/problems/find-the-maximum-length-of-a-good-subsequence-i/description/)

`f[x][j]`表示以数值x结尾、最多有j对相邻元素不同的最长子序列的长度
设`x = nums[i]`
1. 不选x：那么`f[x][j]`不变，即`f[x][j] = f[x][j]`
2. 选x：
   a. 把x加到以y结尾的子序列的末尾（且y=x），那么`f[x][j] = f[y][j] + 1`
   b. 把x加到以y结尾的子序列的末尾（且y!=x），那么`f[x][j] = f[y][j - 1] + 1`

所以：
`f[x][j] = max(f[x][j] + 1, max(f[y][j-1] for y in set) + 1)`
其实情况一是不需要考虑的，因为2(a)一定比1要好，因为`f[x][j] = f[y][j] + 1`的长度肯定是优于`f[x][j] = f[x][j]`的。

```C++
class Solution {
public:
    int maximumLength(vector<int>& nums, int k) {
        unordered_map<int, vector<int>> fs;
        vector<int> mx(k + 1);
        for (int x : nums) {
            if (!fs.count(x)) {
                fs[x] = vector<int>(k + 1);
            }
            auto& f = fs[x];
            for (int j = k; j >= 0; -- j) {
                f[j] = max(f[j], j > 0 ? mx[j - 1] : 0) + 1;
                mx[j] = max(mx[j], f[j]);
            }
        }
        return mx[k];
    }
};
```

[LC3177. 求出最长好子序列 II](https://leetcode.cn/problems/find-the-maximum-length-of-a-good-subsequence-ii/description/)
```C++
class Solution {
public:
    int maximumLength(vector<int>& nums, int k) {
        unordered_map<int, vector<int>> fs;
        vector<int> mx(k + 1);
        for (int x : nums) {
            if (!fs.count(x)) {
                fs[x] = vector<int>(k + 1);
            }
            auto& f = fs[x];
            for (int j = k; j >= 0; -- j) {
                f[j] = max(f[j], j > 0 ? mx[j - 1] : 0) + 1;
                mx[j] = max(mx[j], f[j]);
            }
        }
        return mx[k];
    }
};
```
```java
class Solution {
    public int maximumLength(int[] nums, int k) {
        Map<Integer, int[]> fs = new HashMap<>();
        int[] mx = new int[k + 1];
        Arrays.fill(mx, 0);

        for (int x : nums) {
            if (!fs.containsKey(x)) {
                fs.put(x, new int[k + 1]);
            }
            int[] f = fs.get(x);
            for (int j = k; j >= 0; -- j) {
                f[j] = Math.max(f[j], j > 0 ? mx[j - 1] : 0) + 1;
                mx[j] = Math.max(mx[j], f[j]);
            }
        }

        return mx[k];
    }
}
```


# 第400场周赛
[LC3168. 候诊室中的最少椅子数](https://leetcode.cn/problems/minimum-number-of-chairs-in-a-waiting-room/description/)
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

[LC3169. 无需开会的工作日](https://leetcode.cn/problems/count-days-without-meetings/description/)
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

[LC3170. 删除星号以后字典序最小的字符串](https://leetcode.cn/problems/lexicographically-minimum-string-after-removing-stars/description/)
```C++
class Solution {
public:
    string clearStars(string s) {
        vector<int> v[26];  // 表示从a - z的字母栈，栈中存放的是下标
        int n = s.size();
        vector<bool> st(n); // 记录s中下标为i的字符是否是要被删除的
        for (int i = 0; i < n; ++ i) {
            if (s[i] != '*') {  // 不是*则直接把下标放到栈中
                v[s[i] - 'a'].push_back(i);
            } else {
                for (int c = 0; c < 26; ++ c) {
                    if (!v[c].empty()) {
                        st[v[c].back()] = true; // 标记为true，该位置为*，需要被删除
                        v[c].pop_back();
                        break;
                    }
                }
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


[LC3171. 找到按位与最接近 K 的子数组](https://leetcode.cn/problems/find-subarray-with-bitwise-and-closest-to-k/description/)
```C++
class Solution {
public:
    int minimumDifference(vector<int>& nums, int k) {
        int ans = INT_MAX;
        for (int i = 0; i < nums.size(); ++ i) {
            int x = nums[i];
            ans = min(ans, abs(x - k));
            for (int j = i - 1; j >= 0 && (nums[j] & x) != nums[j]; -- j) {
                nums[j] |= x;
                ans = min(ans, abs(nums[j] - k));
            }
        }
        return ans;
    }
};
```
