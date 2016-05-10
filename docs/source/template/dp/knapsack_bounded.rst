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

    int N, W, w[i], v[i], n[i];
    int dp[MAX_N + 1][MAX_W + 1] = {0};

    for (int i = 1; i <= N; i++)
        for (int j = 0; j <= W; j++)
            for (int k = 0; k <= n[i] && j - k * w[i] >= 0; k++)
                dp[i][j] = max(dp[i - 1][j - k * w[i]] + k * v[i]);


************************
模板驗證
************************

待測

.. `uva10986 <http://codepad.org/nEGXuSYA>`_