###################################################
A. Square and Cube
###################################################

.. sidebar:: Tags

    - ``tag_easy``

.. contents:: TOC
    :depth: 2


*******************************************************************************
`題目 <http://e-tutor.itsa.org.tw/e-Tutor/mod/programming/view.php?id=23646>`_
*******************************************************************************


************************
分析
************************

.. note:: 水題


************************
AC Code
************************

.. code-block:: cpp
    :linenos:

    #include <iostream>
    #include <algorithm>
    using namespace std;

    int main() {
        int TC; cin >> TC;
        while (TC--) {
            long long a;
            cin >> a;
            cout << a * a << " " << a * a * a << "\n";
        }
        return 0;
    }
