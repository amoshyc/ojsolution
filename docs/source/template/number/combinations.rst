###################################################
排列組合
###################################################

.. sidebar:: Tags

    - ``tag_combination``
    - ``tag_template``

.. contents:: TOC
    :depth: 2

************************
程式碼
************************

.. code-block:: cpp
    :linenos:

    const int MAX_N = 1e6;
    const ll M = 1e9 + 7;
    ll fac[MAX_N + 1];

    ll fast_pow(ll a, ll b) {
        ll ans = 1;
        ll base = a % M;
        while (b) {
            if (b & 1)
                ans = ans * base % M;
            base = base * base % M;
            b >>= 1;
        }
        return ans;
    }

    void fac_init() {
        fac[0] = 1;
        for (int i = 1; i <= N; i++)
            fac[i] = fac[i - 1] * i % M;
    }

    ll inv(ll n) {
        return fast_pow(n, M - 2) % M;
    }

    ll C(ll a, ll b) {
        // a! / (b! * (a-b)!)
        return fac[a] * inv(fac[b] * fac[a - b]) % M;
    }

************************
模板驗證
************************

`cf181C <../../other/nctu_sc_enum/pg.html>`_
