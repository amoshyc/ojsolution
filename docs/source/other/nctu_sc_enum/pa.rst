#####################################
[cf138] A. Parallelepiped
#####################################

.. sidebar:: Tags

    - ``tag_enum``
    - ``tag_easy``

.. contents:: TOC
    :depth: 2


******************************************************
`題目 <http://codeforces.com/contest/224/problem/A>`_
******************************************************

************************
Specification
************************


************************
分析
************************

.. note:: 水題

枚舉其中一個邊，其它兩個邊的值就確定了。

************************
AC Code
************************

.. code-block:: cpp
    :linenos:

    #include <bits/stdc++.h>
    using namespace std;

    int gcd(int a, int b) {
        while (b) {
            int temp = a % b;
            a = b;
            b = temp;
        }
        return a;
    }

    int main() {
        int A1, A2, A3;
        cin >> A1 >> A2 >> A3;

        int g = gcd(A1, A2);
        for (int l1 = 1; l1 <= g; l1++) {
            if (g % l1 != 0) continue;
            int l2 = A1 / l1;
            int l3 = A2 / l1;
            if (l2 * l3 == A3) {
                cout << 4 * (l1 + l2 + l3) << endl;
                return 0;
            }
        }

        return 0;
    }
