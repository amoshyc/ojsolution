###################################################
LCA
###################################################

.. sidebar:: Tags

    - ``tag_lca``
    - ``tag_template``

.. contents:: TOC
    :depth: 2

************************
程式碼
************************

.. code-block:: cpp
    :linenos:

    const int MAX_N = 10000;
    const int MAX_LOG_N = 14; // (1 << MAX_LOG_N) > MAX_N

    int N, root;
    int dep[MAX_N];
    int par[MAX_LOG_N][MAX_N];

    vector<int> child[MAX_N];

    void dfs(int u, int p, int d) {
        dep[u] = d;
        // par[0][u] = p;
        for (int i = 0; i < int(child[u].size()); i++) {
            int v = child[u][i];
            if (v != p) {
                dfs(v, u, d + 1);
            }
        }
    }

    void build() {
        // par[0][u] and dep[u]
        dfs(root, -1, 0);

        // par[i][u]
        for (int i = 0; i + 1 < MAX_LOG_N; i++) {
            for (int u = 0; u < N; u++) {
                if (par[i][u] == -1)
                    par[i + 1][u] = -1;
                else
                    par[i + 1][u] = par[i][par[i][u]];
            }
        }
    }

    int lca(int u, int v) {
        if (dep[u] > dep[v]) swap(u, v); // 讓 v 較深
        int diff = dep[v] - dep[u]; // 將 v 上移到與 u 同層
        for (int i = 0; i < MAX_LOG_N; i++) {
            if (diff & (1 << i)) {
                v = par[i][v];
            }
        }
        if (u == v) return u;
        for (int i = MAX_LOG_N - 1; i >= 0; i--) { // 必需倒序
            if (par[i][u] != par[i][v]) {
                u = par[i][u];
                v = par[i][v];
            }
        }
        return par[0][u];
    }

************************
原理
************************

.. image:: http://i.imgur.com/TCy0aVS.png

.. math::

    \text{Let } parent_{i, v} &= \text{v 的第 i 個 parent} \\
    parent_{p, u}   = parent_{p, v} &= lca               &\qquad (1) \\
    \forall x > p,\, parent_{x, u}  &=    parent_{x, v}  &\qquad (2) \\
    \forall x < p,\, parent_{x, u}  &\neq parent_{x, v}  &\qquad (3) \\

當我們建好倍增數組:

.. math::

    par_{i, v} = \text{v 的第 $2^i$ 個 parent}

使用這段程式碼來求出 :math:`lca`

.. code-block:: cpp
    :linenos:

    int lca(int u, int v) {
        if (dep[u] > dep[v]) swap(u, v); // 讓 v 較深
        int diff = dep[v] - dep[u]; // 將 v 上移到與 u 同層
        for (int i = 0; i < MAX_LOG_N; i++) {
            if (diff & (1 << i)) {
                v = par[i][v];
            }
        }

        if (u == v) return u;

        for (int i = MAX_LOG_N - 1; i >= 0; i--) { // 必需倒序
            if (par[i][u] != par[i][v]) {
                u = par[i][u];
                v = par[i][v];
            }
        }
        return par[0][u];
    }

第 2 ~ 8 行不用說，讓 :math:`u, v` 同層。

第 10 行，特判 :math:`lca` 為 :math:`u, v` 其中一個點的情況。

第 12 ~ 18 行程式碼利用改寫後的式子 :math:`(3)` 來求 :math:`lca`

.. math::

    \forall\, 2^i < p,\, par_{i, u} \neq par_{i, v}

這相當於對 :math:`p-1` 二進位分解，但實際上我們不知道 :math:`p-1` 是多少，
所以使用 :math:`par_{i, u} \neq par_{i, v}` 來判定要不要再往上走。（類似於二分搜的 checker function）
但注意 i 必需由大到小。往上走的幅度會越來越小，最終 :math:`u, v` 都停在 :math:`lca` 的下方。

------------------

這個手法如同於要求

==== == == == == == == ==
idx   0  1  2  3  4  5  6
==== == == == == == == ==
f(x)  0  0  0  1  1  1  1
==== == == == == == == ==

or

==== == == == == == == ==
idx   0  1  2  3  4  5  6
==== == == == == == == ==
f(x)  0  0  0  0  0  1  1
==== == == == == == == ==

的 0, 1 分界點可以這樣寫:

.. code-block:: cpp
    :linenos:

    int N = 7, pivot = 1;
    for (int i = 4; i >= 0; i /= 2) {
        if (f(pivot + (i - 1)) == 0) {
            pivot = pivot + (i - 1);
        }
    }
    pivot++; // 讓 pivot 停在 1 上

************************
模板驗證
************************

`poj1330 <../../poj/p1330.html>`_
