###################################################
[cfedu16] D. Two Arithmetic Progressions
###################################################

.. sidebar:: Tags

    - ``tag_crt``
    - ``tag_math``

.. contents:: TOC
    :depth: 2


******************************************************
`題目 <http://codeforces.com/contest/710/problem/D>`_
******************************************************

給定 a1, b1, a2, b2, L, R，請問 [L, R] 中有多少數 :math:`x`，滿足

.. math::

    x = a_1 q_1 + b_1 = a_2 q_2 + b_2 \text{ for some integers } q_1, q_2 \ge 0

************************
Specification
************************

::

    0 < a1, a2 <= 2 * 10^9
    -2 * 10^9 <= b1, b2, L, R <= 2 * 10^9
    L <= R

************************
分析
************************

.. note:: crt

該條件即等價於

.. math::

    x &\equiv b_1 \pmod {a_1} \\
    x &\equiv b_2 \pmod {a_2} \\
    L &= max(L, b1, b2)

前兩個式子可以用中國剩餘定理（擴展版本）合併。細節請參 `模版 <../../template/math/crt.html>`_
假設得到的結果是

.. math::

    x \equiv r \pmod {m}

這事實上就是一個等差數列

.. math::

    <m n + r> \quad (n \in \mathbb {Z})

問在 [L, R] 之間有幾項。
一個簡單的方法就是找 >= L 的最小項是誰，<= R 的最大項是誰。
假設分別是 s, t，那答案就是 (t - s) / m + 1，這是國中數學…

實作時，記得考慮，L, R 為負數與無解的情況。

************************
AC Code
************************

.. code-block:: cpp
    :linenos:

    #include <bits/stdc++.h>
    using namespace std;

    typedef long long ll;

    struct Item {
        ll m, r;
    };

    ll extgcd(ll a, ll b, ll& x, ll&y) {
        if (b == 0) {
            x = 1;
            y = 0;
            return a;
        }
        else {
            ll d = extgcd(b, a % b, y, x);
            y -= (a / b) * x;
            return d;
        }
    }

    Item extcrt(const vector<Item>& v) {
        ll m1 = v[0].m, r1 = v[0].r, x, y;

        for (int i = 1; i < int(v.size()); i++) {
            ll m2 = v[i].m, r2 = v[i].r;
            ll g = extgcd(m1, m2, x, y); // now x = (m/g)^(-1)

            if ((r2 - r1) % g != 0)
                return {-1, -1};

            ll k = (r2 - r1) / g * x % (m2 / g);
            k = (k + m2 / g) % (m2 / g); // for the case k is negative

            ll m = m1 * m2 / g;
            ll r = (m1 * k + r1) % m;

            m1 = m;
            r1 = (r + m) % m; // for the case r is negative
        }

        return (Item) {m1, r1};
    }

    int main() {
        ll a1, b1, a2, b2, L, R;
        scanf("%lld %lld %lld %lld %lld %lld", &a1, &b1, &a2, &b2, &L, &R);
        vector<Item> item;
        item.push_back((Item) {a1, (b1 % a1 + a1) % a1});
        item.push_back((Item) {a2, (b2 % a2 + a2) % a2});
        Item res = extcrt(item);

        if (res.m == -1) {
            puts("0");
            return 0;
        }

        L = max(L, max(b1, b2));
        ll s = (L - res.r) / res.m * res.m + res.r;
        ll t = (R - res.r) / res.m * res.m + res.r;

        if (s < L)
            s += res.m;
        if (t > R) // in case that R and t are nagative
            t -= res.m;

        if (s > t) {
            puts("0");
            return 0;
        }

        ll ans = (t - s) / res.m + 1;
        printf("%lld\n", ans);

        return 0;
    }
