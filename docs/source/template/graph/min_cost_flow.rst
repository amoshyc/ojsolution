###################################################
Min Cost Flow
###################################################

.. sidebar:: Tags

    - ``tag_min_cost_flow``
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

    typedef pair<int, int> pii;
    const int INF = 0x3f3f3f3f;

    struct Edge {
        int to, cap, cost, rev;
    };

    const int MAX_V = 2 * 100 + 10;
    int V;
    vector<Edge> g[MAX_V];
    int h[MAX_V];
    int d[MAX_V];
    int prevv[MAX_V];
    int preve[MAX_V];

    void add_edge(int u, int v, int cap, int cost) {
        g[u].push_back((Edge){v, cap, cost, (int)g[v].size()});
        g[v].push_back((Edge){u, 0, -cost, (int)g[u].size() - 1});
    }

    int min_cost_flow(int s, int t, int f) {
        int res = 0;
        fill(h, h + V, 0);
        while (f > 0) {
            fill(d, d + V, INF);
            priority_queue< pii, vector<pii>, greater<pii> > pq;

            d[s] = 0;
            pq.push(pii(d[s], s));

            while (!pq.empty()) {
                pii p = pq.top(); pq.pop();
                int v = p.nd;
                if (d[v] < p.st) continue;
                for (size_t i = 0; i < g[v].size(); i++) {
                    const Edge& e = g[v][i];
                    if (e.cap > 0 && d[e.to] > d[v] + e.cost + h[v] - h[e.to]) {
                        d[e.to] = d[v] + e.cost + h[v] - h[e.to];
                        prevv[e.to] = v;
                        preve[e.to] = i;
                        pq.push(pii(d[e.to], e.to));
                    }
                }
            }

            if (d[t] == INF) return -1;

            for (int v = 0; v < V; v++)
                h[v] += d[v];

            int bn = f;
            for (int v = t; v != s; v = prevv[v])
                bn = min(bn, g[prevv[v]][preve[v]].cap);

            f -= bn;
            res += bn * h[t];
            for (int v = t; v != s; v = prevv[v]) {
                Edge& e = g[prevv[v]][preve[v]];
                e.cap -= bn;
                g[v][e.rev].cap += bn;
            }
        }
        return res;
    }

************************
模板驗證
************************

`poj2195 <http://codepad.org/IVCbipKk>`_
