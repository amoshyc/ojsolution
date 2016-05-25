#####################################
[cf353] B. Restoring Painting
#####################################

.. sidebar:: Tags

    - ``tag_math``

.. contents:: TOC
    :depth: 2


******************************************************
`題目 <http://codeforces.com/contest/675/problem/B>`_
******************************************************

給定 3x3 矩陣的 a, b, c, d 值，請問在::

    1. 每一元素的範圍都是 [1, N]
    2. 矩陣列任意的 2x2 子矩陣的和都與左上角那個 2x2 子矩陣的和一樣。

的限制下，tuple (x, y, z, w, u) 有幾種可能。

.. math::

    \begin{bmatrix} x & a & y \\ b & z & c \\ w & d & u \end{bmatrix}

************************
Specification
************************

::

    1 <= N <= 10^5
    1 <= a, b, c, d <= N

************************
分析
************************

.. note:: 好好觀察

關於限制 2，可以發現事實上並沒有對 z 做出任何限制。然後我們也可以列出三個等式

.. math::

    x + b &= y + c \\
    x + a &= w + d \\
    y + a &= d + u

移位後，可以發現 (y, w, u) 都是 x 再加上某個定值

.. math::

    y &= x + b - c \\
    w &= x + a - d \\
    u &= y + a - d

這代表了，我們只要枚舉 x，當 x 的值固定時，(y, w, u) 都會是定值。
每枚舉出一個 (x, y, w, u) 再檢查 (y, w, u) 有沒有滿足限制 1。
因為 z 沒被限制，1 ~ N 都可以，所以答案是 (x, y, w, u) 的個數再乘上 N

************************
AC Code
************************

.. code-block:: cpp
    :linenos:

    #include <bits/stdc++.h>
    using namespace std;

    typedef long long ll;

    int main() {
        ios::sync_with_stdio(false);
        cin.tie(0);

        ll n, a, b, c, d;
        cin >> n >> a >> b >> c >> d;

        ll cnt = 0;
        for (ll x = 1; x <= n; x++) {
            ll y = x + (b - c);
            ll w = x + (d - a);
            ll u = y + (d - a);

            if (1 <= y && y <= n &&
                1 <= w && w <= n &&
                1 <= u && u <= n)
                cnt++;
        }

        cout << cnt * n << "\n";

        return 0;
    }
