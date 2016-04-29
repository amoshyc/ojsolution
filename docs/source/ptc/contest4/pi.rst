###################################################
I. Minimum Risk Shortest Path
###################################################

.. sidebar:: Tags

    - ``tag_binary_search``
    - ``tag_dfs``
    - ``tag_dijkstra``

.. contents:: TOC
    :depth: 2


*******************************************************************************
`題目 <http://e-tutor.itsa.org.tw/e-Tutor/mod/programming/view.php?id=23655>`_
*******************************************************************************

待補

************************
Specification
************************

::

    1 <= V <= 500
    1 <= E <= 50000
    1 <= risk[i] <= 100
    1 <= w[i] <= 10000

************************
分析
************************

.. note:: 二分搜 + dfs + dijkstra

待補

************************
AC Code
************************

.. code-block:: cpp
    :linenos:

    #include <iostream>
    #include <vector>
    #include <algorithm>
    #include <cstring>
    #include <stack>
    #include <queue>
    using namespace std;

    #define st first
    #define nd second

    typedef pair<int, int> pii; // <d, v>

    struct Edge {
        int to, w, r;
        Edge(){}
        Edge(int a, int b, int c) {
            to = a;
            w = b;
            r = c;
        }
        bool operator < (const Edge& e) const {
            return w < e.w;
        }
    };

    const int max_v = 500;
    const int INF = 0x3f3f3f3f;
    int V, E;
    vector<Edge> g[max_v];

    bool added[max_v];
    int d[max_v];

    bool C(int r) {
        // dfs
        memset(added, false, sizeof(added));
        stack<int> st;

        st.push(0);
        added[0] = true;

        while (!st.empty()) {
            int u = st.top(); st.pop();

            if (u == V - 1) return true;

            for (size_t i = 0; i < g[u].size(); i++) {
                const Edge& e = g[u][i];
                if (e.r <= r && !added[e.to]) {
                    st.push(e.to);
                    added[e.to] = true;
                }
            }
        }

        return false;
    }

    int dijkstra(int r) {
        fill(d, d + V, INF);
        priority_queue< pii, vector<pii>, greater<pii> > pq;

        d[0] = 0;
        pq.push(pii(0, 0));

        while (!pq.empty()) {
            pii top = pq.top(); pq.pop();
            int v = top.nd;

            if (d[v] < top.st) continue; // 過期資訊

            for (size_t i = 0; i < g[v].size(); i++) {
                const Edge& e = g[v][i];
                if (e.r <= r && d[e.to] > d[v] + e.w) {
                    d[e.to] = d[v] + e.w;
                    pq.push(pii(d[e.to], e.to));
                }
            }
        }

        return d[V - 1]; // may be INF
    }


    int solve(int max_r) {
        int lb = 0, ub = max_r;
        // 0 0 0 0 1 1 1 1
        if (C(lb) == true) return 0;
        if (C(ub) == false) return -1;

        while (ub - lb > 1) {
            int mid = (lb + ub) / 2;
            if (C(mid)) ub = mid;
            else lb = mid;
        }

        cout << ub << " ";

        dijkstra(ub);
        return d[V - 1];
    }

    int main() {
        ios::sync_with_stdio(false);
        cin.tie(0);

        int TC; cin >> TC;
        while (TC--) {
            for (int i = 0; i < max_v; i++)
                g[i].clear();

            cin >> V >> E;

            int max_r = -1;
            for (int i = 0; i < E; i++) {
                int u, v, w, r;
                cin >> u >> v >> r >> w;
                g[u].push_back(Edge(v, w, r));
                max_r = max(max_r, r);
            }

            cout << solve(max_r) << "\n";
        }
        return 0;
    }
