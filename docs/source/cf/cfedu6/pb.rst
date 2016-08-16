#############################################
[cfedu6] B. Grandfather Dovlet’s calculator
#############################################

.. sidebar:: Tags

    - ``tag_easy``

.. contents:: TOC
    :depth: 2


******************************************************
`題目 <http://codeforces.com/contest/620/problem/B>`_
******************************************************

************************
Specification
************************

************************
分析
************************

.. note:: 水題

************************
AC Code
************************

.. code-block:: cpp
    :linenos:

    #include <bits/stdc++.h>
    using namespace std;

    const int a[10] = {6, 2, 5, 5, 4, 5, 6, 3, 7, 6};

    int get_total(int n) {
        int temp = n;
        int cnt = 0;

        while (temp != 0) {
            cnt += a[temp % 10];
            temp /= 10;
        }

        return cnt;
    }

    int main() {
        int a, b;
        scanf("%d %d", &a, &b);

        int ans = 0;
        for (int i = a; i <= b; i++)
            ans += get_total(i);

        printf("%d\n", ans);

        return 0;
    }
