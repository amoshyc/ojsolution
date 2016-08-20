#####################################
[cf368] B. Bakery
#####################################

.. sidebar:: Tags

    - ``tag_enum``

.. contents:: TOC
    :depth: 2


******************************************************
`題目 <http://codeforces.com/contest/707/problem/B>`_
******************************************************

給定一個無向圖 G = (V, E)，其中 K 個節點為儲存點。
欲在儲存點中選一個節點，在非儲存點中也選一個節點，使用兩節點間的距離最小化。
請問該距離為多少？

************************
Specification
************************

::

    1 <= |V|, |E| <= 10^5
    0 <= K <= |V|

************************
分析
************************

.. note:: 枚舉

這題事實上就建圖之後，枚舉各個儲存點的相鄰點，找最近的那個。

比賽時，我想到的是另一個方法。補一個超級源點連到各個儲存點，權重為 0。
之後跑 dijkstra，最後輸出距離超級源點最近的非儲存點的距離。

************************
AC Code 1
************************

.. code-block:: cpp
    :linenos:

    #include <bits/stdc++.h>
    using namespace std;

    const int INF = 0x3f3f3f3f;

    struct Edge {
        int to, w;
    };

    bool isStorage[100000];
    vector<Edge> g[100000];

    int main() {
        int N, M, K;
        scanf("%d %d %d", &N, &M, &K);

        for (int i = 0; i < M; i++) {
            int u, v, w;
            scanf("%d %d %d", &u, &v, &w);
            u--; v--;

            g[u].push_back((Edge) {v, w});
            g[v].push_back((Edge) {u, w});
        }

        for (int i = 0; i < K; i++) {
            int v; scanf("%d", &v); v--;
            isStorage[v] = true;
        }

        int ans = INF;
        for (int v = 0; v < N; v++) {
            if (isStorage[v]) {
                for (auto e : g[v]) {
                    if (!isStorage[e.to]) {
                        ans = min(ans, e.w);
                    }
                }
            }
        }

        if (ans == INF) puts("-1");
        else {
            printf("%d\n", ans);
        }

        return 0;
    }


************************
AC Code 1
************************

.. code-block:: cpp
    :linenos:

    #include <bits/stdc++.h>
    using namespace std;

    #define st first
    #define nd second

    typedef long long ll;
    typedef pair<ll, int> pii; // <d, v>
    struct Edge {
        int to; ll w;
    };

    const int MAX_V = 100000 + 10;
    const ll INF = 1e18;

    int V, S; // V, Source
    vector<Edge> g[MAX_V];
    ll d[MAX_V];

    void dijkstra() {
        fill(d, d + V, INF);
        priority_queue< pii, vector<pii>, greater<pii> > pq;

        d[S] = 0;
        pq.push(pii(0ll, S));

        while (!pq.empty()) {
            pii top = pq.top(); pq.pop();
            int u = top.nd;

            if (d[u] < top.st) continue;

            for (const Edge& e : g[u]) {
                if (d[e.to] > d[u] + e.w) {
                    d[e.to] = d[u] + e.w;
                    pq.push(pii(d[e.to], e.to));
                }
            }
        }
    }

    int main() {
        ios::sync_with_stdio(false);
        cin.tie(0);

        int N, M, K;
        scanf("%d %d %d", &N, &M, &K);
        for (int i = 0; i < M; i++) {
            int u, v, w; scanf("%d %d %d", &u, &v, &w);
            u--; v--;

            g[u].push_back((Edge) {v, w});
            g[v].push_back((Edge) {u, w});
        }

        S = N; // super source
        for (int i = 0; i < K; i++) {
            int c; scanf("%d", &c); c--;
            g[S].push_back((Edge) {c, 0});
        }

        V = N + 1;
        dijkstra();

        ll ans = INF;
        for (int i = 0; i < N; i++) {
            if (d[i] == 0) continue;
            ans = min(ans, d[i]);
        }

        if (ans == INF) puts("-1");
        else {
            printf("%lld\n", ans);
        }

        return 0;
    }
