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

    const int MAX_R = 20000;
    int R;
    ll bit[MAX_R + 1];

    int sum(int i) {
        ll s = 0;
        while (i > 0) {
            s += bit[i];
            i -= (i & -i);
        }
        return s;
    }

    void add(int i, ll x) {
        while (i <= R) {
            bit[i] += x;
            i += (i & -i);
        }
    }

************************
模板驗證
************************

`poj1990 <http://codepad.org/UeDMdncD>`_
