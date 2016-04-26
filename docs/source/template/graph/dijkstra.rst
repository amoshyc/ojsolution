###################################################
[Template] Dijkstra's Algorithm
###################################################

.. sidebar:: Tags

    - ``tag_dijkstra``

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

    const int MAX_N = 20000;
    const int INF = 0x3f3f3f3f;
    int N, M, S, T; // V, E, Source, Target
    vector<Edge> g[MAX_N];
    int d[MAX_N];

    int dijkstra() {
        fill(d, d + N, INF);
        priority_queue<pii, vector<pii>, greater<pii>> pq;

        d[S] = 0;
        pq.push(pii(0, S));

        while (!pq.empty()) {
            pii top = pq.top(); pq.pop();
            int v = top.nd;

            if (d[v] < top.st) continue; // 過期資訊

            for (const Edge& e : g[v]) {
                if (d[e.to] > d[v] + e.w) {
                    d[e.to] = d[v] + e.w;
                    pq.push(pii(d[e.to], e.to));
                }
            }
        }

        return d[T]; // may be INF
    }

************************
模板驗證
************************

`uva10986 <http://codepad.org/YeHVSObJ>`_
