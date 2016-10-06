########################################
[cf202] C. Mafia
########################################

.. sidebar:: Tags

    - ``tag_greedy``
    - ``tag_binary_search``

.. contents:: TOC
    :depth: 2

*********************************************************
`題目 <http://codeforces.com/contest/349/problem/C>`_
*********************************************************

************************
Specification
************************

************************
分析
************************

.. note:: 二分搜水題

二分搜水題，官方題解也有提供 Greedy 的解法

************************
AC Code
************************

.. code-block:: cpp
    :linenos:

    #include <bits/stdc++.h>
    using namespace std;

    typedef long long ll;
    const int MAX_N = 100000;

    int N;
    int A[MAX_N];

    bool C(ll m) {
        ll cnt = 0;
        for (int i = 0; i < N; i++) {
            if (m < A[i]) return false;
            cnt += m - A[i];
        }
        return cnt >= m; // 能提供的裁判數是否 >= m
    }

    int main() {
        scanf("%d", &N);
        for (int i = 0; i < N; i++)
            scanf("%d", &A[i]);

        // binary search
        // C(x) = 玩 x 場能否使所有人滿足，需要 x 個裁判
        // 0 0 0 1 1 1
        ll lb = 0, ub = 1e10;
        // if (C(lb)) impossible;
        // if (!C(ub)) impossible;
        while (ub - lb > 1) {
            ll m = (lb + ub) / 2;
            if (C(m)) ub = m;
            else lb = m;
        }

        printf("%lld\n", ub);

        return 0;
    }
