###################################################
LCA
###################################################

.. sidebar:: Tags

    - ``tag_lca``
    - ``tag_template``

.. contents:: TOC
    :depth: 2

************************
程式碼
************************

.. code-block:: cpp
    :linenos:

    const int MAX_V = ...;
    const int MAX_LOG_V = ...; // ceil(MAX_V)
    int V, root;
    vector<int> g[MAX_V];

    int par[MAX_LOG_V][MAX_V];
    int dep[MAX_V];

    void dfs(int v, int p, int d) {
        par[0][v] = p;
        dep[v] = d;
        for (size_t i = 0; i < g[v].size(); i++)
            if (g[v][i] != p)
                dfs(g[v][i], v, d + 1);
    }

    void init() {
        memset(par, -1, sizeof(par));

        dfs(root, -1, 0);
        for (int k = 0; k + 1 < MAX_LOG_V; k++) {
            for (int v = 0; v < V; v++) {
                if (par[k][v] < 0) par[k + 1][v] = -1;
                else par[k + 1][v] = par[k][par[k][v]];
            }
        }
    }

    int lca(int u, int v) {
        if (dep[u] > dep[v]) swap(u, v);
        for (int k = 0; k < MAX_LOG_V; k++) {
            if ((dep[v] - dep[u]) >> k & 1) {
                v = par[k][v];
            }
        }
        if (u == v) return u;
        for (int k = MAX_LOG_V - 1; k >= 0; k--) {
            if (par[k][u] != par[k][v]) {
                u = par[k][u];
                v = par[k][v];
            }
        }
        return par[0][u];
    }

************************
模板驗證
************************

待測
