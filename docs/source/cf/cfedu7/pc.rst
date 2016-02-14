#####################################
[cfedu7] C. Not Equal on a Segment
#####################################

.. sidebar:: Tags

    - ``tag_dp``

.. contents:: TOC
    :depth: 2


******************************************************
`題目 <http://codeforces.com/contest/622/problem/C>`_
******************************************************

給定長度為 n 的陣列 a[] 與 m 個詢問。
針對每個詢問 l, r, x 請輸出 a[l, r] 中不等於 x 的任一位置。
不存在時輸出 -1

************************
Specification
************************

::

    1 <= n <= 200000
    1 <= m <= 200000
    1 <= a[i] <= 10^6


************************
分析
************************

.. note:: dp

::

    dp[i] = max j such that j < i and a[j] != a[i]
    dp[0] = -1
    dp[i] =
        if a[i] == a[i - 1], dp[i - 1]
        if a[i] != a[i - 1], i - 1

dp 表可在 O(n) 的時間建出來，
之後針對每筆詢問 l, r, x，有三種情況：

::

    1. a[r] != x                -> 輸出 r
    2. a[r] = x && dp[r] >= l   -> 輸出 dp[r]
    3. a[r] = x && dp[r] < l    -> 輸出 -1

記得輸出要跟輸入一樣，是 1-based index。

************************
AC Code
************************

.. code-block:: cpp
    :linenos:

    #include <bits/stdc++.h>
    using namespace std;

    const int max_n = 200000;

    int n, m;
    int a[max_n];
    int dp[max_n];

    int main() {
        scanf("%d %d", &n, &m);
        for (int i = 0; i < n; i++)
            scanf("%d", &a[i]);

        dp[0] = -1;
        for (int i = 1; i < n; i++) {
            if (a[i] == a[i - 1]) dp[i] = dp[i - 1];
            else dp[i] = i - 1;
        }

        while (m--) {
            int l, r, x;
            scanf("%d %d %d", &l, &r, &x);
            l--; r--;

            if (a[r] != x) printf("%d\n", r + 1);
            else {
                if (dp[r] >= l) printf("%d\n", dp[r] + 1);
                else printf("-1\n");
            }
        }

        return 0;
    }
