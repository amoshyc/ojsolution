#####################################
[cf102] A. Help Vasilisa the Wise 2
#####################################

.. sidebar:: Tags

    - ``tag_enum``
    - ``tag_easy``

.. contents:: TOC
    :depth: 2


******************************************************
`題目 <http://codeforces.com/contest/143/problem/A>`_
******************************************************

************************
Specification
************************

************************
分析
************************

.. note:: 枚舉

最水的那種，四個 for 開下去即可

************************
AC Code
************************

.. code-block:: cpp
    :linenos:

    #include <bits/stdc++.h>
    using namespace std;

    int main() {
        int c1, c2, d1, d2, r1, r2;
        scanf("%d%d%d%d%d%d", &r1, &r2, &c1, &c2, &d1, &d2);

        for (int a = 1; a <= 9; a++) {
            for (int b = 1; b <= 9; b++) {
                for (int c = 1; c <= 9; c++) {
                    for (int d = 1; d <= 9; d++) {
                        if (a == b || a == c || a == d ||
                            b == c || b == d ||
                            c == d) continue;

                        if (a + c == c1 &&
                            b + d == c2 &&
                            a + b == r1 &&
                            c + d == r2 &&
                            a + d == d1 &&
                            b + c == d2) {
                                printf("%d %d\n", a, b);
                                printf("%d %d\n", c, d);
                                return 0;
                            }
                    }
                }
            }
        }

        puts("-1");

        return 0;
    }
