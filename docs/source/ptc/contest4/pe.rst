###################################################
E. Coin Change
###################################################

.. sidebar:: Tags

    - ``tag_dp``

.. contents:: TOC
    :depth: 2


*******************************************************************************
`題目 <http://e-tutor.itsa.org.tw/e-Tutor/mod/programming/view.php?id=23650>`_
*******************************************************************************

找零錢問題，N 筆測資，每筆測資有一個整數 M，問用 1, 5, 10, 20, 50 元總出 M 的方法數。

************************
Specification
************************

::

    1 <= N <= 50
    1 <= N <= 15000


************************
分析
************************

.. note:: 經典 dp

這題與完全背包類似，都是順序無關的 dp。

讓我們從最簡單的出發，再慢慢優化::

    ll dp[i][j] = 使用前 i + 1 種硬幣，組出 j 元的方法數

則有初使條件::

    dp[0][j] = 使用第 1 種硬幣（1 元）組出 j 元的方法數 = 1

考慮第 i 種硬幣要用幾個，則可以找到轉移方程式::

    dp[i][j] = sum( [
        dp[i - 1][j - k * coins[i]] for 0 <= k <= (j // coins[i])
    ])

時間為 O(N * M * N)，空間為 O(N * M)，都有點大了，雖然說這題這樣寫就可以 AC，
但還是讓我們優化一下。

跟完全背包問題一樣，根據轉移方程式，將 dp[i][j - coins[i]] 展開::

    dp[i][j] = sum( [
        dp[i - 1][j - coins[i] - k * coins[i]] for 0 <= k <= (j // coins[i])
    ])

即為::

    dp[i][j] = sum( [
        dp[i - 1][j - k * coins[i]] for 1 <= k <= (j // coins[i])
    ])

相比於原來的轉移方程式，只差 k 的都始值，於是我們可以將轉移方程式改寫成::

    dp[i][j] = dp[i - 1][j] + dp[i][j - coins[i]]

順利將時間壓掉一個維度。

------------------------------

最後，如同完全背包問題，再加上重覆使用陣列的技巧，也可以將空間壓掉一個維度::

    dp[j] = dp[j] + dp[j - coins[i]]

最終的時間複雜度為 O(N * M), 空間 O(M)


************************
AC Code 1
************************

.. code-block:: cpp
    :linenos:

    ll dp[5][15000 + 1];

    void init() {
        memset(dp, 0, sizeof(dp));

        for (int j = 0; j <= 15000; j++)
            dp[0][j] = 1;

        for (int i = 1; i < 5; i++) {
            for (ll j = 0; j <= 15000; j++) {
                for (int k = 0; k * coins[i] <= j; k++) {
                    dp[i][j] += dp[i - 1][j - coins[i] * k];
                }
            }
        }
    }

************************
AC Code 2
************************

.. code-block:: cpp
    :linenos:

    ll dp[5][15000 + 1];

    void init() {
        memset(dp, 0, sizeof(dp));

        for (int j = 0; j <= 15000; j++)
            dp[0][j] = 1;

        for (int i = 1; i < 5; i++) {
            for (int j = 0; j <= 15000; j++) {
                // dp[i][j] = dp[i - 1][j] + dp[i][j - coins[i]]
                dp[i][j] = dp[i - 1][j];
                if (j >= coins[i])
                    dp[i][j] += dp[i][j - coins[i]];
            }
        }
    }

************************
AC Code 3
************************

.. code-block:: cpp
    :linenos:

    #include <iostream>
    #include <vector>
    #include <algorithm>
    #include <cstring>
    using namespace std;

    typedef long long ll;

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

    int main() {
        ios::sync_with_stdio(false);
        cin.tie(0);

        init();

        int TC; cin >> TC;
        while (TC--) {
            int n;
            cin >> n;
            cout << dp[n] << "\n";
        }

        return 0;
    }
