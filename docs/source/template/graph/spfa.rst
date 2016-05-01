###################################################
SPFA
###################################################

.. sidebar:: Tags

    - ``tag_spfa``
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
    int cnt[MAX_V];

    bool spfa() { // 回傳有無負環
        fill(d, d + V, INF);
        fill(cnt, cnt + V, 0);
        priority_queue< pii, vector<pii>, greater<pii> > pq;

        d[S] = 0;
        pq.push(pii(0, S));
        cnt[S] = 1;

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

                    cnt[e.to]++;
                    if (cnt[e.to] >= V)
                        return true;
                }
            }
        }

        return false;
    }

************************
模板驗證
************************

`poj3259 <http://codepad.org/aeURgGhp>`_
