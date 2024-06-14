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
struct Data {
    int a, b;
    string s;
};
string operations[6] = {"FILL(1)", "FILL(2)", "DROP(1)", "DROP(2)", "POUR(1,2)", "POUR(2,1)"};
int dist[110][110], A, B, C;

int bfs() {
    memset(dist, -1, sizeof dist);
    queue<Data> q;
    q.push({0, 0, ""});
    dist[0][0] = 0;
    
    while (q.size()) {
        Data t = q.front(); q.pop();
        if (t.a == C || t.b == C) {
            printf("%d\n", dist[t.a][t.b]);
            for (char c : t.s)
                cout << operations[c - '0'] << endl;
            return 0;
        }
        
        // 从容器 A 到容器 B 的最大可能倒水量，由容器 A 的当前水量 (t.x) 和容器 B 空余容量 (b - t.y) 中的较小值决定。
        int pourAB = min(t.a, B - t.b);
        // 从容器 B 到容器 A 的最大可能倒水量，由容器 B 的当前水量 (t.y) 和容器 A 空余容量 (a - t.x) 中的较小值决定。
        int pourBA = min(A - t.a, t.b);
        // dx[]和dy[]: 每个操作对应数组中的一个位置，dx[i] 对应于容器 A 的水量变化，dy[i] 对应于容器 B 的水量变化。
        /**
         * {A - t.a, 0}：填满容器 A。将容器 A 的水量增加到最大容量 (a)。
            {0, B - t.b}：填满容器 B。将容器 B 的水量增加到最大容量 (b)。
            {-t.a, 0}：清空容器 A。将容器 A 的水量减少到 0。
            {0, -t.b}：清空容器 B。将容器 B 的水量减少到 0。
            {-pourAB, pourAB}：从容器 A 到容器 B 倒水 pourAB 单位。从容器 A 中减去 pourAB 单位的水，同时将同等数量的水加到容器 B。
            {pourBA, -pourBA}：从容器 B 到容器 A 倒水 pourBA 单位。从容器 B 中减去 pourBA 单位的水，同时将同等数量的水加到容器 A。
        **/
        int dx[] = {A - t.a, 0, -t.a, 0, -pourAB, pourBA};
        int dy[] = {0, B - t.b, 0, -t.b, pourAB, -pourBA};
        
        for (int i = 0; i < 6; ++ i) {
            int a = t.a + dx[i], b = t.b + dy[i];
            if (dist[a][b] == -1) {
                dist[a][b] = dist[t.a][t.b] + 1;
                q.push({a, b, t.s + to_string(i)});
            }
        }
    }
    
    puts("impossible");
    return 0;
}

int main() {
    scanf("%d%d%d", &A, &B, &C);
    bfs();
}
```

## AcWing 4223. 点火游戏
[传送门](https://www.acwing.com/problem/content/description/4226/)
```C++
#include <bits/stdc++.h>
using namespace std;
using PII = pair<int, int>;
#define x first
#define y second
const int N = 15, INF = 0x3f3f3f3f;
int dx[4] = {-1, 0, 1, 0}, dy[4] = {0, 1, 0, -1};
int dist[N][N], n, m, T;
char g[N][N];
vector<PII> v;

int bfs(PII one, PII two) {
    int cnt = 0, ans = 0;   // cnt记录点燃的草地个数
    queue<PII> q;
    memset(dist, -1, sizeof dist);
    q.push(one), ++ cnt;
    if (one != two) q.push(two), ++ cnt;
    dist[one.x][one.y] = dist[two.x][two.y] = 0;
    
    while(q.size()) {
        PII t = q.front();  q.pop();
        for(int i = 0; i < 4; ++ i) {
            int a = t.x + dx[i], b = t.y + dy[i];
            if (a < 0 || a >= n || b < 0 || b >= m || g[a][b] == '.' || ~dist[a][b]) continue;
            dist[a][b] = dist[t.x][t.y] + 1;
            ans = max(ans, dist[a][b]);
            ++ cnt;
            q.push({a, b});
        }
    }
    
    // 如果点燃了所有草地
    if (cnt == (int)v.size())   return ans;
    return INF;
}

void solve(int id) {
    cin >> n >> m;
    v.clear();
    for (int i = 0; i < n; ++ i) cin >> g[i];
    for (int i = 0; i < n; ++ i) {
        for(int j = 0; j < m; ++ j) {
            if (g[i][j]=='#') v.push_back({i, j}); 
        }
    }
    
    if (v.size() == 0) {
        printf("Case %d: %d\n", id, -1);
        return ;
    }
    if (v.size() <= 2) {
        printf("Case %d: %d\n", id, 0);
        return ;
    }
    
    int ans = INF;
    for (int i = 0; i < v.size(); ++ i) 
        for (int j = i + 1; j < v.size(); ++ j) 
            ans = min(ans, bfs(v[i], v[j]));
    
    if (ans == INF) {
        printf("Case %d: %d\n", id, -1);
    } else {
        printf("Case %d: %d\n", id, ans);
    }
}

int main() {
    cin >> T;
    for (int i = 1; i <= T; ++ i){
        solve(i);
    }
}
```

## AcWing 4224. 起火迷宫
[传送门](https://www.acwing.com/problem/content/description/4227/)
```C++
#include <bits/stdc++.h>
#define x first
#define y second
using namespace std;
using PII = pair<int, int>;
const int N = 1010, INF = 0x3f3f3f3f;
int dx[] = {-1, 0, 1, 0}, dy[] = {0, 1, 0, -1};
char g[N][N];
int d[N][N], dist[N][N], n, m, T;
vector<PII> fire;

void bfs() {
    memset(d, 0x3f, sizeof d);  // 注意是初始化为无穷大而不能是-1
    queue<PII> q;
    for (auto& [x, y] : fire)
        q.push({x, y}), d[x][y] = 0;

    while (q.size()) {
        PII t = q.front();  q.pop();
        for (int i = 0; i < 4; ++ i) {
            int a = t.x + dx[i], b = t.y + dy[i];
            if (a < 1 || a > n || b < 1 || b > m) continue;
            if (d[a][b] != INF || g[a][b] != '.') continue;
            d[a][b] = d[t.x][t.y] + 1;
            q.push({a, b});
        }
    }

}

int solve(int sx, int sy) {
    if (sx == 1 || sx == n || sy == 1 || sy == m) return 1;
    memset(dist, 0x3f, sizeof dist);    // 注意是初始化为无穷大而不能是-1
    queue<PII> q;
    q.push({sx, sy});
    dist[sx][sy] = 0;

    while (q.size()) {
        PII t = q.front();  q.pop();
        for (int i = 0; i < 4; ++ i) {
            int a = t.x + dx[i], b = t.y + dy[i];
            if (a < 1 || a > n || b < 1 || b > m) continue;
            if (dist[a][b] != INF|| g[a][b] != '.' || dist[t.x][t.y] + 1 >= d[a][b]) continue;
            dist[a][b] = dist[t.x][t.y] + 1;
            // 之所以+1是因为此时在边界，在走一步即可出迷宫
            if (a == 1 || a == n || b == 1 || b == m) return dist[a][b] + 1;
            q.push({a, b});
        }

    }

    return -1;
}

int main() {
    cin >> T;
    while (T -- ) {
        cin >> n >> m;
        for (int i = 1; i <= n; ++ i) cin >> g[i] + 1;
        fire.clear();
        
        int sx, sy;
        for (int i = 1; i <= n; ++ i)
            for (int j = 1; j <= m; ++ j)
                if (g[i][j] == 'F') fire.push_back({i, j});
                else if (g[i][j] == 'J')    sx = i, sy = j;

        bfs();    
        
        int ans = solve(sx, sy);
        if (ans == -1)  puts("IMPOSSIBLE");
        else    printf("%d\n", ans);

    }
}
```

## AcWing4225. 石油储备
[传送门](https://www.acwing.com/problem/content/4228/)
```C++
#include <bits/stdc++.h>
using namespace std;
using PII = pair<int, int>;
#define x first
#define y second
const int N = 110;
int dx[] = {-1, -1, -1, 0, 0, 1, 1, 1};
int dy[] = {-1, 0, 1, -1, 1, -1, 0, 1};
int n, m;
char g[N][N];
bool st[N][N];
PII q[N * N];

int bfs(int sx, int sy) {
    int hh = 0, tt = 0;
    q[0] = {sx, sy};
    st[sx][sy] = true;
    
    while (hh <= tt) {
        PII t = q[hh ++ ];
        for (int i = 0; i < 8; ++ i) {
            int a = t.x + dx[i], b = t.y + dy[i];
            if (a < 0 || a >= n || b < 0 || b >= m || g[a][b] == '*' || st[a][b])
                continue;
            q[++ tt] = {a, b};
            st[a][b] = true;
        }
    }
}

int main() {
    while (~scanf("%d%d", &n, &m), n || m) {
        memset(st, 0, sizeof st);
        for (int i = 0; i < n; ++ i)    scanf("%s", g[i]);
        
        int ans = 0;
        for (int i = 0; i < n; ++ i) {
            for (int j = 0; j < m; ++ j) {
                if (g[i][j] == '@' && !st[i][j]) {
                    bfs(i, j);
                    ++ ans;
                }
            }
        }
        
        printf("%d\n", ans);
    }
}
```
