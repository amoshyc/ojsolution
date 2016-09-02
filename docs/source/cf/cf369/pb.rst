#####################################
[cf369] B. Chris and Magic Square
#####################################

.. sidebar:: Tags

    - ``tag_easy``

.. contents:: TOC
    :depth: 2

******************************************************
`題目 <http://codeforces.com/contest/711/problem/B>`_
******************************************************

************************
Specification
************************

************************
分析
************************

.. note:: 水題

************************
AC Code
************************

.. code-block:: cpp
    :linenos:

    #include <bits/stdc++.h>
    using namespace std;

    typedef long long ll;

    int N;
    int er, ec;
    ll sum, val;
    ll A[500][500];

    bool check() {
        // check val
        if (val <= 0 || val > ll(1e18))
            return false;

        // check rows
        for (int r = 0; r < N; r++) {
            ll s = 0;
            for (int c = 0; c < N; c++) {
                s += A[r][c];
            }
            if (s != sum)
                return false;
        }

        // check cols
        for (int c = 0; c < N; c++) {
            ll s = 0;
            for (int r = 0; r < N; r++) {
                s += A[r][c];
            }
            if (s != sum)
                return false;
        }

        // check D1
        ll d1 = 0;
        for (int i = 0; i < N; i++) {
            d1 += A[i][i];
        }
        if (d1 != sum)
            return false;

        // check D2
        ll d2 = 0;
        for (int i = 0; i < N; i++) {
            d2 += A[N - 1 - i][i];
        }
        if (d2 != sum)
            return false;

        return true;
    }

    int main() {
        // ios::sync_with_stdio(false);
        // cin.tie(0);
        // cout.tie(0);

        scanf("%d", &N);
        for (int r = 0; r < N; r++) {
            for (int c = 0; c < N; c++) {
                scanf("%lld", &A[r][c]);

                if (A[r][c] == 0) {
                    er = r;
                    ec = c;
                }
            }
        }

        if (N == 1) {
            puts("1");
            return 0;
        }

        sum = accumulate(A[(er + 1) % N], A[(er + 1) % N] + N, 0ll);
        val = sum - accumulate(A[er], A[er] + N, 0ll);
        A[er][ec] = val;

        // printf("%lld\n", sum);
        // printf("%lld\n", val);

        if (!check()) puts("-1");
        else {
            printf("%lld\n", val);
        }

        return 0;
    }
