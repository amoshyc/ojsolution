###################################################
[Template] BIT 2d
###################################################

.. sidebar:: Tags

    - ``tag_bit``

.. contents:: TOC
    :depth: 2

************************
程式碼
************************

.. code-block:: cpp
    :linenos:

    ll bit[MAX_N + 1][MAX_N + 1]; // index start from 1

    ll sum(int a, int b) {
        ll s = 0;
        for (int i = a; i > 0; i -= (i & -i))
            for (int j = b; j > 0; j -= (j & -j))
                s += bit[i][j];
        return s;
    }

    void add(int a, int b, int x) {
        for (int i = a; i <= N; i += (i & -i))
            for (int j = b; j <= N; j += (j & -j))
                bit[i][j] += x;
    }

************************
模板驗證
************************

`poj2155 <https://ideone.com/5onr93>`_
