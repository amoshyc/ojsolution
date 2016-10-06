########################################
[cf169] C. Little Girl and Maximum Sum
########################################

.. sidebar:: Tags

    - ``tag_greedy``
    - ``tag_sorting``

.. contents:: TOC
    :depth: 2

*********************************************************
`題目 <http://codeforces.com/contest/276/problem/C>`_
*********************************************************

給定長度為 N 的序列 A[]，與 Q 筆詢問 [s, t]，代表區間和 sum(A[s, t])。
請將 A 重排，使得所有詢問的結果加總是最大的。

************************
Specification
************************

************************
分析
************************

.. note:: 被詢問區間涵蓋到越多次的位置放越大的數

被區間涵蓋到越多次的位置放越大的數。而統計每個位置被含蓋到幾次，一個常用技巧為：對每個詢問區間 [s, t], F[s]++, F[t + 1]--，那 A[i] 被涵蓋到的次數即為 sum(F[0, i])，一開始 F 每一項都為 0。

這可以使用前綴和或 BIT 來實作。

************************
AC Code
************************

.. code-block:: cpp
    :linenos:

    #include <bits/stdc++.h>
    using namespace std;

    typedef long long ll;
    const int MAX_N = 200000;

    int N, Q;
    int A[MAX_N];
    int F[MAX_N + 1];
    ll S[MAX_N];

    int main() {
        scanf("%d %d", &N, &Q);

        for (int i = 0; i < N; i++) {
            scanf("%d", &A[i]);
        }

        for (int i = 0; i < Q; i++) {
            int l, r; scanf("%d %d", &l, &r); l--; r--;
            F[l]++;
            F[r + 1]--;
        }

        S[0] = F[0];
        for (int i = 1; i < N; i++) {
            S[i] = S[i - 1] + F[i];
        }

        sort(A, A + N);
        sort(S, S + N);

        ll ans = 0;
        for (int i = 0; i < N; i++) {
            ans += A[i] * S[i];
        }

        printf("%lld\n", ans);

        return 0;
    }
