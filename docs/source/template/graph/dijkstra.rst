###################################################
Dijkstra's Algorithm
###################################################

.. sidebar:: Tags

    - ``tag_dijkstra``
    - ``tag_template``

.. contents:: TOC
    :depth: 2

************************
程式碼
************************

.. code-block:: cpp
    :linenos:

    #define st first
    #define nd second

    typedef pair<int, int> pii; // <d, v>
    struct Edge {
        int to, w;
    };

    const int MAX_V = ...;
    const int INF = 0x3f3f3f3f;

    int V, S; // V, Source
    vector<Edge> g[MAX_V];
    int d[MAX_V];

    int dijkstra() {
        fill(d, d + V, INF);
        priority_queue< pii, vector<pii>, greater<pii> > pq;

        d[S] = 0;
        pq.push(pii(0, S));

        while (!pq.empty()) {
            pii top = pq.top(); pq.pop();
            int u = top.nd;

            if (d[u] < top.st) continue;

            // for (const Edge& e : g[u]) {
            for (size_t i = 0; i < g[u].size(); i++) {
                const Edge& e = g[u][i];
                if (d[e.to] > d[u] + e.w) {
                    d[e.to] = d[u] + e.w;
                    pq.push(pii(d[e.to], e.to));
                }
            }
        }

        return ...;
    }

************************
模板驗證
************************

`uva10986 <http://codepad.org/nEGXuSYA>`_
