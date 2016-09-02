#########################################
[cf369] E. ZS and The Birthday Paradox
#########################################

.. sidebar:: Tags

    - ``tag_math``
    - ``tag_fast_pow``
    - ``tag_mod_inv``

.. contents:: TOC
    :depth: 2

******************************************************
`題目 <http://codeforces.com/contest/711/problem/E>`_
******************************************************

假設一年有 :math:`2^N` 天，現在 :math:`K` 人，求任兩人生日相同的機率
假設機率是 :math:`A/B` 且 :math:`gcd(A,B) = 1` 請輸出 :math:`A, B` 分別 :math:`\pmod {10^6 + 3}` 後的結果

************************
Specification
************************

::

    1 <= N <= 10^18
    2 <= K <= 10^18

************************
分析
************************

.. note:: 生日悖論

在 :math:`Z_{10^6 + 3}` 下，我們可以透過修正的操作與模反元素來進行減法與除法

.. code-block:: cpp

    ll mod_sub(ll a, ll b) { return (a - b + mod) % mod; }
    ll mod_div(ll a, ll b) { return a * inv(b) % mod; }

其中因為 :math:`10^6 + 3` 是質數，模反元素可以透過

.. code-block:: cpp

    ll inv(int a) {
        return mod_pow(a, mod - 2);
    }

計算之。所以底下所有只使用到四則運算的證明與操作都可以在模 :math:`10^6 + 3` 下完成。

---------------------------

=================
引理 1
=================

一個分數 :math:`1 - {b \over a} = {a - b \over a}` 是最簡分數
若且唯若 :math:`b \over a` 是最簡分數。

-------------

這是因為 :math:`gcd(a-b, a) = 1` 若且唯若 :math:`gcd(a, b) = 1`

=================
引理 2
=================

如果有最大的 :math:`m` 使得 :math:`2^m \mid x \quad(x \lt {2^n})`，
那 m 也是最大的數使得 :math:`2^m \mid {(2^n - x)}`，且反向亦成立。

----------------

假設正整數 :math:`k` 在二進制底下最右邊的 1 與其右邊的 0 記為 :math:`lowbit(k)`。
:math:`k` 的最大 :math:`2^m` 因數即為 :math:`lowbit(k)`，這是顯而易見的。

而因為（想想看二進制下直式減法）

.. math:: lowbit(x) = lowbit(2^n - x)

所以引理 2 成立。

=================
引理 3
=================

`Legendre's formula <https://en.wikipedia.org/wiki/Legendre%27s_formula>`_

.. math:: v_p(n!) = \frac{n - s_p(n)} { p - 1 }

其中

.. math::
    v_p(n!) &= max(v \in \Bbb{N}: p^v \mid n!) \\
    s_p(n) &= \text{ sum of the standard base-p digits of n}

=================
說明
=================

由 `生日悖論 <https://en.wikipedia.org/wiki/Birthday_problem>`_ 的公式可知答案的反面，所有人的生日都不同的機率為

.. math:: \bar{p} = \frac{(2^N - 1)(2^N - 2) \dots (2^N - (K - 1))} {2^{N (K - 1)}}

而任兩人相同的機率是 :math:`1 - \bar{p}` 。由引理 1 可知，我們只需將 :math:`\bar{p}` 化為最簡分數，再計算 :math:`1 - \bar{p}` 即可。

而這個式子分子分母的最大公因數 :math:`g`，由分母可知，會是 :math:`2^x \, (x \in \Bbb {N_{\ge 0}})` 這種形式。
而分子中最大的 :math:`2^x` 因數是多少呢？
由引理 2 可知，這相當於求 :math:`(k-1)!` 最大的形如 :math:`2^x` 的因數。
由引理 3 可知，這個數即為 :math:`2^{v_2} \text{ where } v_2 = (K - 1) - s_2(K - 1)`，
其中 :math:`s_2(K-1)` 就是算 :math:`K-1` 在二進位下的數字加總。

得到 :math:`g` 後，把分子分母都除以 :math:`g` 即可得到 :math:`\bar{p}` 的最簡形式。
即可進而求得答案。計算分母時，指數項可能會 overflow，可以利用費馬小定理優化一下。

---------------------------

一些情況可以特判掉，來確保時間複雜度

1.  :math:`K > 2^N` 根據鴿籠原理，解為 :math:`1/1`
2.  :math:`K - 1 >= 10^6 + 3`，分子為 0。這是因為

    .. math::

        (2^N - 1)(2^N - 2) \dots (2^N - (K - 1))

    根據鴿籠原理，這其中必有一項 :math:`mod \, (10^6 + 3)` 後為 0

************************
AC Code
************************

.. code-block:: cpp
    :linenos:

    #include <bits/stdc++.h>
    using namespace std;

    typedef long long ll;
    const ll mod = 1e6 + 3; // a prime

    ll N, K;

    ll mod_pow(ll a, ll b) {
        ll ans = 1;
        ll base = a;
        b %= (mod - 1);
        while (b) {
            if (b & 1)
                ans = ans * base % mod;
            base = base * base % mod;
            b >>= 1;
        }
        return ans;
    }

    ll mod_inv(ll a) {
        return mod_pow(a, mod - 2);
    }

    int main() {
        scanf("%lld %lld", &N, &K);

        if (N <= 62 && K > (1ll << N)) {
            puts("1 1");
            return 0;
        }

        // numerator
        ll nmr = 1;
        if (K - 1 >= mod) nmr = 0;
        else {
            ll val = mod_pow(2, N);
            for (int i = 1; i <= K - 1; i++) {
                ll item = (val - i + mod) % mod;
                (nmr *= item) %= mod;
            }
        }

        // denominator
        ll a = N % (mod - 1);
        ll b = (K - 1) % (mod - 1);
        ll dnm = mod_pow(2, a * b);

        // gcd
        ll v2 = (K - 1) - __builtin_popcountll(K - 1);
        ll inv = mod_inv(mod_pow(2, v2));

        // printf("%lld, %lld, %lld, %lld\n", nmr, dnm, inv, v2);

        (nmr *= inv) %= mod;
        (dnm *= inv) %= mod;
        (nmr = dnm - nmr + mod) %= mod;

        printf("%lld %lld\n", nmr, dnm);

        return 0;
    }
