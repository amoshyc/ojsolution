###################################################
Kruskal's Algorithm
###################################################

.. sidebar:: Tags

    - ``tag_kruskal``
    - ``tag_template``

.. contents:: TOC
    :depth: 2

************************
程式碼
************************

.. code-block:: cpp
    :linenos:

    struct Edge {
        int u, v, cost;
        bool operator < (const Edge& e) const {
            return cost < e.cost;
        }
    };

    int E;
    Edge edges[MAX_E];

    void kruskal() {
        sort(edges, edges + E);
        init(); // dsu init
        for (int i = 0; i < E; i++) {
            Edge& e = edges[i];
            if (!same(e.u, e.v)) {
                unite(e.u, e.v);
            }
        }
    }

************************
模板驗證
************************

`poj1258 <https://ideone.com/uaffK3>`_
