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

    int N;
    bool is_prime[N];
    vector<int> primes;

    void init_primes() {
        fill(is_prime, is_prime + N, true);
        // build primes under N
        for (int i = 2; i < N; i++) {
            if (is_prime[i]) {
                primes.push_back(i);
                for (int j = i * i; j < N; j += i)
                    is_prime[j] = false;
            }
        }
    }

************************
模板驗證
************************

`poj2739 <http://codepad.org/Msof6Rk3>`_
