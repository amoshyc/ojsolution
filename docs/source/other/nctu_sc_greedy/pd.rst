#####################################
[cfother] A. Poster
#####################################

.. sidebar:: Tags

    - ``tag_greedy``

.. contents:: TOC
    :depth: 2

**********************************************************
`題目 <http://codeforces.com/problemset/problem/412/A>`_
**********************************************************

************************
Specification
************************

************************
分析
************************

.. note:: Greedy

一個貪心的想法::

    選路徑長較短的先走，再往另一個方向走

************************
AC Code
************************

.. code-block:: cpp
    :linenos:

    #include <bits/stdc++.h>
    using namespace std;

    int N, K;
    string inp;

    int main() {
        cin >> N >> K;
        cin >> inp;

        int left_cnt = K - 1;
        int right_cnt = N - K;
        if (left_cnt < right_cnt) {
            for (int i = K; i > 1; i--) {
                cout << "LEFT\n";
            }
            for (int i = 1; i <= N; i++) {
                if (i != 1)
                    cout << "RIGHT\n";
                cout << "PRINT " << inp[i - 1] << "\n";
            }
        }
        else {
            for (int i = K; i < N; i++) {
                cout << "RIGHT\n";
            }
            for (int i = N; i >= 1; i--) {
                if (i != N)
                    cout << "LEFT\n";
                cout << "PRINT " << inp[i - 1] << "\n";
            }
        }

        return 0;
    }
