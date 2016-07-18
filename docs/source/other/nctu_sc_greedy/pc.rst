#####################################
[cf236] A. Nuts
#####################################

.. sidebar:: Tags

    - ``tag_greedy``

.. contents:: TOC
    :depth: 2

************************************************************
`題目 <http://codeforces.com/problemset/problem/402/A>`_
************************************************************

************************
Specification
************************


************************
分析
************************

.. note:: Greedy

一個貪心的想法::

    箱子能裝就裝

************************
AC Code
************************

.. code-block:: cpp
    :linenos:

    #include <bits/stdc++.h>
    using namespace std;

    int K, A, B, V;

    int main() {
        scanf("%d %d %d %d", &K, &A, &B, &V);

        int cnt = 0;
        int divi = B;

        int ans = -1;
        for (int box = 1; ; box++) {
            int sec = 1;
            if (divi >= K) {
                sec = K;
                divi -= K - 1;
            }
            else if (divi > 0) {
                sec = divi + 1;
                divi = 0;
            }

            cnt += sec * V;

            // printf("%d: %d, %d\n", box, sec, cnt);

            if (cnt >= A) {
                ans = box;
                break;
            }
        }

        printf("%d\n", ans);

        return 0;
    }
