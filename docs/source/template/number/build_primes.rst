###################################################
質數表
###################################################

.. sidebar:: Tags

    - ``tag_prime``
    - ``tag_template``

.. contents:: TOC
    :depth: 2

************************
程式碼
************************

.. code-block:: cpp
    :linenos:

    ll MAX_NUM;
    bool is_prime[N];
    vector<ll> primes;

    void init_primes() {
        fill(is_prime, is_prime + MAX_NUM, true);
        // build primes under N
        for (ll i = 2; i < MAX_NUM; i++) {
            if (is_prime[i]) {
                primes.push_back(i);
                for (ll j = i * i; j < MAX_NUM; j += i)
                    is_prime[j] = false;
            }
        }
    }

************************
模板驗證
************************

`poj2739 <http://codepad.org/Msof6Rk3>`_
