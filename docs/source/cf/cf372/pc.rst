#####################################
[cf372] C. Sonya and Queries
#####################################

.. sidebar:: Tags

    - ``tag_math``

.. contents:: TOC
    :depth: 2

******************************************************
`題目 <http://codeforces.com/contest/716/problem/C>`_
******************************************************

這個遊戲有 N + 1 個關卡、兩個按鍵 "+", "√"，遊戲積分一開始為 2，你在關卡 1。
假設你在關卡 k，你可以按任意次 "+" 按鈕，每按一次，遊戲積分加 k。
如果你按 "√"，積分會變為原本值的平方根，然後你會到下一關。
但有二個限制：
1. 當你一到下一個關卡 k + 1 時，你的積分必需是 k + 1 的倍數。
2. 當你按下 "√" 時，積份必需是完全平方數。

請問若你想到第 N + 1 個關卡，在各個關卡中，在按 "√" 前需要按幾次 "+"？
該數字不能超過 10^18。不用找最少的次數，只要找到一組可以解。

************************
Specification
************************

::

    1 <= N <= 10^5

************************
分析
************************

.. note:: 數學

看看範例一或三可以發現::

    Level 4: 12 + 4 * 97 = 400 = 20 ** 2 = (5 * 4) ** 2
    Level 3:  6 + 3 * 46 = 144 = 12 ** 2 = (4 * 3) ** 2
    Level 2:  4 + 2 * 16 = 36  = 6 ** 2  = (3 * 2) ** 2
    Level 1:  2 + 1 * 14 = 16  = 4 ** 2

可以發現除了 Level 1，其它關卡有規律，然後用程式驗證一下。
於是就可以寫了，但計算過去中會爆 long long，有兩個解決方式：

1.  改用 python
2.  優化式子，在第 k 層 (k >= 3) 有以下式子::

        Level k: (k - 1) * k + k * ans = (k * (k + 1)) ** 2

    同除 k，得到::

        ans = k * (k + 1) * (k + 1) - (k - 1)

    這樣計算就不會爆 long long 了。

************************
AC Code 1 (python 3)
************************

.. code-block:: python3
    :linenos:

    N = int(input())

    ans = {}
    ans[1] = 14

    num = 4
    for level in range(2, N + 1):
        val = (level * (level + 1)) ** 2
        ans[level] = (val - num) // level
        num = (level * (level + 1))

    for i in range(1, N + 1):
        print(ans[i])

************************
AC Code 2 (c++)
************************

.. code-block:: cpp
    :linenos:

    #include <bits/stdc++.h>
    using namespace std;

    typedef long long ll;

    int main() {
        int N;
        scanf("%d", &N);

        vector<ll> ans(N + 1, 0);
        ans[1] = 14;
        ans[2] = 16;

        for (ll k = 3; k <= N; k++) {
            ans[k] = k * (k + 1) * (k + 1) - (k - 1);
        }

        for (int i = 1; i <= N; i++)
            printf("%lld\n", ans[i]);

        return 0;
    }
