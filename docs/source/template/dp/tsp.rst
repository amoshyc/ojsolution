###################################################
TSP
###################################################

.. sidebar:: Tags

    - ``tag_dp``
    - ``tag_template``

.. contents:: TOC
    :depth: 2

************************
程式碼 1
************************

.. code-block:: cpp
    :linenos:

    // dp[S][v] = 訪問過的點集合為 S，且從目前所在點 v，回到頂點 0 的路徑的最小權重和。
    // （頂點 0 尚未訪問）
    //
    // 從所有尚未訪問過的集合中找轉移的最小值
    // dp[V][0] = 0
    // dp[S][v] = min([
    //     dp[S ∪ {u}][u] + d(v, u) for u not in S
    // ])

    const int MAX_N = ...;
    const int INF = 0x3f3f3f3f;
    int N;
    int dp[1 << MAX_N][MAX_N];

    int tsp() {
        for (int S = 0; S < (1 << N); S++)
            fill(dp[S], dp[S] + N, INF);

        dp[(1 << N) - 1][0] = 0;
        for (int S = (1 << N) - 2; S >= 0; S--)
            for (int v = 0; v < N; v++)
                for (int u = 0; u < N; u++)
                    if (!((S >> u) & 1))
                        dp[S][v] = min(dp[S][v], dp[S | (1 << u)][u] + d[v][u]);

        return dp[0][0];
    }


************************
模板驗證
************************

待測
