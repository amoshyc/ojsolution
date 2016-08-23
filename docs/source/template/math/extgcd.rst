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

    // 求取讓 a*x + b*y = gcd(a, b) 的整數解 (x, y)
    // 同時回傳 gcd(a, b)
    int extgcd(int a, int b, int& x, int& y) {
        if (b != 0) {
            d = extgcd(b, a % b, y, x);
            y -= (a / b) * x;
            return d;
        }
        else {
            x = 1;
            y = 0;
            return a;
        }
    }

************************
模板驗證
************************

待補
