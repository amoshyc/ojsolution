#####################################
[cf367] C. Hard problem
#####################################

.. sidebar:: Tags

    - ``tag_dp``

.. contents:: TOC
    :depth: 2

******************************************************
`題目 <http://codeforces.com/contest/706/problem/C>`_
******************************************************

給定 N 個字串，只允許一種操作：將某個字串反轉，不允許交換等其它操作。
反轉字串 i 需要成本 c[i]，請問要使這 N 個字串依造字典順序排好,
成本最小是多少？無解時輸出 -1。

************************
Specification
************************

::

    2 <= N <= 10^5
    0 <= c[i] <= 10^9
    字串長度總和 <= 10^5

************************
分析
************************

.. note:: 經典題

增維 dp 經典題::

    dp[i][0] = 使前 i + 1 個字串變成字典順序所需的最小成本，且第 i 個字串不轉
    dp[i][1] = 使前 i + 1 個字串變成字典順序所需的最小成本，且第 i 個字串反轉

dp 這樣設了之後，轉移方程式就好寫了，直接看 code 吧。

************************
AC Code
************************

.. code-block:: cpp
    :linenos:

    #include <bits/stdc++.h>
    using namespace std;

    typedef long long ll;
    const int MAX_N = 1e5;
    const ll INF = 1e18;
    int N;
    ll w[MAX_N];
    ll dp[MAX_N][2];

    int main() {
        ios::sync_with_stdio(false);
        cin.tie(0);

        int N;
        cin >> N;

        for (int i = 0; i < N; i++) {
            cin >> w[i];
        }

        string p, rp, s, rs;
        cin >> p;
        rp = p;
        reverse(rp.begin(), rp.end());

        dp[0][0] = 0;
        dp[0][1] = w[0];

        for (int i = 1; i < N; i++) {
            cin >> s;
            rs = s;
            reverse(rs.begin(), rs.end());

            dp[i][0] = INF;
            if (s >= p) {
                dp[i][0] = min(dp[i][0], dp[i - 1][0]);
            }
            if (s >= rp) {
                dp[i][0] = min(dp[i][0], dp[i - 1][1]);
            }

            dp[i][1] = INF;
            if (rs >= p) {
                dp[i][1] = min(dp[i][1], dp[i - 1][0] + w[i]);
            }
            if (rs >= rp) {
                dp[i][1] = min(dp[i][1], dp[i - 1][1] + w[i]);
            }

            p = s;
            rp = rs;
        }

        if (dp[N - 1][0] >= INF && dp[N - 1][1] >= INF) {
            cout << "-1" << endl;
        }
        else {
            cout << min(dp[N - 1][0], dp[N - 1][1]) << endl;
        }

        return 0;
    }
