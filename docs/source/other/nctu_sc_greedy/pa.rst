#####################################
[cf228] A. Fox and Number Game
#####################################

.. sidebar:: Tags

    - ``tag_gcd``

.. contents:: TOC
    :depth: 2

*************************************************************
`題目 <http://codeforces.com/problemset/problem/389/A>`_
*************************************************************

************************
Specification
************************


************************
分析
************************

.. note:: 水題

寫過的題目…
題目描述的過程事實上就是求所有數的 gcd…

************************
AC Code
************************

.. code-block:: cpp
    :linenos:

    #include <bits/stdc++.h>
    using namespace std;

    int gcd(int a, int b) {
        while (b) {
            // a, b = b, a % b
            int temp = a % b;
            a = b;
            b = temp;
        }
        return a;
    }

    int main() {
        int N;
        scanf("%d", &N);

        int ans; scanf("%d", &ans);
        for (int i = 1; i < N; i++) {
            int inp; scanf("%d", &inp);
            ans = gcd(ans, inp);
        }

        printf("%d\n", ans * N);

        return 0;
    }
