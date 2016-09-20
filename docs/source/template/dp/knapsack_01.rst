###################################################
0/1 背包問題
###################################################

.. sidebar:: Tags

    - ``tag_dp``

.. contents:: TOC
    :depth: 2

************************
程式碼
************************

::

    dp[i][j] = 使用第 1 ~ i 種物件，在總重 j 的限制下的最大價值
    dp[0][0] = 0
    考慮第 i 種要用不用：
    dp[i][j] = max(dp[i - 1][j], dp[i - 1][j - w[i]] + v[i])

.. code-block:: cpp
    :linenos:

    typedef long long ll;

    struct Item {
        ll v, w;
    };

    const int MAX_W = 100000;
    ll dp[MAX_W + 1];

    ll knapsack01(const vector<Item>& items, int W) {
        fill(dp, dp + W + 1, 0ll);
        for (int i = 0; i < int(items.size()); i++) {
            for (int j = W; j >= items[i].w; j--) {
                dp[j] = max(dp[j], dp[j - items[i].w] + items[i].v);
            }
        }
        return dp[W];
    }

************************
模板驗證
************************

`poj1276 <../../poj/p1276.html>`_
