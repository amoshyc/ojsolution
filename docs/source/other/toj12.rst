################################
[TOJ] 12. 遙控器
################################

.. sidebar:: Tags

    - ``tag_bit2d``

.. contents:: TOC
    :depth: 2


***************************************************
`題目 <http://toj.tfcis.org/oj/pro/12/>`_
***************************************************

遙控器上有 NxM 個按鍵，每個按鍵上有值 q，請支援兩種操作，共有 Q 個：

- ``C r c k``: 將 (r, c) 改成 k
- ``Q r1 c1 r2 c2``: 輸出 (r1, c2) ~ (r2, c2) 這個矩形區塊的按鍵值和

************************
Specification
************************

::

    1 <= N, M <= 3000
    1 <= Q <= 10000
    1 <= q <= 50
    1 <= r <= N
    1 <= c <= c
    1 <= r1 <= r2 <= N
    1 <= c1 <= c2 <= M

************************
分析
************************

.. note:: 二維 bit

二維 bit 裸題，但時限卡得非常緊，我覺得我已經優化到極限了，仍然有時 AC，有時 TLE。

************************
AC Code
************************

.. code-block:: cpp
    :linenos:

    #include <cstdio>
    using namespace std;

    typedef long long ll;

    const int MAX_N = 3000;
    int N, M, Q;
    ll data[MAX_N][MAX_N];
    ll bit[MAX_N + 1][MAX_N + 1];

    inline ll sum(int a, int b) {
        ll s = 0;
        for (int i = a; i > 0; i -= (i & -i))
            for (int j = b; j > 0; j -= (j & -j))
                s += bit[i][j];
        return s;
    }

    inline void add(int a, int b, ll x) {
        for (int i = a; i <= N; i += (i & -i))
            for (int j = b; j <= M; j += (j & -j))
                bit[i][j] += x;
    }

    int main() {
        scanf("%d %d", &N, &M);
        for (int r = 0; r < N; r++) {
            for (int c = 0; c < M; c++) {
                scanf("%lld", &data[r][c]);
                add(r + 1, c + 1, data[r][c]); // 1-based index
            }
        }

        scanf("%d\n", &Q);
        while (Q--) {
            char cmd[5]; scanf("%s", cmd);
            if (cmd[0] == 'C') {
                int r, c, k;
                scanf("%d %d %d", &r, &c, &k);
                r--; c--;
                add(r + 1, c + 1, -data[r][c] + k);
                data[r][c] = k;
            }
            else {
                int r1, c1, r2, c2;
                scanf("%d %d %d %d", &r1, &c1, &r2, &c2);

                ll A = sum(r2, c2), D = sum(r1 - 1, c1 - 1);
                ll B = sum(r2, c1 - 1), C = sum(r1 - 1, c2);
                printf("%lld\n", A - B - C + D);
            }
        }

        return 0;
    }
