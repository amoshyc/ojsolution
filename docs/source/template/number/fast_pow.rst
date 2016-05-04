###################################################
快速冪
###################################################

.. sidebar:: Tags

    - ``tag_fast_pow``
    - ``tag_matrix``
    - ``tag_template``

.. contents:: TOC
    :depth: 2

************************
程式碼
************************

.. code-block:: cpp
    :linenos:

    ll fast_pow(ll a, ll b) {
        ll ans = 1;
        ll base = a;
        while (b) {
            if (b & 1) ans *= base;
            base = base * base;
            b >>= 1;
        }
        return ans;
    }

************************
模板驗證
************************

`poj2739 <../../poj/p3233.html>`_
