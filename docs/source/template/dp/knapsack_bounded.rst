###################################################
多重背包
###################################################

.. sidebar:: Tags

    - ``tag_dp``
    - ``tag_template``

.. contents:: TOC
    :depth: 2

************************
程式碼
************************

.. code-block:: cpp
    :linenos:

    const int MAX_N = 20;
    const int MAX_W = 1000;
    int N, W;
    ll w[MAX_N + 1];
    ll v[MAX_N + 1];
    ll n[MAX_N + 1];
    ll dp[MAX_N + 1][MAX_W + 1];

    void knapsack() {
        for (int i = 1; i <= N; i++)
            for (int j = 0; j <= W; j++)
                for (int k = 0; k <= n[i] && j - k * w[i] >= 0; k++)
                    dp[i][j] = max(dp[i][j], dp[i - 1][j - k * w[i]] + k * v[i]);
    }


************************
模板驗證
************************

待測
