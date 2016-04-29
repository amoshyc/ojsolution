###################################################
F. The Maximum Bottleneck Spanning Tree
###################################################

.. sidebar:: Tags

    - ``tag_kruskal``

.. contents:: TOC
    :depth: 2


*******************************************************************************
`題目 <http://e-tutor.itsa.org.tw/e-Tutor/mod/programming/view.php?id=23652>`_
*******************************************************************************

待補

************************
Specification
************************

::

    1 <= N <= 1000
    N <= M <= N * (N - 1) / 2

************************
分析
************************

.. note:: Kruskal 變形

待補

************************
AC Code
************************

.. code-block:: cpp
    :linenos:

    #include <iostream>
    #include <algorithm>
    #include <vector>
    #include <cstring>
    using namespace std;

    typedef long long ll;

    struct Edge {
        int u, v, cost;
        Edge(){}
        Edge(int a, int b, int c) {
            u = a;
            v = b;
            cost = c;
        }
        bool operator > (const Edge& e) const {
            return cost > e.cost;
        }
    };

    const int MAX_N = 1000;
    const int MAX_E = 1000 * (1000 - 1) / 2;

    int V, E;
    Edge edges[MAX_E];
    int par[MAX_N];

    void init() {
        memset(par, -1, sizeof(par)); // par[i] = -rank[i] if i is root
    }

    int root(int a) {
        if (par[a] < 0) return a;
        return (par[a] = root(par[a]));
    }

    void unite(int a, int b) {
        a = root(a);
        b = root(b);
        if (a == b) return; // already in same set
        if (-par[a] > -par[b]) swap(a, b); // if (rank[a] > rank[b])
        par[b] += par[a]; // height[b] += height[a]
        par[a] = b;
    }

    bool same(int a, int b) {
        return root(a) == root(b);
    }

    ll solve() {
        sort(edges, edges + E, greater<Edge>());

        ll ans = 0;
        for (int i = 0; i < E; i++) {
            const Edge& e = edges[i];
            if (!same(e.u, e.v)) {
                unite(e.u, e.v);
                ans += e.cost;
            }
        }

        return ans;
    }

    int main() {
        ios::sync_with_stdio(false);
        cin.tie(0);

        int TC; cin >> TC;
        while (TC--) {
            cin >> V >> E;

            init();

            for (int i = 0; i < E; i++) {
                int u, v, c;
                cin >> u >> v >> c;
                edges[i] = Edge(u, v, c);
            }

            cout << solve() << "\n";
        }

        return 0;
    }
