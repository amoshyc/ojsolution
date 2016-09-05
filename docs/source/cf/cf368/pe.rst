#####################################
[cf368] E. Garlands
#####################################

.. sidebar:: Tags

    - ``tag_bit2d``
    - ``tag_offline``

.. contents:: TOC
    :depth: 2

******************************************************
`題目 <http://codeforces.com/contest/707/problem/E>`_
******************************************************

在 NxM 的平面上，有 K 個花環，每個花環由 len[i] 個連續格子所構成。
每個格子都屬於一個花環，一開始每個枚子的數字都是 1。
之後有 Q 筆操作，操作有兩種

1. ``Switch i``： 將所有第 i 個花環的格子的數字反轉（1 -> 0, 0 -> 1）
2. ``Ask r1 c1 r2 c2``: 輸出矩形 (r1, c1, r2, c2) 裡的數字總和

************************
Specification
************************

::

    1 <= N, M, K <= 2000
    1 <= len[i] <= 2000
    1 <= Q <= 10^6

************************
分析
************************

.. note:: 離線處理

第一種操作可以用 bool 陣列記錄每個花環是 1 還是 0。
第二種操作，因為常數小，可以直接記錄每次 Ask 時，各花環的反轉次數。

最後再迭代每個花環 i，每個 Ask j。如果花環 i 在 Ask j 中是 1，
那就將花環 i 在 Ask j 的矩形內的數字和 **加至** Ask j 的答案。
而計算數字和可以用 bit 2d 來實作。

----------------------

這不是官方解法…官方解答我看不懂…

************************
AC Code
************************

.. code-block:: cpp
    :linenos:

    #include <bits/stdc++.h>
    using namespace std;

    typedef long long ll;

    const int MAX_N = 2000;
    const int MAX_M = 2000;
    const int MAX_K = 2000;

    struct Query {
        int r1, r2, c1, c2; ll ans;
        bitset<MAX_K> is_on;
    };

    struct Point {
        int r, c; ll w;
    };

    int N, M, K, Q;
    vector<Query> queries;
    bitset<MAX_K> is_on;
    vector<Point> garland[MAX_K];

    ll bit[MAX_N + 1][MAX_M + 1]; // 1-based index
    ll sum(int a, int b) {
        ll s = 0;
        for (int i = a; i > 0; i -= (i & -i))
            for (int j = b; j > 0; j -= (j & -j))
                s += bit[i][j];
        return s;
    }
    void add(int a, int b, ll x) {
        for (int i = a; i <= N; i += (i & -i))
            for (int j = b; j <= M; j += (j & -j))
                bit[i][j] += x;
    }

    int main() {
        scanf("%d %d %d", &N, &M, &K);

        is_on.flip(); // every garland is on inititally

        for (int i = 0; i < K; i++) {
            int len; scanf("%d", &len);
            for (int j = 0; j < len; j++) {
                Point p;
                scanf("%d %d %lld", &p.r, &p.c, &p.w);
                garland[i].push_back(p);
            }
        }

        scanf("%d", &Q);
        while (Q--) {
            char cmd[10]; scanf("%s", cmd);

            if (cmd[0] == 'S') {
                int id; scanf("%d", &id); id--;
                is_on.flip(id);
            }
            else {
                Query q;
                scanf("%d %d %d %d", &q.r1, &q.c1, &q.r2, &q.c2);
                q.ans = 0;
                q.is_on = is_on;
                queries.push_back(q);
            }
        }

        for (int i = 0; i < K; i++) {
            auto &v = garland[i];

            for (auto p : v) {
                add(p.r, p.c, +p.w);
            }

            for (auto &q : queries) {
                if (q.is_on[i]) {
                    ll cnt = 0;
                    cnt += sum(q.r2, q.c2);
                    cnt -= sum(q.r1 - 1, q.c2);
                    cnt -= sum(q.r2, q.c1 - 1);
                    cnt += sum(q.r1 - 1, q.c1 - 1);
                    q.ans += cnt;
                }
            }

            for (auto p : v) {
                add(p.r, p.c, -p.w);
            }
        }


        for (auto q : queries) {
            printf("%lld\n", q.ans);
        }

        return 0;
    }
