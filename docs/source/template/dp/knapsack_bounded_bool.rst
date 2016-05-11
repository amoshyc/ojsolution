###################################################
多重背包可行性
###################################################

.. sidebar:: Tags

    - ``tag_dp``
    - ``tag_template``

.. contents:: TOC
    :depth: 2

************************
程式碼
************************

::

    當成 無限背包 來做，但另外記錄硬幣的使用個數。

    bool dp[w] = 可不可以組出 w
    int num[w] = 組出 w 需使用幾個 A[i] 硬幣


.. code-block:: cpp
    :linenos:

    int N, M;
    int A[100];
    int C[100];
    bool dp[100000];
    int num[100000];

    int solve() {
        memset(dp, false, sizeof(dp));

        dp[0] = true;
        for (int i = 0; i < N; i++) {
            // num[j] 記錄組出 j 需使用幾個 **A[i]** 硬幣。
            memset(num, 0, sizeof(num));

            // j 從 A[i] 開始，當然。比 A[i] 小的，不可能用 A[i] 組出。
            for (int j = A[i]; j <= M; j++) {
                // 如果 j 尚無法組出，
                // 且 j - A[i] 可以組出，且硬幣數量沒有超過（至少還有一枚可以用）
                if (dp[j] == false && dp[j - A[i]] && num[j - A[i]] < C[i]) {
                    dp[j] = true; // j 就可以組出
                    num[j] = num[j - A[i]] + 1;
                    // 組出 j 的硬幣數量為組出 j - A[i] 的數量再加一。
                }
            }
        }

        return count(dp + 1, dp + M + 1, true);
    }


************************
模板驗證
************************

`poj1742 <http://codepad.org/ABfw2sno>`_
