###################################################
[Template] Disjoint Set 並查集
###################################################

.. sidebar:: Tags

    - ``tag_dsu``

.. contents:: TOC
    :depth: 2

************************
程式碼
************************

.. code-block:: cpp
    :linenos:

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

************************
模板驗證
************************

`poj1182 <http://codepad.org/cgXatNnD>`_
