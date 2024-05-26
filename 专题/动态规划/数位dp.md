这个专题是关于**数位dp**



## P4999 烦人的数学作业

[传送门](https://www.luogu.com.cn/problem/P4999)

```C++
#include <bits/stdc++.h>
using namespace std;
using LL = long long;
const int N = 20, MOD = 1e9 + 7;
LL f[N][9 * 18 + 10], l, r;

LL solve(LL n) {
    string s = to_string(n);
    int m = s.size();
    memset(f, -1, sizeof f);

    function<LL(int, int, int)> dfs = [&](int i, int sum, bool isLimit) -> LL {
        if (i == m) return sum;
        if (!isLimit && ~f[i][sum]) return f[i][sum];

        LL up = isLimit ? s[i] - '0' : 9, ans = 0;
        for (int d = 0; d <= up; ++ d) {
            ans = (ans + dfs(i + 1, sum + d, isLimit && d == up)) % MOD;
        }

        if (!isLimit)   f[i][sum] = ans;

        return ans;
    };

    return dfs(0, 0, true);
}

int main() {
    int T;
    scanf("%d", &T);
    while (T -- ) {
        scanf("%lld%lld", &l, &r);
        LL ans = (solve(r) - solve(l - 1) + MOD) % MOD;
        printf("%lld\n", ans);
    }
}
```



## P2602 数字计数

[传送门](https://www.luogu.com.cn/problem/P2602)

```C++
#include <bits/stdc++.h>
using namespace std;
using LL = long long;
LL f[15][15], l, r;

LL solve(LL n, int x) {
    string s = to_string(n);
    int m = s.size();
    memset(f, -1, sizeof f);

    function<LL(int, int, bool, bool)> dfs = [&](int i, int cnt, bool isLimit, bool isNum) -> LL {
        if (i == m) return cnt;
        if (!isLimit && isNum && ~f[i][cnt])    return f[i][cnt];

        LL up = isLimit ? s[i] - '0' : 9, ans = 0;
        if (!isNum) ans = dfs(i + 1, cnt, false, false);

        for (int d = 1 - isNum; d <= up; ++ d) {
            if (d == x) ans += dfs(i + 1, cnt + 1, isLimit && d == up, true);
            else    ans += dfs(i + 1, cnt, isLimit && d == up, true);
        }

        if (!isLimit && isNum)  f[i][cnt] = ans;
        return ans;
    };

    return dfs(0, 0, true, false);
}

int main() {
    scanf("%lld%lld", &l, &r);
    if (l > r)  swap(l, r);
    for (int i = 0; i <= 9; ++ i) {
        LL ans = solve(r, i) - solve(l - 1, i);
        printf("%lld ", ans);
    }
}
```



## AcWing1081 度的数量

[传送门](https://www.acwing.com/problem/content/1083/)

```C++
#include <bits/stdc++.h>
using namespace std;
const int N = 35;
int f[N][N], l, r, K, B;

int solve(int n) {
    string s;
    while (n) {
        s += (n % B) + '0';
        n /= B;
    }
    reverse(s.begin(), s.end());
    
    int m = s.size();
    memset(f, -1, sizeof f);
    
    function<int(int, int, bool)> dfs = [&](int i, int cnt, bool isLimit) -> int {
        if (i == m) return cnt == K;
        if (!isLimit && ~f[i][cnt]) return f[i][cnt];
        
        int up = isLimit ? s[i] - '0' : 9, ans = 0;
        for (int d = 0; d <= up; ++ d) {
            if (d > 1 || (d == 1 && cnt == K))  continue;
            ans += dfs(i + 1, cnt + (d == 1), isLimit && d == up);
        }
        
        if (!isLimit)   f[i][cnt] = ans;
        return ans;
    };
    
    return dfs(0, 0, true);
}

int main() {
    scanf("%d%d%d%d", &l, &r, &K, &B);
    printf("%d\n", solve(r) - solve(l - 1));
    return 0;
}
```



## AcWing1082 数字游戏

[传送门](https://www.acwing.com/problem/content/1084/)

```C++
#include <bits/stdc++.h>
using namespace std;
int f[15][15], l, r;

int solve(int n) {
    string s = to_string(n);
    int m = s.size();
    memset(f, -1, sizeof f);
    
    function<int(int, int, bool)> dfs = [&](int i, int pre, bool isLimit) -> int {
        if (i == m) return 1;
        if (!isLimit && ~f[i][pre]) return f[i][pre];
        
        int up = isLimit ? s[i] - '0' : 9, ans = 0;
        for (int d = 0; d <= up; ++ d) {
            if (d < pre)    continue;
            ans += dfs(i + 1, d, isLimit && d == up);
        }
        
        if (!isLimit)   f[i][pre] = ans;
        
        return ans;
    };
    
    return dfs(0, 0, true);
}

int main() {
    while (~scanf("%d%d", &l, &r)) {
        printf("%d\n", solve(r) - solve(l - 1));
    }
    return 0;
}
```



## P2657 windy数

[传送门](https://www.luogu.com.cn/problem/P2657)

```C++
#include <bits/stdc++.h>
using namespace std;
const int N = 15;
int f[N][N], l, r;

int solve(int n) {
    string s = to_string(n);
    int m = s.size();
    memset(f, -1, sizeof f);
    
    function<int(int, int, bool, bool)> dfs = [&](int i, int pre, bool isLimit, bool isNum) -> int {
        if (i == m) return isNum;
        if (!isLimit && isNum && ~f[i][pre])    return f[i][pre];
        
        int up = isLimit ? s[i] - '0' : 9, ans = 0;
        if (!isNum) ans = dfs(i + 1, pre, false, false);
        
        for (int d = 1 - isNum; d <= up; ++ d) {
            if (abs(d - pre) >= 2)
                ans += dfs(i + 1, d, isLimit && d == up, true);
        }
        
        if (!isLimit && isNum)  f[i][pre] = ans;
        
        return ans;
    };
    
    return dfs(0, -2, true, false);
}

int main() {
    scanf("%d%d", &l, &r);
    printf("%d\n", solve(r) - solve(l - 1));
    return 0;
}
```



## AcWing1084 数字游戏II

[传送门](https://www.acwing.com/problem/content/1086/)

```C++
#include <bits/stdc++.h>
using namespace std;
const int N = 1010;
int f[N][N], l, r, MOD;

int solve(int n) {
    string s = to_string(n);
    int m = s.size();
    memset(f, -1, sizeof f);
    
    function<int(int, int, bool)> dfs = [&](int i, int sum, bool isLimit) -> int {
        if (i == m) return sum % MOD == 0;
        if (!isLimit && ~f[i][sum]) return f[i][sum];
        
        int up = isLimit ? s[i] - '0' : 9, ans = 0;
        for (int d = 0; d <= up; ++ d) 
            ans += dfs(i + 1, sum + d, isLimit && d == up);
        
        if (!isLimit)   f[i][sum] = ans;
        return ans;
    };
    
    return dfs(0, 0, true);
}

int main() {
    while (~scanf("%d%d%d", &l, &r, &MOD)) {
        printf("%d\n", solve(r) - solve(l - 1));
    }
    return 0;
}
```



## AcWing1085 不要62

[传送门](https://www.acwing.com/problem/content/1087/)

```C++
#include <bits/stdc++.h>
using namespace std;
int f[15][15], l, r;

int solve(int n) {
    string s = to_string(n);
    int m = s.size();
    memset(f, -1, sizeof f);
    
    function<int(int, int, bool)> dfs = [&](int i, int pre, bool isLimit) -> int {
        if (i == m) return 1;
        if (!isLimit && ~f[i][pre]) return f[i][pre];
        
        int up = isLimit ? s[i] - '0' : 9, ans = 0;
        for (int d = 0; d <= up; ++ d) {
            if (d == 4 || (pre == 6 && d == 2)) continue;
            ans += dfs(i + 1, d, isLimit && d == up);
        }
        
        if (!isLimit)   f[i][pre] = ans;
        return ans;
    };
    
    return dfs(0, 0, true);
}

int main() {
    while (~scanf("%d%d", &l, &r), l || r) {
        printf("%d\n", solve(r) - solve(l - 1));
    }
    return 0;
}
```



## hdu3555 Bomb

[传送门](https://acm.hdu.edu.cn/showproblem.php?pid=3555)

```C++
#include <bits/stdc++.h>
using namespace std;
using LL = long long;
LL f[30][30];

LL solve(LL n) {
    string s = to_string(n);
    int m = s.size();
    memset(f, -1, sizeof f);

    function<LL(int, int, bool)> dfs = [&](int i, int pre, bool isLimit) -> LL {
        if (i == m) return 1;
        if (!isLimit && ~f[i][pre]) return f[i][pre];

        int up = isLimit ? s[i] - '0' : 9;
        LL ans = 0;
        for (int d = 0; d <= up; ++ d) {
            if (pre == 4 && d == 9) continue;
            ans += dfs(i + 1, d, isLimit && d == up);
        }

        if (!isLimit)   f[i][pre] = ans;
        return ans;
    };

    return dfs(0, 0, true) - 1;
}

int main() {
    int T;
    scanf("%d", &T);
    while (T -- ) {
        LL n;
        scanf("%I64d", &n);
        printf("%I64d\n", n - solve(n));
    }
    return 0;
}
```



## POJ3252 Round Numbers

[传送门](http://poj.org/problem?id=3252)

```C++
#include <iostream>
#include <cstring>
#include <functional>
#include <algorithm>
using namespace std;
const int N = 40;
int f[N][N][N], l, r, m;
string s;

int dfs(int i, int x0, int x1, bool isLimit, bool isNum) {
    if (i == m) return x0 >= x1;
    if (!isLimit && isNum && ~f[i][x0][x1]) return f[i][x0][x1];

    int up = isLimit ? s[i] - '0' : 1, ans = 0;
    if (!isNum) ans = dfs(i + 1, x0, x1, false, false);

    for (int d = 1 - isNum; d <= up; ++ d) {
        ans += dfs(i + 1, x0 + (d == 0), x1 + (d == 1), isLimit && d == up, true);
    }

    if (!isLimit && isNum)  f[i][x0][x1] = ans;
    return ans;
}

int solve(int n) {
    s = "";
    while (n) {
        s += (n % 2) + '0';
        n /= 2;
    }
    reverse(s.begin(), s.end());
    m = s.size();
    memset(f, -1, sizeof f);

    return dfs(0, 0, 0, true, false);
}

int main() {
    while (~scanf("%d%d", &l, &r)) {
        printf("%d\n", solve(r) - solve(l - 1));
    }
    return 0;
}
```



## P4124 手机号码

[传送门](https://www.yuque.com/camellia_/zm0aqy/yixdtl344f36kr14)

```C++
#include <bits/stdc++.h>
using namespace std;
using LL = long long ;
LL f[15][11][11][2][2][2], l, r;

LL solve(LL n) {
    string s = to_string(n);
    int m = s.size();
    if (m != 11)    return 0;
    memset(f, -1, sizeof f);

    function<LL(int, int, int, bool, bool, bool, bool)> dfs = [&](int i, int a, int b, bool state, bool four, bool eight, bool isLimit) -> LL {
        if (i == m) return state && !(four && eight) ? 1 : 0;
        if (!isLimit && ~f[i][a][b][state][four][eight])    return f[i][a][b][state][four][eight];

        int up = isLimit ? s[i] - '0' : 9;
        LL ans = 0;
        for (int d = 0; d <= up; ++ d)
            ans += dfs(i + 1, d, a, state || (d == a && d == b), four || (d == 4), eight || (d == 8), isLimit && d == up);

        if (!isLimit)   f[i][a][b][state][four][eight] = ans;
        return ans;
    };

    LL ans = 0;
    for (int i = 1; i <= (s[0] - '0'); i ++ )
        ans += dfs(1, i, 0, false, i == 4, i == 8, i == (s[0] - '0'));

    return ans;
}

int main() {
    scanf("%lld%lld", &l, &r);
    printf("%lld\n", solve(r) - solve(l - 1));
    return 0;
}
```



## LC233 数字1的个数

[传送门](https://leetcode.cn/problems/number-of-digit-one/)

```C++
class Solution {
public:
    int solve(int n) {
        string s = to_string(n);
        int m = s.size(), f[m][m];
        memset(f, -1, sizeof f);

        function<int(int, int, bool)> dfs = [&](int i, int cnt, bool isLimit) -> int {
            if (i == m) return cnt;
            if (!isLimit && ~f[i][cnt]) return f[i][cnt];

            int up = isLimit ? s[i] - '0' : 9, ans = 0;
            for (int d = 0; d <= up; ++ d) {
                ans += dfs(i + 1, cnt + (d == 1), isLimit && d == up);
            }

            if (!isLimit)   f[i][cnt] = ans;

            return ans;
        };

        return dfs(0, 0, true);
    }

    int countDigitOne(int n) {
        return solve(n);
    }
};
```



## LC357 统计各位数字都不同的数字个数

[传送门](https://leetcode.cn/problems/count-numbers-with-unique-digits/)

```C++
class Solution {
public:
    int solve(int n) {
        string s = to_string(n);
        int m = s.size(), f[m][1 << 10];
        memset(f, -1, sizeof f);

        function<int(int, int, bool, bool)> dfs = [&](int i, int mask, bool isLimit, bool isNum) -> int {
            if (i == m) return isNum;
            if (!isLimit && isNum && ~f[i][mask])   return f[i][mask];

            int up = isLimit ? s[i] - '0' : 9, ans = 0;
            if (!isNum) ans = dfs(i + 1, mask, false, false);

            for (int d = 1 - isNum; d <= up; ++ d) {
                if (mask >> d & 1)  continue;
                ans += dfs(i + 1, mask | (1 << d), isLimit && d == up, true);
            }

            if (!isLimit && isNum)  f[i][mask] = ans;

            return ans;
        };

        return dfs(0, 0, true, false) + 1;  // 0符合要求  因此要多加一
    }

    int countNumbersWithUniqueDigits(int n) {
        int x = 1;
        for (int i = 0; i < n; ++ i)    x *= 10;
        -- x;
        return solve(x);
    }
};
```



## LC2376 统计特殊整数

[传送门](https://leetcode.cn/problems/count-special-integers/)

```C++
class Solution {
public:
    int solve(int n) {
        string s = to_string(n);
        int m = s.size(), f[m][1 << 10];
        memset(f, -1, sizeof f);

        function<int(int, int, bool, bool)> dfs = [&](int i, int mask, bool isLimit, bool isNum) -> int {
            if (i == m) return isNum;
            if (!isLimit && isNum && ~f[i][mask])   return f[i][mask];

            int ans = 0, up = isLimit ? s[i] - '0' : 9;
            if (!isNum) ans = dfs(i + 1, mask, false, false);

            for (int d = 1 - isNum; d <= up; ++ d) {   
                if (mask >> d & 1)  continue;    
                ans += dfs(i + 1, mask | (1 << d), isLimit && d == up, true);
            }

            if (!isLimit && isNum)  f[i][mask] = ans;

            return ans;
        };

        return dfs(0, 0, true, false);
    }

    int countSpecialNumbers(int n) {
        return solve(n);
    }
};
```



## LC600 不含连续1的非负整数

[传送门](https://leetcode.cn/problems/non-negative-integers-without-consecutive-ones/)

```C++
class Solution {
public:
    int solve(int n) {
        string s = bitset<32>(n).to_string();
        int m = s.size(), f[m][2];
        memset(f, -1, sizeof f);

        function<int(int, bool, bool)> dfs = [&](int i, bool pre, bool isLimit) -> int {
            if (i == m) return 1;
            if (!isLimit && ~f[i][pre]) return f[i][pre];

            int up = isLimit ? s[i] - '0' : 1, ans = 0;
            for (int d = 0; d <= up; ++ d) {
                if (pre && d == 1)  continue;
                ans += dfs(i + 1, d == 1, isLimit && d == up);
            }

            if (!isLimit)   f[i][pre] = ans;

            return ans;
        };

        return dfs(0, false, true);
    }

    int findIntegers(int n) {
        return solve(n);
    }
};
```



## LC788 旋转数字

[传送门](https://leetcode.cn/problems/rotated-digits/)

```C++
class Solution {
public:
    int diffs[10] = {0, 0, 1, -1, -1, 1, 1, -1, 0, 1};	// 0~9

    int solve(int n) {
        string s = to_string(n);
        int m = s.size(), f[m][2];
        memset(f, -1, sizeof f);

        function<int(int, bool, bool)> dfs = [&](int i, bool hasDiff, bool isLimit) -> int {
            // 只有包含 2/5/6/9 才算一个好数
            if (i == m) return hasDiff;
            if (!isLimit && ~f[i][hasDiff]) return f[i][hasDiff];

            int up = isLimit ? s[i] - '0' : 9, ans = 0;
            for (int d = 0; d <= up; ++ d) 
                if (~diffs[d])  // d 不是 3/4/7
                    ans += dfs(i + 1, hasDiff || diffs[d], isLimit && d == up);

            if (!isLimit)   f[i][hasDiff] = ans;

            return ans;
        };

        return dfs(0, false, true);
    }

    int rotatedDigits(int n) {
        return solve(n);
    }
};
```



## LC902 最大为 N 的数字组合

[传送门](https://leetcode.cn/problems/numbers-at-most-n-given-digit-set/)

```C++
class Solution {
public:
    int atMostNGivenDigitSet(vector<string>& digits, int n) {
        string s = to_string(n);
        int m = s.size(), f[m];
        memset(f, -1, sizeof f);

        function<int(int, bool, bool)> dfs = [&](int i, bool isLimit, bool isNum) -> int {
            if (i == m) return isNum;   
            if (!isLimit && isNum && ~f[i]) return f[i];

            int ans = 0;
            if (!isNum) ans = dfs(i + 1, false, false);
            char up = isLimit ? s[i] : '9'; 
            for (auto &d : digits) {
                if (d[0] > up)  break;
                ans += dfs(i + 1, isLimit && d[0] == up, true);
            }

            if (!isLimit && isNum)  f[i] = ans;
            
            return ans;
        };

        return dfs(0, true, false);
    }
};
```



## LC1012 至少有 1 位重复的数字

[传送门](https://leetcode.cn/problems/numbers-with-repeated-digits/)

```C++
class Solution {
public:
    int solve(int n) {
        string s = to_string(n);
        int m = s.size(), f[m][1 << 10];
        memset(f, -1, sizeof f);

        function<int(int, int, bool, bool)> dfs = [&](int i, int mask, bool isLimit, bool isNum) -> int {
            if (i == m) return isNum;
            if (!isLimit && isNum && ~f[i][mask])   return f[i][mask];

            int up = isLimit ? s[i] - '0' : 9, ans = 0;
            if (!isNum) ans = dfs(i + 1, mask, false, false);

            for (int d = 1 - isNum; d <= up; ++ d) {
                if (mask >> d & 1)  continue;
                ans += dfs(i + 1, mask | (1 << d), isLimit && d == up, true);
            }

            if (!isLimit && isNum)  f[i][mask] = ans;

            return ans;
        };

        return dfs(0, 0, true, false);
    }

    int numDupDigitsAtMostN(int n) {
        return n - solve(n);	
    }
};
```



## LC1067 范围内的数字计数

[传送门](https://leetcode.cn/problems/digit-count-in-range/)

```C++
class Solution {
public:
    int solve(int x, int n) {
        string s = to_string(n);
        int m = s.size(), f[m][m];
        memset(f, -1, sizeof f);

        function<int(int, int, bool, bool)> dfs = [&](int i, int cnt, bool isLimit, bool isNum) -> int {
            if (i == m) return cnt;
            if (!isLimit && isNum && ~f[i][cnt])    return f[i][cnt];

            int up = isLimit ? s[i] - '0' : 9, ans = 0;
            if (!isNum) ans = dfs(i + 1, cnt, false, false);

            for (int d = 1 - isNum; d <= up; ++ d) {
                ans += dfs(i + 1, cnt + (d == x), isLimit && d == up, true);
            }

            if (!isLimit && isNum)  f[i][cnt] = ans;

            return ans;
        };

        return dfs(0, 0, true, false);
    }

    int digitsCount(int d, int low, int high) {
        return solve(d, high) - solve(d, low - 1);
    }
};
```



## LC2719 统计整数数目

[传送门](https://leetcode.cn/problems/count-of-integers/)

```C++
class Solution {
public:
    const int MOD = 1e9 + 7;
    int x, y;

    int solve(string s) {
        int m = s.size(), f[m][min(9 * m, y) + 1];
        memset(f, -1, sizeof f);

        function<int(int, int ,bool isLimit)> dfs = [&](int i, int sum, bool isLimit) -> int {
            if (sum > y)    return 0;
            if (i == m) return x <= sum;
            if (!isLimit && ~f[i][sum]) return f[i][sum];

            int up = isLimit ? s[i] - '0' : 9, ans = 0;
            for (int d = 0; d <= up; ++ d) {
                ans = (ans + dfs(i + 1, sum + d, isLimit && d == up)) % MOD;
            }

            if (!isLimit)   f[i][sum] = ans;

            return ans;
        };

        return dfs(0, 0, true);
    }

    int count(string num1, string num2, int min_sum, int max_sum) {
        x = min_sum, y = max_sum;
        int ans = solve(num2) - solve(num1) + MOD;  // 避免负数
        int sum = 0;
        for (char c : num1) sum += c - '0';
        ans += x <= sum && sum <= y;        // x=num1 是合法的，补回来
        return ans % MOD;
    }
};
```



## LC2801 统计范围内的步进数字数目

[传送门](https://leetcode.cn/problems/count-stepping-numbers-in-range/description/)

```C++
class Solution {
public:
    const int MOD = 1e9 + 7;
    int solve(string s) {
        int m = s.size(), f[m][10];
        memset(f, -1, sizeof f);

        function<int(int, int, bool, bool)> dfs = [&](int i, int pre, bool isLimit, bool isNum) -> int {
            if (i == m) return isNum;
            if (!isLimit && isNum && ~f[i][pre])    return f[i][pre];

            int up = isLimit ? s[i] - '0' : 9, ans = 0;
            if (!isNum) ans = dfs(i + 1, pre, false, false);

            for (int d = 1 - isNum; d <= up; ++ d) {
                if (pre == -1 || abs(d - pre) == 1)
                    ans = (ans + dfs(i + 1, d, isLimit && d == up, true)) % MOD;
            }

            if (!isLimit && isNum)  f[i][pre] = ans;

            return ans;
        };

        return dfs(0, -1, true, false);
    }

    bool valid(string s) {
        for (int i = 1; i < s.size(); ++ i) 
            if (abs(int(s[i]) - int(s[i - 1])) != 1)
                return false;
        return true;
    }

    int countSteppingNumbers(string low, string high) {
        return (solve(high) - solve(low) + MOD + valid(low)) % MOD;
    }
};
```

