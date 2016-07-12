#####################################
[cf208] B. Maximum Absurdity
#####################################

.. sidebar:: Tags

    - ``tag_enum``

.. contents:: TOC
    :depth: 2


******************************************************
`題目 <http://codeforces.com/contest/332/problem/B>`_
******************************************************

給定長度為 N 的序列 A[]，與一整數 K，請問 A 中哪兩個長度為 K 的不相交區間，有最大的總和

************************
Specification
************************

::

    2 <= N <= 2*10^5
    1 <= A[i] <= 10^9
    0 < 2 * K <= N

************************
分析
************************

.. note:: 兩次 dp

這個 dp 沒那麼好想啊，不過還是被我想出來了，哈 XD

首先，R[i] 代表右端點為 i 的區間和。存在轉移方程式::

    R[i] = R[i - 1] - A[i - K] + A[i]

接下來，我只要知道「每個區間的左邊，不與之相交的區間中，誰的和最大」（dp[i]），
區間 i 與之結合，即可產生最大的總和。

※ 區間 i 有可能與其右的區間 j 結合的情況會與在枚舉 j 時考慮到。

dp[i] 正式定義為::

    dp[i] = x such that R[x] is maximum among R[0, i - K]

dp[i] 存在轉移方程式。

.. code-block:: cpp

    if (R[i - K] > R[dp[i - 1]]) {
        dp[i] = i - K;
    }
    else {
        dp[i] = dp[i - 1];
    }

枚舉區間 i，看哪個區間與他的 dp[i] 結合時，能產生最大的值。

************************
AC Code
************************

.. code-block:: cpp
    :linenos:

    #include <bits/stdc++.h>
    using namespace std;

    typedef long long ll;

    const int MAX_N = 200000;
    int N, K;
    ll A[MAX_N];
    // R[i] = sum of A[i - K + 1, i]
    // dp[i] = x such that R[x] is max among R[0, i - K]
    ll R[MAX_N];
    ll dp[MAX_N];

    int main() {
        scanf("%d %d", &N, &K);
        for (int i = 0; i < N; i++) {
            scanf("%lld", &A[i]);
        }

        R[K - 1] = accumulate(A, A + K, 0ll);
        for (int i = K; i < N; i++) {
            R[i] = R[i - 1] - A[i - K] + A[i];
        }

        dp[K] = K - 1;
        for (int i = K + 1; i < N; i++) {
            if (R[i - K] > R[dp[i - 1]]) {
                dp[i] = i - K;
            }
            else {
                dp[i] = dp[i - 1];
            }
        }

        ll ans = -1; int idx1, idx2; // right end of segment in 0-based
        for (int i = 2 * K - 1; i < N; i++) {
            if (R[dp[i]] + R[i] > ans) {
                ans = R[dp[i]] + R[i];
                idx1 = dp[i];
                idx2 = i;
            }
        }

        printf("%d %d\n", idx1 - K + 2, idx2 - K + 2);

        return 0;
    }
