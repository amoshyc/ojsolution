###################################################
LIS
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

    int N;
    int A[MAX_N];
    int dp[MAX_N]; // dp[i] = 長度為 i + 1 的 LIS 最後一項的最小值

    fill(dp, dp + N, INF);
    for (int i = 0; i < N; i++) {
        *lower_bound(dp, dp + N, A[i]) = A[i];
    }
    int ans = *lower_bound(dp, dp + N, A[i]) - dp;

************************
模板驗證
************************

`uva10986 <http://codepad.org/nEGXuSYA>`_
