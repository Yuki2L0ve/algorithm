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

## AcWing1100. 抓住那头牛
[传送门](https://www.acwing.com/problem/content/description/1102/)
```C++
#include <bits/stdc++.h>
using namespace std;
const int N = 1e5 + 10;
int dist[N], q[N], n, k;

int bfs() {
    memset(dist, -1, sizeof dist);
    int hh = 0, tt = 0;
    q[0] = n, dist[n] = 0;
    
    while (hh <= tt) {
        int u = q[hh ++ ];
        if (u == k) return dist[u];
        if (u - 1 >= 0 && dist[u - 1] == -1) {
            dist[u - 1] = dist[u] + 1;
            q[++ tt] = u - 1;
        }
        if (u + 1 < N && dist[u + 1] == -1) {
            dist[u + 1] = dist[u] + 1;
            q[++ tt] = u + 1;
        }
        if (u * 2 < N && dist[2 * u] == -1) {
            dist[2 * u] = dist[u] + 1;
            q[++ tt] = 2 * u;
        }
    }
    
    return -1;
}

int main() {
    scanf("%d%d", &n, &k);
    printf("%d\n", bfs());
}
```

## AcWing4218. 翻转
[传送门](https://www.acwing.com/problem/content/4221/)
```C++
#include <bits/stdc++.h>
using namespace std;
const int N = 20, INF = 0x3f3f3f3f;
int dx[5] = {0, -1, 0, 1, 0}, dy[5] = {0, 0, 1, 0, -1};
int g[N][N], bak[N][N], path[N][N], ans[N][N], n, m, res = INF;

void turn(int x, int y) {
    for (int i = 0; i < 5; ++ i) {
        int a = x + dx[i], b = y + dy[i];
        if (a < 0 || a >= n || b < 0 || b >= m)
            continue;
        g[a][b] ^= 1;
    }
    ++ path[x][y];
}

void solve() {
    for (int k = 0; k < 1 << m; ++ k) { // 枚举第1行的1<<m种翻转状态
        memcpy(bak, g, sizeof g);
        
        memset(path, 0, sizeof path);
        int cnt = 0;    // 翻转次数
        
        for (int j = 0; j < m; ++ j) {  // 如果第一行中有1则把它翻转
            if (k >> j & 1) {
                turn(0, j);
                ++ cnt;
            }
        }
        
        // 确定了第一行状态后，则约束了后面所有行的状态
        for (int i = 0; i < n - 1; ++ i) {
            for (int j = 0; j < m; ++ j) {
                if (g[i][j] == 1) {
                    turn(i + 1, j); // 如果上一行中该列有1，则由下一行中该列来翻转解决
                    ++ cnt;
                }
            }
        }
        
        // 枚举第n行，如果还有1，则说明这种方案是非法的(因为已经没有第n+1行来翻转解决第n行了)
        bool ok = true;
        for (int j = 0; j < m; ++ j) {  
            if (g[n - 1][j] == 1) {
                ok = false;
                break;
            }
        }
        
        if (ok && res > cnt) {
            res = cnt;  // 翻转次数最少的那种解决方案
            memcpy(ans, path, sizeof path);
        }
        
        memcpy(g, bak, sizeof bak);
    }
}

int main() {
    scanf("%d%d", &n, &m);
    for (int i = 0; i < n; ++ i)
        for (int j = 0; j < m; ++ j)
            scanf("%d", &g[i][j]);
    
    solve();
    
    if (res == INF) {
        puts("IMPOSSIBLE");
    } else {
        for (int i = 0; i < n; ++ i) {
            for (int j = 0; j < m; ++ j)
                printf("%d ", ans[i][j]);
            puts("");
        }
    }

}
```

## AcWing4219. 找倍数
[传送门](https://www.acwing.com/problem/content/4222/)
```C++
#include <bits/stdc++.h>
using namespace std;
using ULL = unsigned long long ;
int n;

int main() {
    while (~scanf("%d", &n), n) {
        queue<ULL> q;
        while (!q.empty())  q.pop();
        q.push(1);
        
        while (q.size()) {
            ULL x = q.front();  q.pop();
            if (x % n == 0) {
                cout << x << endl;
                break;
            }
            q.push(x * 10);
            q.push(x * 10 + 1);
        }
    }
}
```

## AcWing4220. 质数路径
[传送门](https://www.acwing.com/problem/content/4223/)
```C++
#include <bits/stdc++.h>
using namespace std;
const int N = 1e4 + 10;
int dist[N], q[N], primes[N], cnt, T;
bool st[N];

void init(int n) {
    for (int i = 2; i <= n; ++ i) {
        if (!st[i]) 
            primes[cnt ++ ] = i;
        for (int j = 0; j < cnt && primes[j] <= n / i; ++ j) {
            st[i * primes[j]] = true;
            if (i % primes[j] == 0) 
                break;
        }
    }
}

int change(int t, int i, int j) {
    if (i == 0)    {
        int pre = t / 10;
        return pre * 10 + j;
    } else if (i == 1) {   
        int pre = t / 100, last = t % 10;
        return pre * 100 + j * 10 + last;
    } else if (i == 2) {
        int pre = t / 1000, last = t % 100;
        return pre * 1000 + j * 100 + last;
    } else if (i == 3) {
        if (!j) return 0;
        return j * 1000 + t % 1000;
    }
}

int bfs(int a, int b) {
    memset(dist, -1, sizeof dist);
    int hh = 0, tt = 0;
    q[0] = a;
    dist[a] = 0;
    
    while (hh <= tt) {
        int u = q[hh ++ ];
        if (u == b) return dist[u];
        
        for (int i = 0; i < 4; ++ i) {
            for (int j = 0; j <= 9; ++ j) {
                int v = change(u, i, j);
                if (!v || v == u || st[v] || dist[v] != -1)
                    continue;
                q[++ tt] = v;
                dist[v] = dist[u] + 1;
            }
        }
    }
    
    return -1;
}

int main() {
    int T; 
    scanf("%d", &T);
    init(N);
    while (T -- )  {
        int a, b; 
        scanf("%d%d", &a, &b);
        int t = bfs(a, b);
        if (t == -1) puts("Impossible");
        else    printf("%d\n", t);
    }
}
```

## AcWing4221. 洗牌
[传送门](https://www.acwing.com/problem/content/4224/)
```C++
#include <bits/stdc++.h>
using namespace std;
string a, b, c;
int n, T;

int bfs() {
    unordered_map<string, int> dist;
    queue<string> q;
    string s;   // 初始状态
    // 洗牌操作之后S12的最底下的那一张牌是 S2 的最底下的那一张牌。S12的最顶上的那一张牌，是S1最上面的那一张牌。
    // 中间的牌交叉放置，记住这个操作叫做洗牌！因此是先加b再加a
    for (int i = 0; i < n; ++ i)    s += b[i], s += a[i];
    dist[s] = 1;    // 此时已经花费了1次洗牌操作
    q.push(s);
    
    while (q.size()) {
        string u = q.front();   q.pop();
        if (u == c) return dist[u];
        string v;
        for (int i = 0; i < n; ++ i)    v += u[i + n], v += u[i];
        if (!dist.count(v)) {
            dist[v] = dist[u] + 1;
            q.push(v);
        }
    }
    
    return -1;
}

int main() {
    scanf("%d", &T);
    for (int i = 1; i <= T; ++ i) {
        cin >> n >> a >> b >> c;
        printf("%d %d\n", i, bfs());
    }
}
```

## AcWing4222. 罐子
[传送门](https://www.acwing.com/problem/content/4225/)
```C++
#include <bits/stdc++.h>
using namespace std;
struct Data{
    int x, y;
    string s;
};
string operations[6]={"FILL(1)", "FILL(2)", "DROP(1)", "DROP(2)", "POUR(1,2)", "POUR(2,1)"};
int dist[110][110], a, b, c;

int bfs() {
    memset(dist,-1,sizeof dist);
    queue<Data> q;
    q.push({0,0,""});
    dist[0][0]=0;
    
    while (q.size()) {
        Data t = q.front();    q.pop();
        if (t.x == c|| t.y == c) {
            cout << dist[t.x][t.y] << endl;
            for (auto i : t.s) {
                cout << operations[i] << endl;
            }
            return 0;
        }
        
        int t1 = min(t.x, b - t.y);
        int t2 = min(a - t.x, t.y);
        int vx[] = {a - t.x, 0, -t.x, 0, -t1, t2};
        int vy[] = {0, b - t.y, 0, -t.y, t1, -t2};
        for (int i = 0; i < 6; ++ i) {
            int x = t.x + vx[i];
            int y = t.y + vy[i];
            if (dist[x][y] == -1) {
                dist[x][y] = dist[t.x][t.y] + 1;
                char op = i;
                string s = t.s + op;
                q.push({x, y, s});
            }
        }
    }
    
    cout << "impossible" << endl;
    return 0;
}

int main() {
    cin >> a >> b >> c;
    bfs();
}
```
