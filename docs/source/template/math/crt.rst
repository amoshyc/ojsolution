###################################################
Chinese Remainder Theorem
###################################################

.. sidebar:: Tags

    - ``tag_crt``
    - ``tag_template``

.. contents:: TOC
    :depth: 2

************************
程式碼
************************

(m[i] 彼此可以非互質)

.. code-block:: cpp
    :linenos:

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


************************
原理
************************

====================
引理 1
====================

如果 :math:`gcd(a, n) = 1`，那 :math:`extgcd(a, n, x, y)` 得到的 :math:`x` 即為 :math:`a` 在 :math:`Z_n` 底下的反元素

-------------------------

證明:

:math:`extgcd(a, n, x, y)` 是求

.. math::

    a x + n y &= gcd(a, n) = 1 \\
    a x &= - n y + 1 \\
    a x &\equiv 1 \pmod{n}

的一組解 :math:`(x, y)`，由最後一個式子可知，:math:`x` 即為 :math:`a^{-1} \pmod{n}`

====================
引理 2
====================

假設 :math:`g = gcd(a, n)`，那 :math:`extgcd(a, n, x, y)` 得到的 :math:`x` 即為 :math:`a/g` 在 :math:`Z_{n/g}` 底下的反元素

-----------------------

證明:

:math:`extgcd(a, n, x, y)` 是求

.. math::

    a x + n y &= gcd(a, n) = g \\[1ex]
    \frac{a}{g}x + \frac{n}{g}y &= 1

的一組解 :math:`(x, y)`，由引理 1 可知，:math:`x` 即為 :math:`\left(\frac{a}{g}\right)^{-1} \pmod{\frac{n}{g}}`

====================
引理 3
====================

給定一元線性同餘方程

.. math::

    \begin{cases}
    x \equiv r_1 \pmod {m_1} \\
    x \equiv r_2 \pmod {m_2}
    \end{cases}

解應為

.. math::

    x &\equiv m_1 K + r_1 \pmod {\frac{m_1 m_2}{g}} \\[1ex]
    where \quad K &= \left(\frac{m_1}{g}\right)^{-1} \, \frac{r_2 - r_1}{g} \\[1ex]

解是否存在可由 :math:`gcd(m_1, m_2) \mid (r_2 - r_1)` 判定

-----------------

證明：

由定義可知

.. math::

    x &= m_1 * k_1 + r_1 \quad (k_1 \in \mathbb{Z}) \\
    x &= m_2 * k_2 + r_2 \quad (k_2 \in \mathbb{Z}) \\
    m_1 k_1 + r_1 &= m_2 k_2 + r_2 \\
    m_1 k_1 &= m_2 k_2 + (r_2 - r_1)

把 :math:`m_1, m_2` 的公因數除掉，讓 :math:`m_1/g,\, m_2/g` 互質

.. math::

    Let \quad g &= gcd(m_1, m_2) \\[1ex]
    \frac{ m_1 k_1}{ g } &= \frac{ m_2 k_2 }{ g } + \frac{ r2 - r1 }{ g } \\[1ex]
    \frac{ m_1 }{ g } k_1 &\equiv \frac{ r2 - r1 }{ g } \pmod { \frac{ m_2 }{ g } }

明顯地，如果 :math:`\frac{r2 - r1}{g}` 不為整數則無解。利用模反元素，可得

.. math::

    k_1 &\equiv \left(\frac{m1}{g}\right)^{-1} \, \frac{r2 - r1}{g} \pmod {\frac{m_2}{g}} \\[1ex]
    Let \quad K &= \left(\frac{m_1}{g}\right)^{-1} \, \frac{r2 - r1}{g} \\[1ex]
    k_1 &\equiv K \pmod {\frac{m_2}{g}} \\[1ex]
    k_1 &= \frac{m_2}{g} q + K \quad (q \in \mathbb{Z})

知道了 :math:`k_1`，即可求得 :math:`x`

.. math::

    x &= m_1 k_1 + r_1 \\[1ex]
      &= m_1 (\frac{m_2}{g} q + K) + r_1 \\[1ex]
      &= \frac{m_1 m2}{g} q + m_1 K + r_1 \\[1ex]
      &\equiv m_1 K + r_1 \pmod {\frac{m_1 m_2}{g}}

所以解即為：

.. math::

    x &\equiv m_1 K + r_1 \pmod {\frac{m_1 m_2}{g}} \\[1ex]
    where \quad K &= \left(\frac{m_1}{g}\right)^{-1} \, \frac{r_2 - r_1}{g}

其中模反元素 :math:`\left(\frac{m_1}{g}\right)^{-1}` 可由引理 1 或引理 2 計算。

====================
演算法
====================

給定 :math:`n` 個一元線性同餘方程

.. math::

    \begin{cases}
    x \equiv r_1 \pmod {m_1} \\
    x \equiv r_2 \pmod {m_2} \\
    \dots \\
    x \equiv r_n \pmod {m_n}
    \end{cases}

我們可以利用引理 3，不斷地把兩個方程合併成一個，直到剩下一個方程式為止，而該方程即為解。

----------------------

這個網頁最上面那個模版是使用引理 2 來求反元素，以下給出使用引理 1 求反元素的模版

.. code-block:: cpp
    :linenos:

    typedef long long ll;

    struct Item {
        ll m, r;
    };

    ll gcd(ll a, ll b) {
        if (b == 0)
            return a;
        return gcd(b, a % b);
    }

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
            ll g = gcd(m1, m2);

            if ((r2 - r1) % g != 0)
                return {-1, -1};

            extgcd(m1 / g, m2 / g, x, y); // now x is (m1 / g)^(-1)

            ll k = (r2 - r1) / g * x % (m2 / g);
            k = (k + m2 / g) % (m2 / g); // for the case k is negative

            ll m = m1 * m2 / g;
            ll r = (m1 * k + r1) % m;

            m1 = m;
            r1 = (r + m) % m; // for the case r is negative
        }

        return (Item) {m1, r1};
    }


************************
模板驗證
************************

`poj 2891 <../../poj/p2891.html>`_
