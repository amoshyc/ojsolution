###################################################
Minimum Bottleneck
###################################################

.. sidebar:: Tags

    - ``tag_lca``
    - ``tag_mst``
    - ``tag_offline``

.. contents:: TOC
    :depth: 2

************************
程式碼
************************

給定一個帶權無向圖 G = (N, E)，Q 筆詢問，每筆 (s, t) 考慮 s, t 之間所有路徑，每條路徑有其經過的最大邊權，即為這條路徑的瓶頸值，請問這些路徑中，最小的瓶頸值是多少？

-----------------

可證明任兩點之間的最小瓶頸值必在整個圖的最小生成樹上（想想看 kruskal 是怎麼建 mst 的）。
然後利用離線 lca 倍增法的手法求樹上任兩點的最大瓶頸值。

.. code-block:: cpp
    :linenos:

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
                if (par[i][u] == -1 || par[i][par[i][u]] == -1) {
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

************************
模板驗證
************************

`uva11354 <../../uva/p11354.html>`_
