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

## AcWing 1096. 地牢大师
[传送门](https://www.acwing.com/problem/content/description/1098/)
```C++
#include <bits/stdc++.h>
using namespace std;
const int N = 110;
struct Point {
    int x, y, z;
} q[N * N * N];
int dx[6] = {-1, 0, 1, 0, 0, 0};
int dy[6] = {0, 1, 0, -1, 0, 0};
int dz[6] = {0, 0, 0, 0, 1, -1};
char g[N][N][N];
int dist[N][N][N], L, R, C;

int bfs(Point S, Point E) {
    memset(dist, -1, sizeof dist);
    dist[S.x][S.y][S.z] = 0;
    int hh = 0, tt = 0;
    q[0] = S;
    
    while (hh <= tt) {
        Point t = q[hh ++ ];
        if (t.x == E.x && t.y == E.y && t.z == E.z)
            return dist[t.x][t.y][t.z];
        for (int i = 0; i < 6; ++ i) {
            int a = t.x + dx[i], b = t.y + dy[i], c = t.z + dz[i];
            if (a < 0 || a >= L || b < 0 || b >= R || c < 0 || c >= C)
                continue;
            if (g[a][b][c] == '#' || ~dist[a][b][c])
                continue;
            dist[a][b][c] = dist[t.x][t.y][t.z] + 1;
            q[++ tt] = {a, b, c};
        }
    }
    
    return -1;
}

int main() {
    while (cin >> L >> R >> C, L || R || C) {
        Point S, E;
        for (int i = 0; i < L; ++ i) {
            for (int j = 0; j < R; ++ j) {
                for (int k = 0; k < C; ++ k) {
                    cin >> g[i][j][k];
                    if (g[i][j][k] == 'S') {
                        S = {i, j, k};
                    } else if (g[i][j][k] == 'E') {
                        E = {i, j, k};
                    }
                }
            }
        }
        
        int t = bfs(S, E);
        if (t == -1)    puts("Trapped!");
        else    printf("Escaped in %d minute(s).\n", t);
    }
}
```
