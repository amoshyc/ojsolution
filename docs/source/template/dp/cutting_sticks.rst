###################################################
Cutting Sticks
###################################################

.. sidebar:: Tags

    - ``tag_dp``

.. contents:: TOC
    :depth: 2

************************
程式碼
************************

.. code-block:: cpp
    :linenos:

    int N, K;
    int pos[MAX_N + 2];
    int dp[MAX_N + 2][MAX_N + 2];

    pos[0] = 0;
    pos[K + 1] = N;

    int solve(int i, int j) { // 切 (i, j) 所有切點的最小成本, top down dp
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

    int ans = solve(0, K + 1);

************************
模板驗證
************************

`[桂冠賽#4]pf <../../ptc/contest4/pf.html>`_
