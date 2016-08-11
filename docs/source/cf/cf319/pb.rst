#####################################
[cf319] B. Modulo Sum
#####################################

.. sidebar:: Tags

    - ``tag_dp``
    - ``tag_pigeonhole``

.. contents:: TOC
    :depth: 2

******************************************************
`題目 <http://codeforces.com/contest/577/problem/B>`_
******************************************************

給定長度為 N 的序列 A 與一正整數 M，
請問該序列中有無一個子序列，子序列的總合是 M 的倍數

************************
Specification
************************

::

    1 <= N <= 10^6
    2 <= M <= 10^3

************************
分析
************************

.. note:: 分段討論

考慮序列的前綴和，長度為 N 的序列會有 N 個前綴和。
若 N > M，則根據鴿籠原理，必有至少兩個前綴和的值 mod M 為相同值。
假設這兩個前綴和分別為 S[i], S[j]，
那 **子字串** （也是子序列） A[j+1 : i] 的和 S[i] - S[j] mod M 後必為零。
所以只要 N > M，解必定存在。

------------------------------------

若 N <= M，則可以使用 dp 解，時間為 O(M^2)::

    dp[i][j] = 前 i + 1 個數可否組出 mod m = j 的數
    dp[i][j] = true if
        dp[i - 1][(j - (a[i] mod m)) mod m] or
        dp[i - 1][j] or
        j = a[i] % m

最後檢查 dp[N - 1][0] 是否可行。

其中要注意 j - (a[i] % m) 可能為負數，
所以計算 j - (a[i] % m) mod m 時，再加上 m 再 mod

************************
AC Code
************************

.. code-block:: cpp
    :linenos:

    #include <bits/stdc++.h>
    using namespace std;

    const int MAX_N = 1e6;
    const int MAX_M = 1e3;

    int N, M;
    int A[MAX_N];
    bool dp[MAX_M][MAX_M];

    inline int mod(int a, int b) {
        return (a + b) % b;
    }

    int main() {
        scanf("%d %d", &N, &M);
        for (int i = 0; i < N; i++)
            scanf("%d", &A[i]);

        if (N > M) {
            puts("YES");
            return 0;
        }

        // dp[i][j] = 前 i + 1 個數可否組出 mod m = j 的數
        // dp[i][j] = true if dp[i - 1][(j - (a[i] mod m)) mod m] or
        //                    dp[i - 1][j] or
        //                    j = a[i] % m

        dp[0][A[0] % M] = true;
        for (int i = 1; i < N; i++) {
            dp[i][A[i] % M] = true;
            for (int j = 0; j < M; j++) {
                if (dp[i - 1][j] || dp[i - 1][mod(j - A[i] % M, M)]) {
                    dp[i][j] = true;
                }
            }
        }

        puts(((dp[N - 1][0]) ? "YES" : "NO"));

        return 0;
    }
