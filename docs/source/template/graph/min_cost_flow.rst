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

    typedef pair<double, int> pii;
    const double INF = 1e10;

    struct Edge {
        int to, cap;
        double cost;
        int rev;
    };

    const int MAX_V = 2 * 100 + 10;
    int V;
    vector<Edge> g[MAX_V];
    double h[MAX_V];
    double d[MAX_V];
    int prevv[MAX_V];
    int preve[MAX_V];
    // int match[MAX_V];

    void add_edge(int u, int v, int cap, double cost) {
        g[u].push_back((Edge){v, cap, cost, (int)g[v].size()});
        g[v].push_back((Edge){u, 0, -cost, (int)g[u].size() - 1});
    }

    double min_cost_flow(int s, int t, int f) {
        double res = 0;
        fill(h, h + V, 0);
        fill(match, match + V, -1);
        while (f > 0) {
            // dijkstra 找最小成本增廣路徑
            // without h will reduce to SPFA = O(V*E)
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

            // 找不到增廣路徑
            if (d[t] == INF) return -1;

            // 維護 h[v]
            for (int v = 0; v < V; v++)
                h[v] += d[v];

            // 找瓶頸
            int bn = f;
            for (int v = t; v != s; v = prevv[v])
                bn = min(bn, g[prevv[v]][preve[v]].cap);

            // // find match
            // for (int v = prevv[t]; v != s; v = prevv[prevv[v]]) {
            //     int u = prevv[v];
            //     match[v] = u;
            //     match[u] = v;
            // }

            // 更新剩餘圖
            f -= bn;
            res += bn * h[t]; // SPFA: res += bn * d[t]
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

 - [最小費用流] `poj2195 <http://codepad.org/kGDEcbBW>`_
 - [最大費用流] `poj3422 <http://codepad.org/1NjG4RQS>`_
 - [配對] `poj 3565 <http://codepad.org/OpFifd7f>`_
