###################################################
[cfedu16] E. Generate a String
###################################################

.. sidebar:: Tags

    - ``tag_dp``
    - ``tag_spfa``

.. contents:: TOC
    :depth: 2


******************************************************
`題目 <http://codeforces.com/contest/710/problem/E>`_
******************************************************

允許三種操作：

1. 加入一個字元，成本為 x
2. 刪除一個字元，成本為 x
3. 將字串複製一份，接到原字串尾端，成本為 y

一開始字串為空字串，請問要產生長度為 n 的字串，成本最少是多少？

************************
Specification
************************

::

    1 <= n <= 10^7
    1 <= x, y <= 10^9

************************
分析
************************

.. note:: dp 順序

很容易可以想到 dp::

    dp[i] = 產生長度為 i 的字串的最少成本
    dp[i] = min(
        dp[i - 1] + x,
        dp[i + 1] + x,
        dp[i / 2] + y (if i is even)
    )

第一眼來看，這個 dp 是找不到建表順序，我們得調整轉移方程式。
我們考慮哪時候 dp[i + 1] + x 是 dp[i] 的答案。

仔細想一下，可以發現，只在::

    第三種操作，先產生比 i 長的字串，再刪掉一些字元，變成長度為 i 的字串。

這可以設計出新的轉移方程式，假設我們已求出 dp[1, i - 1]::

    dp[i] = min(
        dp[i - 1] + x,
        dp[(i + 1) / 2] + y + x (if i is odd),
        dp[i / 2] + y (if i is even)
    )

這樣就可以實作了。

----------------------------------

比賽時，我是想到用 dijkstra 做::

    節點 u 代表長度 u 的字串
    u -> u + 1, 權重 x
    u -> u - 1, 權重 x
    u -> u * 2, 權重 y

建一個節為 1 ~ 2N 的圖，跑節點 1 到節點 N 的最短路徑，該距離即為答案。
這個想法是對的，但一直 TLE/MLE，賽後看別人的程式碼，使用 spfa + implicit graph 就會過。
我猜 dijkstra 不會過可能是因為 pq 爆掉（MLE/TLE），因為 c++ 內建的 pq 沒辦法 decrease key…
以下也給出使用最短路徑的 AC Code，使用了 inqueue 陣列與 circular queue 優化到 500ms。

************************
AC Code 1
************************

.. code-block:: cpp
    :linenos:

    #include <bits/stdc++.h>
    using namespace std;

    typedef long long ll;
    const int MAX_N = 1e7;
    const ll INF = 1e18;

    int N; ll x, y;
    ll dp[MAX_N + 1];

    int main() {
        cin >> N >> x >> y;

        fill(dp + 1, dp + N + 1, INF);

        dp[1] = x;
        for (int i = 2; i <= N; i++) {
            dp[i] = min(dp[i], dp[i - 1] + x);
            if (i & 1)
                dp[i] = min(dp[i], dp[(i + 1) / 2] + y + x);
            else
                dp[i] = min(dp[i], dp[i / 2] + y);
        }

        cout << dp[N] << endl;

        return 0;
    }

************************
AC Code 2
************************

.. code-block:: cpp
    :linenos:

    #include <bits/stdc++.h>
    using namespace std;

    const int MAX_N = 1e7;
    int N; ll x, y;

    ll d[MAX_N * 2 + 1];
    bitset<MAX_N * 2 + 1> inq;

    int q[MAX_N * 2], qs = -1, qt = 0;
    inline void enqueue(int i) {
        q[qt++] = i;
        if (qt >= MAX_N * 2)
            qt = 0;
    }
    inline void dequeue() {
        qs++;
        if (qs >= MAX_N * 2)
            qs = 0;
    }
    inline int qfront() {
        return q[qs];
    }

    void spfa() {
        fill(d + 1, d + N * 2 + 1, ll(1e18));

        d[1] = x;
        inq[1] = true;
        enqueue(1);

        while (qs != qt) {
            int u = qfront(); dequeue();
            inq[u] = false;

            if (u < 2 * N && d[u + 1] > d[u] + x) {
                d[u + 1] = d[u] + x;
                if (!inq[u + 1]) {
                    inq[u + 1] = true;
                    enqueue(u + 1);
                }
            }
            if (u > 1 && d[u - 1] > d[u] + x) {
                d[u - 1] = d[u] + x;
                if (!inq[u - 1]) {
                    inq[u - 1] = true;
                    enqueue(u - 1);
                }
            }
            if (u * x < y) continue;
            if (u < N && d[u * 2] > d[u] + y) {
                d[u * 2] = d[u] + y;
                if (!inq[u * 2]) {
                    inq[u * 2] = true;
                    enqueue(u * 2);
                }
            }
        }
    }

    int main() {
        scanf("%d %lld %lld", &N, &x, &y);
        spfa();
        printf("%lld\n", d[N]);

        return 0;
    }
