###################################################
Coin Change
###################################################

.. sidebar:: Tags

    - ``tag_dp``

.. contents:: TOC
    :depth: 2

************************
程式碼
************************

::

    ll dp[i][j] = 使用前 i + 1 種硬幣，組出 j 元的方法數
    dp[i][j] = sum( [
     dp[i - 1][j - k * coins[i]] for 0 <= k <= (j // coins[i])
    ])
    dp[j] = dp[j] + dp[j - coins[i]]

.. code-block:: cpp
    :linenos:

    ll coins[5] = {1, 5, 10, 20, 50};
    ll dp[15000 + 1];

    void init() {
        memset(dp, 0, sizeof(dp));

        for (int j = 0; j <= 15000; j++)
            dp[j] = 1;
        for (int i = 1; i < 5; i++) {
            for (int j = coins[i]; j <= 15000; j++) {
                dp[j] += dp[j - coins[i]];
            }
        }
    }

************************
模板驗證
************************

`[桂冠賽#4]pe <../../ptc/contest4/pe.html>`_
