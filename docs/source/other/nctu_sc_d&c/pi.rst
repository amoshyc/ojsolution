#####################################
[cf240] B. Mashmokh and Tokens
#####################################

.. sidebar:: Tags

    - ``tag_math``

.. contents:: TOC
    :depth: 2

******************************************************
`題目 <http://codeforces.com/contest/415/problem/B>`_
******************************************************

:math:`w` 個 tokens 可以換成 :math:`\floor{\frac{w * a}{b} }` 元。
給定長度為 N 的序列 x[]，代表 N 天。針對每一天，請問在保證獲得最大金錢的情況下，最多會剩下多少個 tokens？

************************
Specification
************************

::

    1 <= n <= 10^5
    1 <= a, b <= 10^9

************************
分析
************************

.. note:: 數學

坑死人啊～

:math:`w` 個 tokens 可以換成 :math:`\lfloor \frac{w * a}{b} \rfloor` 元，那要得到 :math:`d` 元，要用幾個 tokens 呢？答案是 :math:`\lceil \frac{d * b}{a} \rceil` ──不是 :math:`\lfloor \frac{d * b}{a} \rfloor` 啊！

大家應該都死很慘。

************************
AC Code
************************

.. code-block:: cpp
    :linenos:

    #include <bits/stdc++.h>
    using namespace std;

    typedef long long ll;

    ll ceildiv(ll a, ll b) {
        return a / b + (a % b != 0);
    }

    int main() {
        ios::sync_with_stdio(false);
        cin.tie(0);
        cout.tie(0);

        int N; ll a, b;
        cin >> N >> a >> b;

        for (int i = 0; i < N; i++) {
            ll x; cin >> x;
            ll money = x * a / b; // floor
            ll rest = x - ceildiv(money * b, a);
            // ll rest = x - ll(ceil(double(money) * b / a));
            cout << rest << " ";
        }
        cout << endl;

        return 0;
    }
