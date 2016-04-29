###################################################
D. Polynomials
###################################################

.. sidebar:: Tags

    - ``tag_easy``

.. contents:: TOC
    :depth: 2


*******************************************************************************
`題目 <http://e-tutor.itsa.org.tw/e-Tutor/mod/programming/view.php?id=23649>`_
*******************************************************************************


************************
分析
************************

.. note:: 水題

d 很小，直接展開即可。

************************
AC Code
************************

.. code-block:: cpp
    :linenos:

    #include <iostream>
    #include <vector>
    #include <algorithm>
    using namespace std;

    typedef long long ll;

    int main() {
        ios::sync_with_stdio(false);
        cin.tie(0);

        int TC; cin >> TC;
        while (TC--) {
            ll a, b, c, d;
            cin >> a >> b >> c >> d;

            if (d == 1) {
                cout << a << " " << b << " " << c << "\n";
                continue;
            }

            if (d == 2) {
                cout << a*a << " "
                     << (2 * a * b) << " "
                     << (2 * a * c + b * b) << " "
                     << (2 * b * c) << " "
                     << c * c << "\n";
                continue;
            }

            cout << (a * a * a) << " "
                 << (3 * a * a * b) << " "
                 << (3 * a * a * c + 3 * a * b * b) << " "
                 << (6 * a * b * c + b * b * b) << " "
                 << (3 * a * c * c + 3 * b * b * c) << " "
                 << (3 * b * c * c) << " "
                 << (c * c * c) << "\n";
        }
        return 0;
    }
