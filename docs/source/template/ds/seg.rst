###################################################
Segment Tree
###################################################

.. sidebar:: Tags

    - ``tag_segment_tree``
    - ``tag_template``

.. contents:: TOC
    :depth: 2

************************
程式碼
************************

.. code-block:: cpp
    :linenos:

    int nn;
    ll seg[2 * max_nn - 1];
    ll lazy[2 * max_nn - 1];
    // lazy[u] != 0 : the subtree of u (u not included) is not up-to-date

    void seg_gather(int u) {
        seg[u] = seg[u * 2 + 1] + seg[u * 2 + 2]; // sum
    }

    void seg_push(int u, int l, int m, int r) {
        if (lazy[u] != 0) {
            seg[u * 2 + 1] += (m - l) * lazy[u];
            seg[u * 2 + 2] += (r - m) * lazy[u];

            lazy[u * 2 + 1] += lazy[u];
            lazy[u * 2 + 2] += lazy[u];
            lazy[u] = 0;
        }
    }

    void seg_init() {
        nn = 1;
        while (nn < n)
            nn *= 2;

        memset(seg, 0, sizeof(seg));
        memset(lazy, 0, sizeof(lazy));
        // memcpy(seg + nn - 1, inp, sizeof(ll) * n);
    }

    void seg_build(int u, int l, int r) {
        if (u >= nn - 1) { // leaf
            seg[u] = inp[u - nn + 1];
            return;
        }

        seg_build(u * 2 + 1, l, (l + r) / 2);
        seg_build(u * 2 + 2, (l + r) / 2, r);
        seg_gather(u);
    }

    void seg_update(int a, int b, int delta, int u, int l, int r) {
        if (l >= b || r <= a) {
            return;
        }

        if (a <= l && r <= b) {
            seg[u] += (r - l) * delta;
            lazy[u] += delta;
            return;
        }

        int m = (l + r) / 2;
        seg_push(u, l, m, r);
        seg_update(a, b, delta, u * 2 + 1, l, m);
        seg_update(a, b, delta, u * 2 + 2, m, r);
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
        seg_push(u, l, m, r);
        ll ans = 0;
        ans += seg_query(a, b, u * 2 + 1, l, m);
        ans += seg_query(a, b, u * 2 + 2, m, r);
        seg_gather(u);

        return ans;
    }

************************
模板驗證
************************

`poj3468 <https://ideone.com/PBzsZ8>`_
