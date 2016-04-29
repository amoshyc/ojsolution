###################################################
F. Cutting Sticks
###################################################

.. sidebar:: Tags

    - ``tag_easy``

.. contents:: TOC
    :depth: 2


*******************************************************************************
`題目 <http://e-tutor.itsa.org.tw/e-Tutor/mod/programming/view.php?id=23651>`_
*******************************************************************************

給定長度為 N 的木頭，與 K 個切點 p[i]，你可以自由決定哪點切點要先切，但每個切點最後都要切到。
當一個切點被切了之後，木頭被切成兩段，而切一個切點的成本，為那兩段木頭的長度和。
請問在切完所有切點的成本最小可能是多少？

************************
Specification
************************

::

    2 <= N <= 100
    1 <= K < N
    1 <= p[0] < p[1] < ... < p[N - 1] < N

************************
分析
************************

.. note:: 經典 dp

太經典了，就不多說了，直接給式子::

    dp[i][j] = 使編號在 (i, j) 這個開區間中的所有切點都被切的最小成本

考慮下一次要切哪個切點，不知道，每個都試試看，看誰的成本最小::

    dp[i][j] = min([
        dp[i][c] + dp[c][j] + (p[j] - p[i]) for i < c < j
    ])

初使條件::

    dp[i][i + 1] = 0

不易用 bottom-up 的寫法來實作，就用 top-down 吧。

另外實作時，為了方便，會在::

    p[0] = 0
    pos[K + 1] = N

這兩個地方各補一個切點。
則所求即為 dp[0][K + 1]。

************************
AC Code
************************

.. code-block:: cpp
    :linenos:

    #include <iostream>
    #include <vector>
    #include <algorithm>
    #include <cstring>
    using namespace std;

    const int max_n = 100;
    int N, K;
    int pos[max_n + 2];
    int dp[max_n + 2][max_n + 2];

    // 切 (i, j) 所有切點的最小成本
    int solve(int i, int j) {
        if (dp[i][j] != -1)
            return dp[i][j];

        if (i + 1 == j) {
            dp[i][j] = 0;
            return dp[i][j];
        }

        int ans = 0x3f3f3f3f;
        for (int c = i + 1; c < j; c++) {
            ans = min(ans, solve(i, c) + solve(c, j));
        }

        dp[i][j] = ans + pos[j] - pos[i];
        return dp[i][j];
    }

    int main() {
        ios::sync_with_stdio(false);
        cin.tie(0);

        int TC; cin >> TC;
        while(TC--) {
            cin >> N >> K;
            for (int i = 1; i <= K; i++)
                cin >> pos[i];
            pos[0] = 0;
            pos[K + 1] = N;

            memset(dp, -1, sizeof(dp));
            cout << solve(0, K + 1) << "\n";
        }

        return 0;
    }
