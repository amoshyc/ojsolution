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

    const int MAX_N = 100000;
    const int MAX_NN = (1 << 20); // should be bigger than MAX_N

    int N;
    ll inp[MAX_N];

    int NN;
    ll seg[2 * MAX_NN - 1];
    ll lazy[2 * MAX_NN - 1]; // lazy[u] != 0 : the subtree of u (u not included) is not up-to-date

    void seg_gather(int u) {
        seg[u] = seg[u * 2 + 1] + seg[u * 2 + 2];
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
        NN = 1;
        while (NN < N)
            NN *= 2;

        memset(seg, 0, sizeof(seg));    // val that won't affect result
        memset(lazy, 0, sizeof(lazy));  // val that won't affect result
        memcpy(seg + NN - 1, inp, sizeof(ll) * N); // fill in leaves
    }

    void seg_build(int u) {
        if (u >= NN - 1) { // leaf
            return;
        }

        seg_build(u * 2 + 1);
        seg_build(u * 2 + 2);
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

`poj3264 <http://codepad.org/rQdI5Xv6>`_
`poj3468 <http://codepad.org/ITp1iOPE>`_
