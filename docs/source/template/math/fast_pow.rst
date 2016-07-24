###################################################
快速冪
###################################################

.. sidebar:: Tags

    - ``tag_fast_pow``
    - ``tag_template``

.. contents:: TOC
    :depth: 2

************************
程式碼
************************

.. code-block:: cpp
    :linenos:

    const ll MOD = 1e9 + 7;

    // a^b
    ll fast_pow(ll a, ll b) {
        // b %= (MOD - 1) // if MOD is prime, Fermat's little theorem
        ll ans = 1;
        ll base = a % MOD;
        while (b) {
            if (b & 1) ans = ans * base % MOD;
            base = base * base % MOD;
            b >>= 1;
        }
        return ans;
    }

************************
模板驗證
************************

`poj2739 <../../poj/p3233.html>`_
