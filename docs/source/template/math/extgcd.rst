###################################################
Extgcd
###################################################

.. sidebar:: Tags

    - ``tag_extgcd``
    - ``tag_template``

.. contents:: TOC
    :depth: 2

************************
程式碼
************************

.. code-block:: cpp
    :linenos:

    // 求取讓 a*x + b*y = 1 的整數解 (x, y)
    int extgcd(int a, int b, int& x, int& y) {
        int d = a;
        if (b != 0) {
            d = extgcd(b, a % b, y, x);
            y -= (a / b) * x;
        }
        else {
            x = 1;
            y = 0;
        }
        return d;
    }

************************
模板驗證
************************

待補
