###################################################
Bipartite Matching
###################################################

.. sidebar:: Tags

    - ``tag_bipartite_matching``
    - ``tag_template``

.. contents:: TOC
    :depth: 2

************************
程式碼
************************

.. code-block:: cpp
    :linenos:

    const int MAX_V = ...;
    int V;
    vector<int> g[MAX_V];
    int match[MAX_V];
    bool used[MAX_V];

    void add_edge(int u, int v) {
        g[u].push_back(v);
        g[v].push_back(u);
    }

    // 回傳有無找到從 v 出發的增廣路徑
    //（首尾都為未匹配點的交錯路徑）
    // [待確認] 每次遞迴都找一個末匹配點 v 及匹配點 u
    bool dfs(int v) {
        used[v] = true;
        for (size_t i = 0; i < g[v].size(); i++) {
            int u = g[v][i], w = match[u];
            // 尚未配對或可從 w 找到增廣路徑（即路徑繼續增長）
            if (w < 0 || (!used[w] && dfs(w))) {
                // 交錯配對
                match[v] = u;
                match[u] = v;
                return true;
            }
        }
        return false;
    }

    int bipartite_matching() { // 匈牙利演算法
        int res = 0;
        memset(match, -1, sizeof(match));
        for (int v = 0; v < V; v++) {
            if (match[v] == -1) {
                memset(used, false, sizeof(used));
                if (dfs(v)) {
                    res++;
                }
            }
        }
        return res;
    }

************************
模板驗證
************************

`poj3041 <http://codepad.org/i5khF6Sf>`_
