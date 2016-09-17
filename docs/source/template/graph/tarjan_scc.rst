###################################################
Tarjan SCC Algorithm
###################################################

.. sidebar:: Tags

    - ``tag_scc``
    - ``tag_template``

.. contents:: TOC
    :depth: 2

************************
程式碼
************************

.. code-block:: cpp
    :linenos:

    const int MAX_V = ...;
    const int INF = 0x3f3f3f3f;
    int V;
    vector<int> g[MAX_V];

    int dfn_idx = 0;
    int scc_cnt = 0;
    int dfn[MAX_V];
    int low[MAX_V];
    int belong[MAX_V];
    bool in_st[MAX_V];
    vector<int> st;

    void scc(int v) {
        dfn[v] = low[v] = dfn_idx++;
        st.push_back(v);
        in_st[v] = true;

        for (int i = 0; i < int(g[v].size()); i++) {
            const int u = g[v][i];
            if (dfn[u] == -1) {
                scc(u);
                low[v] = min(low[v], low[u]);
            }
            else if (in_st[u]) {
                low[v] = min(low[v], dfn[u]);
            }
        }

        if (dfn[v] == low[v]) {
            int k;
            do {
                k = st.back(); st.pop_back();
                in_st[k] = false;
                belong[k] = scc_cnt;
            } while (k != v);
            scc_cnt++;
        }
    }

    void tarjan() { // scc 建立的順序即為反向的拓璞排序
        st.clear();
        fill(dfn, dfn + V, -1);
        fill(low, low + V, INF);
        dfn_idx = 0;
        scc_cnt = 0;
        for (int v = 0; v < V; v++) {
            if (dfn[v] == -1) {
                scc(v);
            }
        }
    }

************************
模板驗證
************************

`poj2186 <../../poj/p2186.html>`_
