[kuangbin带你飞]专题一简单搜索：[题单](https://vjudge.net/contest/65959)
## AcWing1114. 棋盘问题
[传送门](https://www.acwing.com/problem/content/1116/)
```C++
#include <bits/stdc++.h>
using namespace std;
const int N = 10;
char g[N][N];
bool st[N];
int n, k, ans;

void dfs(int i, int cnt) {
    if (cnt == k) {
        ++ ans;
        return ;
    }
    if (i >= n) return ;
    
    dfs(i + 1, cnt);
    
    for (int j = 0; j < n; ++ j) {
        if (g[i][j] == '#' && !st[j]) {
            st[j] = true;
            dfs(i + 1, cnt + 1);
            st[j] = false;
        }
    }
}

int main() {
    while (~scanf("%d%d", &n, &k), ~n && ~k) {
        ans = 0;
        for (int i = 0; i < n; ++ i)    scanf("%s", g[i]);
        dfs(0, 0);
        printf("%d\n", ans);
    }
}
```
