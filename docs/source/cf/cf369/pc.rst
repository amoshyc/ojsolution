#####################################
[cf369] C. Coloring Trees
#####################################

.. sidebar:: Tags

    - ``tag_dp``

.. contents:: TOC
    :depth: 2

******************************************************
`題目 <http://codeforces.com/contest/711/problem/C>`_
******************************************************

給定一個長度為 N 的序列 c，與整數 M, K，與一 NxM 的矩陣 p。
該序列可以分割成幾個有相同值的連續區間，序列的 beauty 定義為該區間數量的最小值。
例如::

    2, 1, 1, 1, 3, 2, 2 -> [2], [1, 1, 1], [3], [2, 2] -> beauty = 4

序列中有幾項是 0，代表該格沒數字，你可以填入任何 1~M 的數。
如果是在位置 i 填入數字 j，那成本是 p[i][j]。

請問要把所有的 0 都填入數字，使得序列的 beauty 為 K，所需的最小成本和是多少？
※ 如果 c[i] 不為 0，你不能在該格填入數字。

************************
Specification
************************

::

    1 <= K <= N <= 100
    1 <= M <= 100
    0 <= c[i] <= M
    1 <= p[i][j] <= 10^9

************************
分析
************************

.. note:: 三維 dp

這個 dp…有點難啊。

::

    dp[i][j][k] =
        使 c[1, i] 都有數字，且 c[1, i] 的 beauty 為 j，且 c[i] = k 的最小成本
        沒定義時，為 INF

    答案為 min(dp[N][K][x] for x in [1, M])

考慮第 i 格

1.  如果 c[i] 不為 0，代表 dp[i][j][x] (x ≠ c[i]) 都沒被定義。
    而 dp[i][j][c[i]] 有被定義，其值可以透過枚舉 i - 1 的數字 z 來獲得
    注意 beauty 值的變動。詳細的說，
    當 z == c[i] 時，是回溯到 dp[i - 1][j - 1][z]
    當 z != c[i] 時，是回溯到 dp[i - 1][j][z]
    ::

        for (int z = 1; z <= M; z++) { // color at (i - 1)-th
            if (z == c[i])
                upd_min(dp[i][j][c[i]], dp[i - 1][j][z]);
            else
                upd_min(dp[i][j][c[i]], dp[i - 1][j - 1][z]);
        }

2.  如果為 c[i] 為 0，我們可以填入 [1, M] 的數字，即 dp[i][j][k] (k in [1, M]) 都有定義。
    而 dp[i][j][k] 的求法跟前一個情況類似，枚舉 i - 1 的數字 z，找最小。
    記得要加入成本，寫程式碼::

        for (int k = 1; k <= M; k++) { // color at i-th
            for (int z = 1; z <= M; z++) { // color at (i - 1)-th
                if (z == k)
                    upd_min(dp[i][j][k], dp[i - 1][j][z] + p[i][k]);
                else
                    upd_min(dp[i][j][k], dp[i - 1][j - 1][z] + p[i][k]);
            }
        }

實作上，全部使用 1-based index 會比較方便。

************************
AC Code
************************

.. code-block:: cpp
    :linenos:

    #include <bits/stdc++.h>
    using namespace std;

    typedef long long ll;

    const int MAX_N = 100;
    const int MAX_M = 100;
    const int MAX_K = 100;
    const ll INF = 1e18;

    // all 1-based index
    int N, M, K;
    int c[MAX_N + 1];
    ll p[MAX_N + 1][MAX_M + 1];
    ll dp[MAX_N + 1][MAX_K + 1][MAX_M + 1];

    void upd_min(ll& a, ll b) {
        a = min(a, b);
    }

    int main() {
        scanf("%d %d %d", &N, &M, &K);
        for (int i = 1; i <= N; i++)
            scanf("%d", &c[i]);
        for (int i = 1; i <= N; i++)
            for (int j = 1; j <= M; j++)
                scanf("%lld", &p[i][j]);

        for (int i = 0; i <= N; i++) {
            for (int j = 0; j <= K; j++) {
                for (int k = 0; k <= M; k++) {
                    dp[i][j][k] = INF;
                }
            }
        }

        // base case
        if (c[1] != 0) dp[1][1][c[1]] = 0;
        else {
            for (int k = 1; k <= M; k++) {
                dp[1][1][k] = p[1][k];
            }
        }

        for (int i = 2; i <= N; i++) {
            for (int j = 1; j <= i; j++) {
                if (c[i] != 0) { // c[i] is fixed
                    for (int z = 1; z <= M; z++) { // color at (i - 1)-th
                        if (z == c[i])
                            upd_min(dp[i][j][c[i]], dp[i - 1][j][z]);
                        else
                            upd_min(dp[i][j][c[i]], dp[i - 1][j - 1][z]);
                    }
                }
                else { // c[i] is not fixed
                    for (int k = 1; k <= M; k++) { // color at i-th
                        for (int z = 1; z <= M; z++) { // color at (i - 1)-th
                            if (z == k)
                                upd_min(dp[i][j][k], dp[i - 1][j][z] + p[i][k]);
                            else
                                upd_min(dp[i][j][k], dp[i - 1][j - 1][z] + p[i][k]);
                        }
                    }
                }
            }
        }

        // for (int i = 0; i <= N; i++) {
        //     for (int j = 0; j <= i; j++) {
        //         for (int k = 1; k <= M; k++) {
        //             printf("dp[%d][%d][%d] = %lld\n", i, j, k, dp[i][j][k]);
        //         }
        //     }
        //     puts("");
        // }

        ll ans = *min_element(dp[N][K] + 1, dp[N][K] + M + 1);
        if (ans >= INF) puts("-1");
        else {
            printf("%lld\n", ans);
        }

        return 0;
    }
