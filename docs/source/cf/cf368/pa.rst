#####################################
[cf368] A. Brain's Photos
#####################################

.. sidebar:: Tags

    - ``tag_easy``

.. contents:: TOC
    :depth: 2

******************************************************
`題目 <http://codeforces.com/contest/707/problem/A>`_
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

    int main() {
        int N, M;
        scanf("%d %d", &N, &M);

        bool ok = true;
        for (int i = 0; i < N; i++) {
            for (int j = 0; j < M; j++) {
                char inp[5];
                scanf("%s", inp);

                if (inp[0] == 'C' || inp[0] == 'M' || inp[0] == 'Y')
                    ok = false;
            }
        }

        puts((ok) ? "#Black&White" : "#Color");

        return 0;
    }
