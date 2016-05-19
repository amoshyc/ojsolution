#####################################
[cf346] A. Round House
#####################################

.. sidebar:: Tags

    - ``tag_easy``

.. contents:: TOC
    :depth: 2


******************************************************
`題目 <http://codeforces.com/contest/659/problem/A>`_
******************************************************

N 個門圍成一圈，給定初使位置與移動方向即步數 b，問最後停在哪個門？

************************
Specification
************************

::

    1 <= N <= 100
    -100 <= b <= 100


************************
分析
************************

.. note:: 水題

直接看 code。


************************
AC Code
************************

.. code-block:: cpp
    :linenos:

    #include <bits/stdc++.h>
    using namespace std;

    int main() {
        int n, a, b;
        cin >> n >> a >> b;
        int i = a - 1;
        if (b < 0) {
            i -= -b;
            while (i < 0)
                i += n;
        }
        else {
            i += b;
            i %= n;
        }

        cout << i+1 << endl;

        return 0;
    }
