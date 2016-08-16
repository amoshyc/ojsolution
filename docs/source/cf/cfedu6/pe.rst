########################################
[cfedu6] E. New Year Tree
########################################

.. sidebar:: Tags

    - ``tag_dfs``
    - ``tag_segment_tree``
    - ``tag_bitmask``

.. contents:: TOC
    :depth: 2

******************************************************
`題目 <http://codeforces.com/contest/620/problem/E>`_
******************************************************

給定一個大小為 N 的樹 T，根的邊號為 1，每個節點上有其顏色。
請寫一個程式支援 M 個操作。操作的種類為

    1. ``1 v c`` 將以 v 為根的子樹的每個節點的顏色都改為顏色 c
    2. ``2 v`` 輸出以 v 為根的子樹中共有幾種不同的顏色

顏色最多只有 60 種。

************************
Specification
************************

::

    1 <= n <= 500000


************************
分析
************************

.. note:: dfs 轉線段樹

首先我們利用 dfs 將 T 轉為線段樹，即將每個節點用 dfs 迭代的順序重新編號。
這樣做有一個好處::

    對某個子樹進行的操作 變成 對某個區間進行操作。

.. image:: http://i.imgur.com/0biWz91.png

具體來說，dfs 的同時，記錄每個節點 v，以 v 為根的子樹對應到哪個區間，
將之儲存在 idxL[v], idxR[v] 這兩個陣列裡。這是一個半開區間。

這題的操作就變成

    1. ``1 v c`` 將區間 [idxL[v], idxR[v]) 都改為 c
    2. ``2 v`` 輸出區間 [idxL[v], idxR[v]) 中共有幾個不同的數字

這樣這題就變成線段樹的區間查詢。
可以利用 bitwise-OR 並把顏色壓進位元裡來實作。

另外，因為有區間更改，所以線段樹需要延遲標記。

************************
AC Code
************************

.. code-block:: cpp
    :linenos:

    #include <bits/stdc++.h>
    using namespace std;

    typedef long long ll;

    const int MAX_N = 400000;
    const int MAX_NN = (1 << 19); // should be bigger than MAX_N

    int N, M;
    int A[MAX_N];
    vector<int> g[MAX_N];
    // [L, R)
    int idxL[MAX_N];
    int idxR[MAX_N];

    int dfn = 0;
    void dfs(int u, int p) {
        idxL[u] = dfn++;
        for (int v : g[u]) {
            if (v == p) continue;
            else dfs(v, u);
        }
        idxR[u] = dfn;
    }

    int NN;
    ll seg[2 * MAX_NN - 1];
    ll lazy[2 * MAX_NN - 1];
    // lazy[u] != 0 : the subtree of u (u not included) is not up-to-date

    inline int lc(int u) { return 2 * u + 1; }
    inline int rc(int u) { return 2 * u + 2; }

    void seg_gather(int u) {
        seg[u] = seg[lc(u)] | seg[rc(u)];
    }

    void seg_push(int u) {
        if (lazy[u] != 0) {
            seg[lc(u)] = lazy[u];
            seg[rc(u)] = lazy[u];

            lazy[lc(u)] = lazy[u];
            lazy[rc(u)] = lazy[u];
            lazy[u] = 0;
        }
    }

    void seg_init() {
        NN = 1;
        while (NN < N)
            NN <<= 1;
        fill(seg, seg + NN, 0);
        fill(lazy, lazy + NN, 0);
        for (int i = 0; i < N; i++) {
            seg[NN - 1 + idxL[i]] = (1ll << A[i]);
        }
    }

    void seg_build(int u) {
        if (u >= NN - 1) { // leaf
            return;
        }
        seg_build(lc(u));
        seg_build(rc(u));
        seg_gather(u);
    }

    void seg_update(int a, int b, int delta, int u, int l, int r) {
        if (l >= b || r <= a) {
            return;
        }

        if (a <= l && r <= b) {
            seg[u] = (1ll << delta);
            lazy[u] = (1ll << delta);
            return;
        }

        int m = (l + r) / 2;
        seg_push(u);
        seg_update(a, b, delta, lc(u), l, m);
        seg_update(a, b, delta, rc(u), m, r);
        seg_gather(u);
    }

    ll seg_query(int a, int b, int u, int l, int r) {
        if (l >= b || r <= a) {
            return 0;
        }

        if (a <= l && r <= b) {
            return seg[u];
        }

        int m = (l + r) / 2;
        seg_push(u);
        ll ans = 0;
        ans |= seg_query(a, b, lc(u), l, m);
        ans |= seg_query(a, b, rc(u), m, r);
        seg_gather(u);

        return ans;
    }

    int main() {
        scanf("%d %d", &N, &M);
        for (int i = 0; i < N; i++)
            scanf("%d", &A[i]);
        for (int i = 0; i < N - 1; i++) {
            int u, v; scanf("%d %d", &u, &v); u--; v--;
            g[u].push_back(v);
            g[v].push_back(u);
        }

        dfs(0, -1);

        seg_init();
        seg_build(0);

        while (M--) {
            int cmd; scanf("%d", &cmd);

            if (cmd == 1) {
                int v, c; scanf("%d %d", &v, &c); v--;
                seg_update(idxL[v], idxR[v], c, 0, 0, NN);
            }
            else {
                int v; scanf("%d", &v); v--;
                ll res = seg_query(idxL[v], idxR[v], 0, 0, NN);
                printf("%d\n", __builtin_popcountll(res));
            }
        }

        return 0;
    }
