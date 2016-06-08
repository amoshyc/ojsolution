###################################################
BIT
###################################################

.. sidebar:: Tags

    - ``tag_bit``
    - ``tag_template``

.. contents:: TOC
    :depth: 2

************************
程式碼
************************

.. code-block:: cpp
    :linenos:

    const int MAX_N = 20000;
    ll bit[MAX_N + 1];

    int sum(int i) {
        ll s = 0;
        while (i > 0) {
            s += bit[i];
            i -= (i & -i);
        }
        return s;
    }

    void add(int i, ll x) {
        while (i <= MAX_N) {
            bit[i] += x;
            i += (i & -i);
        }
    }

************************
模板驗證
************************

`poj1990 <http://codepad.org/UeDMdncD>`_
