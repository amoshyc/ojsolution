###################################################
C. Apple trees in a line
###################################################

.. sidebar:: Tags

    - ``tag_dp``

.. contents:: TOC
    :depth: 2

*******************************************************************************
`題目 <http://e-tutor.itsa.org.tw/e-Tutor/mod/programming/view.php?id=23189>`_
*******************************************************************************

************************
分析
************************

.. note:: 水題

看懂題目的話就會知道是最大連續區間和，大家應該都寫過，這題應該要 O(N) 才能過。

************************
AC Code
************************

.. code-block:: cpp
    :linenos:

    #include <iostream>
    #include <algorithm>
    #include <vector>
    #include <cstdio>

    using namespace std;

    int main() {
        // ios::sync_with_stdio(false);

        int N;
        while (scanf("%d", &N)) {
            if (N == 0) break;

            int max_ = 0;
            int max_ending_here = 0;
            for (int i = 0; i < N; i++) {
                int inp;
                scanf("%d", &inp);

                max_ending_here = max(0, max_ending_here + inp);
                max_ = max(max_, max_ending_here);
            }

            printf("%d\n", max_);
        }

        return 0;
    }
