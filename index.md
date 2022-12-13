<h1 align = "center">CSP-S 2022 题解</h1>

呜呜呜我为什么这么菜 /kk

## 假期计划（holiday）

枚举景点 B 和景点 C，求出以每个点为景点 B 的情况下，权值最大/次大/第三大的景点 A，直接 $3\times3$ 更新答案。两两距离直接 $n$ 次 BFS 预处理好就可以。

```c++
#include <bits/stdc++.h>
using namespace std;

const int N = 2510, M = 20010, inf = 0x3f3f3f3f;

int n, m, K;
long long val[N];

int to[M], nxt[M], head[N], cnt;
void add(int x, int y) {
    to[++ cnt] = y;
    nxt[cnt] = head[x];
    head[x] = cnt;
}

int dis[N];
bool ok[N][N];
void bfs(int x) {
    memset(dis, 0x3f, sizeof(dis));
    queue <int> q;
    q.push(x);
    dis[x] = -1;
    while (!q.empty()) {
        int u = q.front();
        q.pop();
        if (dis[u] == K) continue;
        for (int i = head[u]; i; i = nxt[i])
            if (dis[to[i]] == inf) {
                dis[to[i]] = dis[u] + 1;
                q.push(to[i]);
            }
    }
    for (int i = 1; i <= n; ++ i)
        if (i != x && dis[i] <= K) ok[x][i] = true;
}

int mx[N], cmx[N], ccmx[N], a[2][3];

int main() {
    freopen("holiday.in", "r", stdin);
    freopen("holiday.out", "w", stdout);
    scanf("%d%d%d", &n, &m, &K);
    for (int i = 2; i <= n; ++ i) scanf("%lld", val + i);
    int u, v;
    while (m --) {
        scanf("%d%d", &u, &v);
        add(u, v);
        add(v, u);
    }
    for (int i = 1; i <= n; ++ i) bfs(i);
    for (int i = 2; i <= n; ++ i)
        if (ok[1][i])
            for (int j = 2; j <= n; ++ j)
                if (ok[i][j]) {
                    if ((! mx[j]) || val[i] > val[mx[j]]) {
                        ccmx[j] = cmx[j];
                        cmx[j] = mx[j];
                        mx[j] = i;
                    } else if ((! cmx[j]) || val[i] > val[cmx[j]]) {
                        ccmx[j] = cmx[j];
                        cmx[j] = i;
                    } else if ((! ccmx[j]) || (val[i] > val[ccmx[j]])) ccmx[j] = i;
                }
    long long ans = 0;
    for (int i = 2; i <= n; ++ i)
        for (int j = 2; j <= n; ++ j)
            if (ok[i][j]) {
                a[0][0] = mx[i], a[0][1] = cmx[i], a[0][2] = ccmx[i], a[1][0] = mx[j], a[1][1] = cmx[j], a[1][2] = ccmx[j];
                for (int l1 = 0; l1 < 3; ++ l1)
                    for (int l2 = 0; l2 < 3; ++ l2)
                        if (a[0][l1] && a[1][l2] && a[0][l1] != a[1][l2] && a[0][l1] != j && a[1][l2] != i)
                            ans = max(ans, val[i] + val[j] + val[a[0][l1]] + val[a[1][l2]]);
            }
    printf("%lld\n", ans);
    return 0;
}
```

## 策略游戏（game）

用线段树分别维护序列 $a$ 和序列 $b$ 的区间最小值，最大值，最小正数，最大负数，之后 $4\times4$ 求答案即可，不需要分类讨论。

```c++
#include <bits/stdc++.h>
using namespace std;

const int N = 100010, inf = 0x3f3f3f3f;
const long long infl = 0x3f3f3f3f3f3f3f3f;

int n, m, q, a[2][N];

struct node {
    int a[4];
    node() {}
    node(int a0, int a1, int a2, int a3) { a[0] = a0, a[1] = a1, a[2] = a2, a[3] = a3; }
    int & operator [] (const int x) { return a[x]; }
    node operator + (node x) { return node(max(a[0], x[0]), min(a[1], x[1]), min(a[2], x[2]), max(a[3], x[3])); }
} tr[2][N << 2];

void build(bool op, int x, int l, int r) {
    if (l == r) {
        if (a[op][l] >= 0) tr[op][x] = node(a[op][l], a[op][l], a[op][l], -inf);
        else tr[op][x] = node(a[op][l], a[op][l], inf, a[op][l]);
        return;
    }
    int mid = ((l + r) >> 1);
    build(op, x << 1, l, mid);
    build(op, x << 1 | 1, mid + 1, r);
    tr[op][x] = tr[op][x << 1] + tr[op][x << 1 | 1];
}

node query(bool op, int x, int l, int r, int L, int R) {
    if (L <= l && r <= R) return tr[op][x];
    int mid = ((l + r) >> 1);
    if (L > mid) return query(op, x << 1 | 1, mid + 1, r, L, R);
    if (R <= mid) return query(op, x << 1, l, mid, L, R);
    return query(op, x << 1, l, mid, L, R) + query(op, x << 1 | 1, mid + 1, r, L, R);
}

int main() {
    freopen("game.in", "r", stdin);
    freopen("game.out", "w", stdout);
    scanf("%d%d%d", &n, &m, &q);
    for (int i = 1; i <= n; ++ i) scanf("%d", a[0] + i);
    for (int i = 1; i <= m; ++ i) scanf("%d", a[1] + i);
    build(false, 1, 1, n);
    build(true, 1, 1, m);
    while (q --) {
        int l1, r1, l2, r2;
        scanf("%d%d%d%d", &l1, &r1, &l2, &r2);
        node ls = query(false, 1, 1, n, l1, r1), rs = query(true, 1, 1, m, l2, r2);
        long long ans = -infl;
        for (int i = 0; i < 4; ++ i)
            if (ls[i] != inf && ls[i] != -inf) {
                long long tmp = infl;
                for (int j = 0; j < 4; ++j)
                    if (rs[j] != inf && rs[j] != -inf) tmp = min(tmp, 1ll * ls[i] * rs[j]);
                ans = max(ans, tmp);
            }
        printf("%lld\n", ans);
    }
    return 0;
}
```

## 星战（galaxy）

题目中要求满足整个图是奇环内向森林，但也可以直接看作每个点的出度恰好为 $1$。给每个点随机赋一个权值 $a_i$，并构造全图 hash 值为 $\sum\limits_{i=1}^{n}a_i\times out_i$，其中 $out_i$ 表示 $i$ 节点的出度。当且仅当 hash 值为全图点权和时答案为 `YES`。

```c++
#include <bits/stdc++.h>
using namespace std;
#define int unsigned long long

const int N = 500010;

int n, m, q, a[N], in[N], out[N], sum[N], tot, now, nv[N];

signed main() {
    freopen("galaxy.in", "r", stdin);
    freopen("galaxy.out", "w", stdout);
    scanf("%llu%llu", &n, &m);
    for (int i = 1; i <= n; ++i) {
        a[i] = rand();
        tot += a[i];
    }
    for (int i = 1; i <= m; ++ i) {
        scanf("%llu%llu", in + i, out + i);
        sum[out[i]] += a[in[i]];
        now += a[in[i]];
    }
    for (int i = 1; i <= n; ++ i) nv[i] = sum[i];
    scanf("%llu", &q);
    while (q--) {
        int t;
        scanf("%llu", &t);
        if (t == 1) {
            int u, v;
            scanf("%llu%llu", &u, &v);
            nv[v] -= a[u];
            now -= a[u];
        } else if (t == 2) {
            int u;
            scanf("%lld", &u);
            now -= nv[u];
            nv[u] = 0;
        } else if (t == 3) {
            int u, v;
            scanf("%llu%llu", &u, &v);
            nv[v] += a[u];
            now += a[u];
        } else {
            int u;
            scanf("%lld", &u);
            now += sum[u] - nv[u];
            nv[u] = sum[u];
        }
        if (now == tot) puts("YES");
        else puts("NO");
    }
    return 0;
}
```

## 数据传输（transmit）

$k=1$ 和 $k=2$ 答案一定在链上，可以直接 dp，树上静态 dp 直接倍增矩阵乘法即可。$k=3$ 时可能会走到链外，整理一下，设 $dp_{i,j}$ 表示到第 $i$ 个点为止，所有选的点中距离 $i$ 最近的点与 $i$ 的距离为 $j$ 的最小花费，设链上 $i$ 的前一个点为 $v$，所有与 $i$ 有连边的点中最小点权为 $mn_i$。

$$
dp_{i,0}=(\min\limits_{j=0}^{k-1}dp_{v,j})+val_i
$$

$$
dp_{i,1}=\min(dp_{v,0},dp_{v,1}+mn_i)
$$

$$
dp_{i,2}=dp_{i,1}
$$

倍增和树上矩阵实现，矩阵向上和向下转移都要存。

```c++
#include <bits/stdc++.h>
using namespace std;
#define int long long

const int N = 200010, M = 21, inf = 0x3c3c3c3c3c3c3c3c, _inf = 0xc3c3c3c3c3c3c3c3;

int n, K, q, val[N], mn[N];

int to[N << 1], nxt[N << 1], head[N], cnt;
void add(int x, int y) {
    to[++ cnt] = y;
    nxt[cnt] = head[x];
    head[x] = cnt;
}

int fa[N][30], dep[N];
struct mat {
    int a[3][3];
    mat() { memset(a, 0x3c, sizeof(a)); }
    int * operator [] (int x) { return a[x]; }
    friend mat operator * (mat x, mat y) {
        mat z;
        for (int i = 0; i < K; ++ i) {
            for (int j = 0; j < K; ++ j) {
                for (int k = 0; k < K; ++ k) z[i][j] = min(z[i][j], x[i][k] + y[k][j]);
            }
        }
        return z;
    }
} up[N][M], down[N][M];
void dfs(int x) {
    for (int i = head[x]; i; i = nxt[i]) {
        if (to[i] == fa[x][0]) continue;
        dep[to[i]] = dep[x] + 1;
        fa[to[i]][0] = x;
        dfs(to[i]);
    }
}
void init() {
    for (int i = 1; i <= n; ++i) {
        for (int j = 0; j < K; ++j) up[i][0][j][0] = down[i][0][j][0] = val[i];
        if (K >= 2) up[i][0][0][1] = down[i][0][0][1] = 0;
        if (K >= 3) up[i][0][1][2] = down[i][0][1][2] = 0, up[i][0][1][1] = down[i][0][1][1] = mn[i];
    }
    for (int i = 1; i <= 19; ++ i) {
        for (int j = 1; j <= n; ++ j) {
            fa[j][i] = fa[fa[j][i - 1]][i - 1];
            up[j][i] = up[j][i - 1] * up[fa[j][i - 1]][i - 1];
            down[j][i] = down[fa[j][i - 1]][i - 1] * down[j][i - 1];
        }
    }
}

int lca(int x, int y) {
    if (dep[x] < dep[y]) swap(x, y);
    for (int i = 19; ~ i; -- i) {
        if (dep[fa[x][i]] < dep[y]) continue;
        x = fa[x][i];
    }
    if (x == y) return x;
    for (int i = 19; ~ i; -- i) {
        if (fa[x][i] == fa[y][i]) continue;
        x = fa[x][i], y = fa[y][i];
    }
    return fa[x][0];
}

mat jumpup(int x, int y) {
    mat res;
    for (int i = 0; i < K; ++ i) res[i][i] = 0;
    for (int i = 19; ~ i; -- i) {
        if (dep[fa[x][i]] < dep[y]) continue;
        res = res * up[x][i];
        x = fa[x][i];
    }
    return res;
}

mat jumpdown(int x, int y) {
    mat res;
    for (int i = 0; i < K; ++ i) res[i][i] = 0;
    for (int i = 19; ~ i; -- i) {
        if (dep[fa[x][i]] < dep[y]) continue;
        res = down[x][i] * res;
        x = fa[x][i];
    }
    return res;
}

signed main() {
    freopen("transmit.in", "r", stdin);
    freopen("transmit.out", "w", stdout);
    scanf("%lld%lld%lld", &n, &q, &K);
    memset(mn, 0x3c, sizeof(mn));
    for (int i = 1; i <= n; ++ i) scanf("%lld", val + i);
    int u, v;
    for (int i = 1; i < n; ++ i) {
        scanf("%lld%lld", &u, &v);
        add(u, v);
        add(v, u);
        mn[u] = min(mn[u], val[v]);
        mn[v] = min(mn[v], val[u]);
    }
    dep[1] = 1;
    dfs(1);
	fa[1][0] = n + 1;
	dep[0] = -1;
    init();
    while (q --) {
        scanf("%lld%lld", &u, &v);
        if (dep[u] < dep[v]) swap(u, v);
        int lc = lca(u, v);
        if (lc == v) {
            mat res;
            res[0][0] = val[u];
            res = res * jumpup(fa[u][0], fa[v][0]);
            printf("%lld\n", res[0][0]);
        } else {
            mat res;
            res[0][0] = val[u];
            res = res * jumpup(fa[u][0], fa[lc][0]);
            res = res * jumpdown(v, lc);
            printf("%lld\n", res[0][0]);
        }
    }
    return 0;
}
```
