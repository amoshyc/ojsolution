#####################################
[cf100814] C. Connecting Graph
#####################################

.. sidebar:: Tags

    - ``tag_mst``
    - ``tag_lca``
    - ``tag_offline``

.. contents:: TOC
    :depth: 2

******************************************************
`題目 <http://codeforces.com/gym/100814/problem/C>`_
******************************************************

N 個節點，Q 個操作，操作種類為：
1. ``1 u v``: 加入一條無向邊 (u, v)
2. ``2 u v``: u, v 是在第幾筆操作連通的？尚未連通輸出 -1

************************
Specification
************************

::

    1 <= N <= 10^5
    2 <= Q <= 10^5

************************
分析
************************

.. note:: 最小瓶頸值

如果將操作 ``1 u v`` 視為在無向圖上，在 u, v 之間加入一條權重 id 的邊（id 即第幾次操作）。那操作 ``2 u v`` 即為在這個圖上，找 u, v 之間的最小瓶頸值，而這是 mst + lca 的題目。

u, v 的最小瓶頸值定義為，u, v 的所有路徑中，每條路徑有其經過的最大權重（瓶頸值），在所有路徑中，最小的最大權重即為 u, v 的最小瓶頸值。可以發現，產生 u, v 最小瓶頸值的那條邊必在 mst 中。想想 Kruskal 建 mst 即可發現這是正確的。無向圖上，離線找任兩點之間的最小瓶頸值可以用 mst + lca 解決。

************************
AC Code
************************

.. code-block:: cpp
    :linenos:

    #include <bits/stdc++.h>
    using namespace std;

    const int MAX_N = 100000;
    const int MAX_LOG_N = 17;
    int N, Q;

    // dsu
    int dsu[MAX_N];
    int root(int u) {
        if (dsu[u] < 0) return u;
        return (dsu[u] = root(dsu[u]));
    }
    void unite(int u, int v) {
        u = root(u);
        v = root(v);
        if (u == v) return;
        if (-dsu[u] > -dsu[v]) swap(u, v);
        dsu[v] += dsu[u];
        dsu[u] = v;
    }
    bool same(int u, int v) {
        return root(u) == root(v);
    }

    // lca
    // btn[i][u] = u 前往它 2^i parent 的路上經過的最大權重
    // par[i][u] = u 的 2^i parent 是誰
    int dep[MAX_N];
    int btn[MAX_LOG_N][MAX_N];
    int par[MAX_LOG_N][MAX_N];

    // mst
    struct Edge { int to, w; };
    vector<Edge> g[MAX_N];

    void dfs(int u, int p, int d) {
        dep[u] = d;
        par[0][u] = p;
        for (Edge e : g[u]) {
            if (e.to != p) {
                btn[0][e.to] = e.w;
                dfs(e.to, u, d + 1);
            }
        }
    }

    void build() {
        for (int u = 0; u < N; u++) {
            if (dep[u] == -1) {
                dfs(u, -1, 0);
            }
        }

        for (int i = 0; i + 1 < MAX_LOG_N; i++) {
            for (int u = 0; u < N; u++) {
                if (par[i][u] == -1) {
                    par[i + 1][u] = -1;
                    btn[i + 1][u] = 0;
                }
                else {
                    par[i + 1][u] = par[i][par[i][u]];
                    btn[i + 1][u] = max(btn[i][u], btn[i][par[i][u]]);
                }
            }
        }
    }

    int lca(int u, int v) { // 回傳 u, v 之間的最大權重
        int mx = -1; // u, v 之間的最大權重

        if (dep[u] > dep[v]) swap(u, v);
        int diff = dep[v] - dep[u];
        for (int i = MAX_LOG_N - 1; i >= 0; i--) {
            if (diff & (1 << i)) {
                mx = max(mx, btn[i][v]);
                v = par[i][v];
            }
        }

        if (u == v) return mx;

        for (int i = MAX_LOG_N - 1; i >= 0; i--) {
            if (par[i][u] != par[i][v]) {
                mx = max(mx, btn[i][u]);
                mx = max(mx, btn[i][v]);
                u = par[i][u];
                v = par[i][v];
            }
        }
        // lca = par[0][u] = par[0][v];
        mx = max(mx, max(btn[0][u], btn[0][v]));

        return mx;
    }

    struct Oper {
        int id, cmd, u, v;
    };
    vector<Oper> operations;

    int main() {
        int TC;
        scanf("%d", &TC);
        while (TC--) {
            scanf("%d %d", &N, &Q);

            // init
            fill(dep, dep + N, -1);
            for (int i = 0; i < MAX_LOG_N; i++) {
                fill(btn[i], btn[i] + N, -1);
                fill(par[i], par[i] + N, -1);
            }
            for (int i = 0; i < N; i++)
                g[i].clear();
            operations.clear();

            for (int id = 1; id <= Q; id++) {
                int cmd, u, v;
                scanf("%d %d %d", &cmd, &u, &v);
                u--; v--;
                operations.push_back((Oper) {id, cmd, u, v});
            }

            // construct mst(s)
            fill(dsu, dsu + N, -1);
            for (Oper op : operations) {
                int u = op.u, v = op.v;
                if (op.cmd == 1) {
                    if (!same(u, v)) {
                        unite(u, v);
                        g[u].push_back((Edge) {v, op.id});
                        g[v].push_back((Edge) {u, op.id});
                    }
                }
            }

            build(); // build lca, btn on mst(s)

            // 重新跑一次操作, dsu 維護連通性，用 lca 計算答案
            fill(dsu, dsu + N, -1);
            for (Oper op : operations) {
                int u = op.u, v = op.v;
                if (op.cmd == 1) {
                    if (!same(u, v)) {
                        unite(u, v);
                    }
                }
                else {
                    if (!same(u, v)) puts("-1");
                    else {
                        printf("%d\n", lca(u, v));
                    }
                }
            }
        }

        return 0;
    }
