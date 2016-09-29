###################################################
LIS
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

    // dp[i] = max(dp[j] | 0 <= j < i) + 1

************************
程式碼 2
************************

.. code-block:: cpp
    :linenos:

    int N;
    int A[MAX_N];
    int dp[MAX_N]; // dp[i] = 長度為 i + 1 的 LIS 最後一項的最小值

    fill(dp, dp + N, INF);
    for (int i = 0; i < N; i++) {
        *lower_bound(dp, dp + N, A[i]) = A[i];
    }
    int ans = lower_bound(dp, dp + N, INF) - dp;

************************
模板驗證
************************
