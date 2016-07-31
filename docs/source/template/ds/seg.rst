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
程式碼（單點更新）
************************

.. code-block:: cpp

    template<class T>
    struct SegTree {
        int NN;
        vector<T> seg;
        T def; // default vale

        T func(T a, T b) {
            return min(a, b);
        }

        void gather(int u) {
            seg[u] = func(seg[u * 2 + 1], seg[u * 2 + 2]);
        }

        void init(int N, T d, T* inp) {
            def = d;
            NN = 1;
            while (NN < N)
                NN *= 2;

            seg = vector<T>(2 * NN - 1, def);
            for (int i = 0; i < N; i++) {
                seg[NN - 1 + i] = inp[i];
            }

            build(0);
        }

        void build(int u) {
            if (u >= NN - 1) { // leaf
                return;
            }
            build(u * 2 + 1);
            build(u * 2 + 2);
            gather(u);
        }

        void _update(int idx, T val, int u, int l, int r) {
            if (l > idx || r <= idx) {
                return;
            }

            if (l == idx && idx + 1 == r) {
                seg[u] = val;
                return;
            }

            int m = (l + r) / 2;
            _update(idx, val, u * 2 + 1, l, m);
            _update(idx, val, u * 2 + 2, m, r);
            gather(u);
        }

        T _query(int a, int b, int u, int l, int r) {
            if (l >= b || r <= a) {
                return def;
            }

            if (a <= l && r <= b) {
                return seg[u];
            }

            int m = (l + r) / 2;
            T res1 = _query(a, b, u * 2 + 1, l, m);
            T res2 = _query(a, b, u * 2 + 2, m, r);

            return func(res1, res2);
        }

        void update(int idx, T val) {
            _update(idx, val, 0, 0, NN);
        }

        T query(int a, int b) {
            return _query(a, b, 0, 0, NN);
        }
    };

************************
模板驗證
************************

 - [不用更新] `poj3264 <http://codepad.org/rQdI5Xv6>`_
 - [延遲標記] `poj3468 <http://codepad.org/ITp1iOPE>`_
